---
title: SpringSecurity
date: 2022-08-01 06:31:33
tags:
	- 大学
cover: https://images.wallpaperscraft.com/image/single/spruce_art_forest_131371_300x168.jpg
---

## SpringSecurity完成基于数据库的认证（登录）

 1. application.yml配置

    ```yml
    spring:
      datasource:
            name: test
            url: jdbc:mysql://localhost:3306/javaboy?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
            username: root
            password: wqeq
            # 使用druid数据源
            type: com.alibaba.druid.pool.DruidDataSource
            driver-class-name: com.mysql.cj.jdbc.Driver
    
    ```

 2. mysql,mybatis,druid依赖必须有

    ```xml
    		<dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.1</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.1.7</version>
            </dependency>
    ```

    

 3. 创建实体类

   4. User类 ：
      - 实现	**UserDetails**接口，并实现其中的方法(这是个规范，因为每个人设计的用户名和密码不一定都是username和password)

      - <u>List< Role > roles</u>;变量用于存储user的角色属性

      - <u>Collection<? enxtend GrandtedAuthority> getAuthorities()</u>方法，用于返回用户所有角色

        ```java
        public class User implements UserDetails {
             private Integer id;
             private String username;
             private String password;
             private Boolean enabled;
             private Boolean locked;
             private List<Role> roles;
        
            public Boolean getLocked() {
                return locked;
            }
        
            public List<Role> getRoles() {
                return roles;
            }
        
            public void setRoles(List<Role> roles) {
                this.roles = roles;
            }
        
            public Integer getId() {
                return id;
            }
        
            public void setId(Integer id) {
                this.id = id;
            }
        
            public void setUsername(String username) {
                this.username = username;
            }
        
            public void setPassword(String password) {
                this.password = password;
            }
        
            public Boolean getEnabled() {
                return enabled;
            }
        
            public void setEnabled(Boolean enabled) {
                this.enabled = enabled;
            }
            public void setLocked(Boolean locked) {
                this.locked = locked;
            }
        
            @Override
            public Collection<? extends GrantedAuthority> getAuthorities() {
                List<SimpleGrantedAuthority> authorities = new ArrayList<>();
                for (Role role : roles) {
                    authorities.add(new SimpleGrantedAuthority("ROLE_"+role.getName()));
                }
        
                return authorities;
            }
        
            @Override
            public String getPassword() {
                return password;
            }
        
            @Override
            public String getUsername() {
                return username;
            }
        
            @Override
            public boolean isAccountNonExpired() { //账户是否未过期
                return true;//一般表里会有一个字段表示账户是否过期，但是我们的表里还没有，为了方便起见，默认返回true，也就是未过期
            }
        
            @Override
            public boolean isAccountNonLocked() {//账户是否未锁定
                return !locked;
            }
        
            @Override
            public boolean isCredentialsNonExpired() {//密码是否未过期
                return true;
            }
        
            @Override
            public boolean isEnabled() {//是否可用
                return enabled;
            }
        }
        
        ```

        

   5. Role类，List< Role > roles;变量用于存储user的角色属性

      ```java
      package com.lcdzzz.mysecuritydb.bean;
      
      import java.util.List;
      
      public class Role {
          private Integer id;
          private String name;
          private String nameZh;
          private List<Role> roles;
      
          public List<Role> getRoles() {
              return roles;
          }
      
          public void setRoles(List<Role> roles) {
              this.roles = roles;
          }
      
          public Integer getId() {
              return id;
          }
      
          public void setId(Integer id) {
              this.id = id;
          }
      
          public String getName() {
              return name;
          }
      
          public void setName(String name) {
              this.name = name;
          }
      
          public String getNameZh() {
              return nameZh;
          }
      
          public void setNameZh(String nameZh) {
              this.nameZh = nameZh;
          }
      }
      
      ```

      

