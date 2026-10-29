---
title:  Spring-Cloud-Gateway-路由定位器RouteLocator源码解析
date: 2026-01-27
tags:
	- 项目实战
    - 精选原创
	- java
	- Spring Cloud Gateway
	- 原创
categories: Spring生态
cover: https://img1.baidu.com/it/u=990940841,655710864&fm=253&fmt=auto?w=1025&h=489
---



# 前言

在整理和优化 星云网关 的时候，顺便仔细看了看Spring Cloud Gateway源码



# 网关代码

```java

/**
 * 动态路由定义定位器
 */
@Component("dynamicRouteDefinitionLocator")
public class DynamicRouteDefinitionLocator implements RouteDefinitionLocator {
    private static final Logger logger = LoggerFactory.getLogger(DynamicRouteDefinitionLocator.class);

    private final RedisRouteDefinitionRepository repository;
    private final ApplicationEventPublisher eventPublisher;

    public DynamicRouteDefinitionLocator(RedisRouteDefinitionRepository repository,ApplicationEventPublisher eventPublisher) {
        this.repository = repository;
        this.eventPublisher = eventPublisher;
    }

    @Override
    public Flux<RouteDefinition> getRouteDefinitions() {
        return repository.getRouteDefinitions();  // 返回从Redis获取的路由定义
    }

    // 外部可调用的缓存刷新方法，并推送路由更新消息
    public Mono<Void> refreshCache() {
        return repository.refreshRoutes().then(Mono.fromRunnable(() ->
                eventPublisher.publishEvent(new RefreshRoutesEvent(this))));
    }
}
```

因为此篇博客关注点在于源码解析，所以网关的具体代码实现先不讲。



## 网关代码和Spring Cloud Gateway的关系

- `DynamicRouteDefinitionLocator`实现了 `RouteDefinitionLocator` 接口
- 通过 getRouteDefinitions() 方法向Spring Cloud Gateway提供路由配置



## 引起的思考

我们自定义的`DynamicRouteDefinitionLocator`类，它实现了RouteDefinitionLocator接口，那么自定义类是如何与Spring Cloud Gateway的核心路由加载机制相结合的呢。



# 源码解析

先说结论：整个路由加载的链路可以表示如下：

```java
请求匹配/路由刷新事件
    ↓
CachingRouteLocator.getRoutes()        // 缓存层，性能优化
    ↓
CompositeRouteLocator.getRoutes()      // 组合器，聚合多个RouteLocator
    ↓
RouteDefinitionRouteLocator.getRoutes() // 核心路由转换器
    ↓
RouteDefinitionLocator.getRouteDefinitions() // 路由定义获取接口
    ↓
CompositeRouteDefinitionLocator.getRouteDefinitions() // Spring默认的组合器
    ↓
DynamicRouteDefinitionLocator.getRouteDefinitions()  // 我们的自定义实现
    ↓
RedisRouteDefinitionRepository.getRouteDefinitions() //  我们的自定义实现-Redis存储层
```



## 关键链路源码分析

### CachingRouteLocator 

CachingRouteLocator是路由请求的入口点，它实现了缓存机制以提升性能。

<u>外部调用getRoutes()方法，从而构造函数执行。</u>

#### getRoutes()方法

```java
	@Override
	public Flux<Route> getRoutes() {
		return this.routes;
	}
```

#### 构造函数

```java
public CachingRouteLocator(RouteLocator delegate) {
		this.delegate = delegate;
		routes = CacheFlux.lookup(cache, CACHE_KEY, Route.class).onCacheMissResume(this::fetch);
	}

```

- CacheFlux.lookup():在cache 中查找键为 CACHE_KEY ("routes") 的路由数据，当缓存中不存在数据时，通过onCacheMissResume触发fetch()方法。

  并且它是懒加载，构造时不立刻加载数据，首次访问时从 delegate 获取数据并缓存，后续访问直接从缓存返回。

- 它接受一个`RouteLocator`类型的`delegate`参数

  同时我们可以在GatewayAutoConfiguration，这个delegate实际上是`CompositeRouteLocator`实例。

  ```java
  @Bean
  @Primary
  @ConditionalOnMissingBean(name = "cachedCompositeRouteLocator")
  // TODO: property to disable composite?
  public RouteLocator cachedCompositeRouteLocator(List<RouteLocator> routeLocators) {
  return new CachingRouteLocator(new CompositeRouteLocator(Flux.fromIterable(routeLocators)));
  }
  
  ```

#### fetch() 方法

调用`delegate.getRoutes()`来获取路由，即委托给`CompositeRouteLocator`。