6. 定义mapper层和实现类（service层）
   1. UserMapper

   2. 创建service层中的**UserService**类，实现UserDetailService这个类和它的方法（<u>public UserDetails loadUserByUsername</u>这个方法）

      ```java
      @Service
      public class UserService implements UserDetailsService {
          @Autowired
          UserMapper userMapper;
      
          @Override
          public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
              User user = userMapper.loadUserByUsername(username);
              if (user==null){
                  throw new UsernameNotFoundException("用户不存在!");
              }
              user.setRoles(userMapper.getUserRolesById(user.getId()));
              return user;
          }
      }
      ```

      

   3. 用@Autowired注解注入UserMapper对象

   4. 在<u>loadUserByUsername</u>来<u>自定义查询用户信息的方法</u>
      1. 一个是loadUserByUsername这个方法，如果没有在数据库找到符合条件的数据，则返回null，抛出UsernameNotFoundException异常
      2. 一个是getRolesById这个方法，得到用户的角色（身份）信息

   5. 在UserMapper中定义方法

      ```java
      @Mapper
      public interface UserMapper  {
      
      
           User loadUserByUsername(String username);
      
          List<Role> getUserRolesById(Integer id);
      }
      
      ```

      

   6. 在UserMapper.xml中实现

      ```xml
      <mapper namespace="com.lcdzzz.mysecuritydb.mapper.UserMapper">
      
          <select id="loadUserByUsername" resultType="com.lcdzzz.mysecuritydb.bean.User">
              select * from user where username=#{username}
          </select>
      
          <select id="getUserRolesById" resultType="com.lcdzzz.mysecuritydb.bean.Role">
              select * from role where id in (select rid from user_role where uid=#{id})
          </select>
      </mapper>
      ```

      

7. config层中的**SecurityConfig**
   1. 记得加上@Configuration注解

   2. **SecurityConfig**继承**WebSecurityConfigurerAdapter**

   3. 来一个<u>protect void configure(AuthenticationManagerBuilder auth)</u> 方法

   4. 用@Autowired注解把UserService注入进来

   5. 来一个<u>PasswordEncoder passwordEncoder()</u>方法，再return new BCryptPasswordEncoder()，最后加上注解@Bean

   6. 在“3”说的方法里写：<u>auth.userDetailService(userService)</u> 来<u>指定使用自定义查询用户信息来完成身份认证;</u>

   7. 并且通过protected void configure(HttpSecurity http)方法来定义权限的访问范围

      ```java
      @Configuration
      public class SecurityConfig extends WebSecurityConfigurerAdapter {
          @Autowired
          UserService userService;
      
          @Override
          protected void configure(AuthenticationManagerBuilder auth) throws Exception {
              auth.userDetailsService(userService);//指定使用自定义查询用户信息来完成身份认证
          }
          @Bean
          PasswordEncoder passwordEncoder(){
              return new BCryptPasswordEncoder();
          }
      
      
          @Bean
          RoleHierarchy roleHierarchy() {
              RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
              String hierarchy = "ROLE_dba > ROLE_admin \n ROLE_admin > ROLE_user";
              roleHierarchy.setHierarchy(hierarchy);
              return roleHierarchy;
          }
          @Override
          protected void configure(HttpSecurity http) throws Exception {
              http.authorizeRequests()
                      .antMatchers("/dba/**").hasRole("dba")
                      .antMatchers("/admin/**").hasRole("admin")
                      .antMatchers("/user/**").hasRole("user")
                      .anyRequest().authenticated()
                      .and()
                      .formLogin()
                      .permitAll()
                      .and()
                      .csrf().disable();
          }
      }
      
      ```

   8. 其中这段代码很重要他代表dba既可以干admin的事情也可以干user的事情；admin可以干user的事情。==角色的继承==

      ```java
      @Bean
          RoleHierarchy roleHierarchy() {
              RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
              String hierarchy = "ROLE_dba > ROLE_admin \n ROLE_admin > ROLE_user";
              roleHierarchy.setHierarchy(hierarchy);
              return roleHierarchy;
          }
      ```

      

   9. <u>在controller层写一个HelloController</u>

   10. 在porm.xml里配一个resources，指定资源文件目录



## 动态配置权限（基于数据库）

1. 上面登录完成后，要想通过数据库来动态配置权限。就要定义几个东西

2. 关于MyFilter类：

   1. 在config层中创一个 **MyFilter**类，当然名字是自定义的。去实现 **FilterInvocationSecurityMetadataSource**并实现其中的三个方法

   2. **getAttribute**方法的作用：==根据请求的地址，分析出来这个地址需要哪些角色==

   3. `AntPathMacher pathMatcher = new AntPathMatcher()` 这个是一个路径匹配符

   4. 在这个方法正式运作之前，要在Menu定义一个`private List< Role> roles` 变量，因为每个menu需要是某个角色才能访问，意思是当前这个menu需要具备哪些角色才能访问

   5. 在mapper层来一个MenuMapper类，定义 `List< Menu> getAllMenus()` 方法

   6. 在service层来一个 **MenuService** ，实现一个 `List< Menu> getAllMenu()`方法，通过@Autowired把MenuMapper自动装配过来

   7. 在MenuMapper中实现。在这里 因为到时候查出来是一对多的关系，所以不能resultType，要使用resultMap

   8. 转到MyFilter，用@Autowired把MenuService注入进来

   9. 以下是关键代码以及解释：

      ```java
       @Override
          /**
           * 根据请求的地址，分析出来这个地址需要哪些角色
           * 根据需要的角色，拿出来目前“我”具有的角色，比较一下是否具备
           */
          public Collection<ConfigAttribute> getAttributes(Object o) throws IllegalArgumentException {
              String requestUrl = ((FilterInvocation) o).getRequestUrl();//请求的地址
              List<Menu> allMenus = menuService.getAllMenus();//得到所有的菜单
              for (Menu menu : allMenus) {
                  if (pathMatcher.match(menu.getPattern(), requestUrl)) {//第一个是规则，第二个是地址，看看是否匹配
                      List<Role> roles = menu.getRoles();
                      String[] rolesStr = new String[roles.size()];
                      for (int i = 0; i < roles.size(); i++) {
                          rolesStr[i] = roles.get(i).getName();
                      }
                      return SecurityConfig.createList(rolesStr);
                  }
              }
              return SecurityConfig.createList("ROLE_login");//如果角色是ROLE_login，代表登录之后就可以访问这个资源
          }
      
      ```

      

3. 关于MyAccessDecisionManager

   1. 创建MyAccessDecisionManager类，让它实现AccessDecisionManager接口，并实现里面的方法<!--核心只有最上面一个方法，下面两个方法和之前一样，意思是：“是否支持这种方式”，所以返回true就行	-->，记得在类的上面加上@Component注解

   2. 以下是关键方法以及代码的解释

      ```java
       @Override
          /**
           * authentication保存着当前登录的用户的信息。从这里可以知道我有哪些角色
           * collection就是MyFilter类中的getAttributes的返回值。从这里可以知道需要哪些角色
           */
          public void decide(Authentication authentication, Object o, Collection<ConfigAttribute> collection) throws AccessDeniedException, InsufficientAuthenticationException {
              for (ConfigAttribute attribute : collection) {//这是需要的角色
                  if ("ROLE_login".equals(attribute.getAttribute())) {//意思是这个请求只要登录了就能访问（这边可以自定义，我们这里就这么举例）
                      if (authentication instanceof AnonymousAuthenticationToken) {//AnonymousAuthenticationToken意思是匿名用户，也就是没登录，所以要抛异常
                          throw new AccessDeniedException("非法请求!");
                      } else {
                          return;
                      }
                  }
                  Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();//得到我现在属于的角色
                  for (GrantedAuthority authority : authorities) {
                      if (authority.getAuthority().equals(attribute.getAttribute())) {//如果我具备你需要的
                          return;
                      }
                  }
              }
              throw new AccessDeniedException("非法请求!");//非常不幸的走到了这一步，意味着你是非法请求（不然中途就break了）
          }
      
      ```



## Spring Security结合OAuth2协议



1. 生成一个加密后的密码，明文“123”

   ```java
   @SpringBootTest
   class Oauth2ApplicationTests {
       @Test
       public void contextLoads() {
           System.out.println(new BCryptPasswordEncoder().encode("123"));
       }
   }
   ```

   

2. 配置授权服务器**AuthorizationServerConfig**继承**AuthorizationServerConfigurerAdapter**

   ```java
   @Configuration
   @EnableAuthorizationServer
   public class AuthorizationServerConfig extends AuthorizationServerConfigurerAdapter {
       @Autowired
       AuthenticationManager authenticationManager;//主要用来支持password的认证模式
       @Autowired
       RedisConnectionFactory redisConnectionFactory;
       @Autowired
       UserDetailsService userDetailsService;//刷新token的时候会用到
   
       @Bean
       PasswordEncoder passwordEncoder(){
           return new BCryptPasswordEncoder();
       }
   
   
       @Override
       public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
           clients.inMemory()//配置在内存里边的
                   .withClient("password")//认证模式为password模式
                   .authorizedGrantTypes("password", "refresh_token")//配授权模式，两种
                   .accessTokenValiditySeconds(1800)//token的过期时间，1800秒
                   .resourceIds("rid")//给资源取个名字
                   .scopes("all")
                   .secret("c405d914-f9cf-42d5-972e-0ffd4a1522bd");//一会需要的密码
       }
   
   
       /**
        * 配置令牌的存储，待会把令牌存到哪去！
        */
       @Override
       public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
           endpoints.tokenStore(new RedisTokenStore(redisConnectionFactory))
                   .authenticationManager(authenticationManager)
                   .userDetailsService(userDetailsService);
       }
   
       @Override
       public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
           security.allowFormAuthenticationForClients();//表示支持登录认证
       }
   }
   
   ```

   