```java
private Flux<Route> fetch() {
    return this.delegate.getRoutes().sort(AnnotationAwareOrderComparator.INSTANCE);
}
```



### CompositeRouteLocator

`CompositeRouteLocator`负责聚合多个`RouteLocator`实例，提供一个统一的路由流。

#### 构造函数

接受一个`Flux<RouteLocator>`类型的`delegates`参数，该参数包含了所有具体的`RouteLocator`Bean（如`RouteDefinitionRouteLocator`）

```java
public CompositeRouteLocator(Flux<RouteLocator> delegates) {
	this.delegates = delegates;
}

```



#### getRoutes() 方法

```java
@Override
public Flux<Route> getRoutes() {
	return this.delegates.flatMapSequential(RouteLocator::getRoutes);
}
```

使用`flatMapSequential`顺序调用每个`delegate`的`getRoutes()`方法，并将结果合并成一个统一的`Flux<Route>`

也就是说，当`CachingRouteLocator`调用`delegate.getRoutes()`时，实际是调用`CompositeRouteLocator.getRoutes()`，后者再调用所有注册的`RouteLocator`的`getRoutes()`方法



### RouteDefinitionRouteLocator 

`RouteDefinitionRouteLocator`是具体的路由定位器，负责将路由定义（RouteDefinition）转换为可用的Route对象

#### getRoutes() 方法

```java
@Override
public Flux<Route> getRoutes() {
	Flux<Route> routes = this.routeDefinitionLocator.getRouteDefinitions().map(this::convertToRoute);
	// ... 错误处理的逻辑
    
	return routes.map(route -> {
		if (logger.isDebugEnabled()) {
			logger.debug("RouteDefinition matched: " + route.getId());
		}
		return route;
	});
}
```

从`routeDefinitionLocator`获取`RouteDefinition`流，并通过`convertToRoute`方法将每个定义转换为`Route`

#### convertToRoute 方法

组合断言（predicates）和过滤器（filters），构建完整的`Route`对象
```java
private Route convertToRoute(RouteDefinition routeDefinition) {
    AsyncPredicate<ServerWebExchange> predicate = combinePredicates(routeDefinition);
    List<GatewayFilter> gatewayFilters = getFilters(routeDefinition);
    return Route.async(routeDefinition).asyncPredicate(predicate).replaceFilters(gatewayFilters).build();
}
```

因此，当`CompositeRouteLocator`调用其`delegates`时，`RouteDefinitionRouteLocator`会生成实际的路由规则，这些规则被聚合到`CompositeRouteLocator`的返回流中



### RouteDefinitionLocator接口调用

RouteDefinitionRouteLocator的getRoutes()方法里执行了this.routeDefinitionLocator.getRouteDefinitions()

也就是RouteDefinitionLocator的getRouteDefinitions()方法



### CompositeRouteDefinitionLocator

作为RouteDefinitionLocator接口的实现类，执行了getRouteDefinitions方法

```java
public class CompositeRouteDefinitionLocator implements RouteDefinitionLocator {
    private final Collection<RouteDefinitionLocator> delegates;
    
    @Override
    public Flux<RouteDefinition> getRouteDefinitions() {
        return this.delegates.flatMapSequential(RouteDefinitionLocator::getRouteDefinitions)
            // ... 其他处理逻辑
    }
}
```

- delegates: 包含了所有注册的 RouteDefinitionLocator 实现
- flatMapSequential(RouteDefinitionLocator::getRouteDefinitions): 对集合中的每个定位器调用其 getRouteDefinitions() 方法
- 我们的DynamicRouteDefinitionLocator: 作为其中一个 RouteDefinitionLocator 实例被包含在 delegates 集合中



### DynamicRouteDefinitionLocator 

终于到了我们自己定义的动态路由定义定位器

- 我们的 DynamicRouteDefinitionLocator 被标记为 @Component 并注册为Spring Bean
- Spring容器自动将其注入到 CompositeRouteDefinitionLocator 的 delegates 集合中
- 因此在 flatMapSequential 操作中会遍历到你的实例并调用其 getRouteDefinitions() 方法



### RedisRouteDefinitionRepository

将存储逻辑分离到 `RedisRouteDefinitionRepository`，从Redis中获取路由定义。

由于篇幅有限 <u>路由配置存储与加载</u> 有关的内容，放于其他篇章介绍


# 总结

我们的`DynamicRouteDefinitionLocator`会被调用，它集成到了Spring Cloud Gateway的路由加载链路中。当网关需要路由定义时，会通过 `CompositeRouteDefinitionLocator`调用到您的实现，从而从Redis获取动态路由配置。