3. 配置资源服务器**ResourceServerConfig**继承**ResourceServerConfigurerAdapter**

   ```java
   /**
    * 资源服务器
    */
   @Configuration //表示是个配置类
   @EnableResourceServer
   public class ResourceServerConfig extends ResourceServerConfigurerAdapter {
       @Override
       public void configure(ResourceServerSecurityConfigurer resources) throws Exception {
           resources.resourceId("rid")//指定资源id，就是在授权服务器里面配置的 rid  【.resourceIds("rid")】
                   .stateless(true);//意思是这些资源是基于令牌来认证的
       }
   
   
       /**
        * 这就是我提供的资源！
        */
       @Override
       public void configure(HttpSecurity http) throws Exception {
           http.authorizeRequests().antMatchers("/admin/**").hasRole("admin")
                   .antMatchers("/user/**").hasRole("user")
                   .anyRequest().authenticated();//剩下其他的请求都是（authenticated）登录之后就可以访问
       }
   }
   ```

   

4. 配置**SecurityConfig**继承**WebSecurityConfigurerAdapter**

   ```java
   
   @Configuration
   public class SecurityConfig  extends WebSecurityConfigurerAdapter {
       @Override
       @Bean
       protected AuthenticationManager authenticationManager() throws Exception {
           return super.authenticationManager();//这个authenticationManager和
           // 下面的userDetailsService会传给授权服务器
       }
   
       @Bean
       @Override
       protected UserDetailsService userDetailsService() {
           return super.userDetailsService();
       }
   
       @Override
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           auth.inMemoryAuthentication()
                   .withUser("lcdzzz").password("$2a$10$BQsi4LxO/9536a2wwW.5D.T/t3fm52xzF17Eo6xlFinxuk8uKjEg2").roles("admin")
                   .and()
                   .withUser("zhoudian")
                   .password("$2a$10$BQsi4LxO/9536a2wwW.5D.T/t3fm52xzF17Eo6xlFinxuk8uKjEg2")
                   .roles("user");
       }
   
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http.antMatcher("/oauth/**")
                   .authorizeRequests()
                   .antMatchers("/oauth/**").permitAll()
                   .and().csrf().disable();
       }
   }
   
   ```

   

5. 在application.properties里配置

   ```properties
   spring.redis.host=8.142.93.194
   spring.redis.port=6379
   spring.redis.database=0
   spring.redis.timeout=1000
   ```

   



## Spring Security使用Json登录

1. 在filter层中创建MyAuthenticationFilter来继承UsernamePasswordAuthenticationFilter，重写父类的attemptAuthentication方法

   ```java
   public class MyAuthenticationFilter extends UsernamePasswordAuthenticationFilter {
       @Override
       public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
           if (!request.getMethod().equals("POST")) {
               throw new AuthenticationServiceException(
                       "Authentication method not supported: " + request.getMethod());
           }
           if (request.getContentType().equals(MediaType.APPLICATION_JSON_VALUE)) {
               //说明用户以 JSON 的形式传递的参数
   
               String username = null;
               String password = null;
               try {
                   Map<String, String> map = new ObjectMapper().readValue(request.getInputStream(), Map.class);//getInputStream是一个流，把这个流解析出来就是个json字符串了
                   //不是所有请求都有流，get就没有，只有有body的请求才有流
   
                   username = map.get("username");
                   password = map.get("password");
               } catch (IOException e) {
                   e.printStackTrace();
               }
   
               if (username == null) {
                   username = "";
               }
   
               if (password == null) {
                   password = "";
               }
   
               username = username.trim();
   
               UsernamePasswordAuthenticationToken authRequest = new UsernamePasswordAuthenticationToken(
                       username, password);
   
               // Allow subclasses to set the "details" property
               setDetails(request, authRequest);
   
               return this.getAuthenticationManager().authenticate(authRequest);
           }
           return super.attemptAuthentication(request, response);
   
       }
   }
   
   ```

2. 接下来如何让上面的东西生效呢？则需要再配下security的配置

   在config层创建一个SecurityConfig继承WebSecurityConfigurerAdapter

   ```java
   @Configuration
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http.authorizeRequests().anyRequest().authenticated()
                   .and()
                   .formLogin().permitAll()
                   .and().csrf().disable();
           http.addFilterAt(myAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);//加一个filter
       }
       @Bean
       MyAuthenticationFilter myAuthenticationFilter() throws Exception {
           MyAuthenticationFilter filter = new MyAuthenticationFilter();
           filter.setAuthenticationManager(authenticationManagerBean());
           return filter;
       }
   }
   ```

