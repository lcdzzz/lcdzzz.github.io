---

title: Spring5自学
date: 2022-01-20 06:31:33
tag: 
	- 大学
cover: https://images.wallpaperscraft.com/image/single/forest_trees_mountains_146485_300x168.jpg
---

动力节点Spring框架初学总结：https://www.processon.com/view/link/5fbdf69b0791294615630fd9#map





### Spring框架概述：

1. Spring是轻量级的开源的JavaEE框架
2. Spring可以解决企业应用开发的负杂性
3. Spring有两个核心部分：IOC和Aop

<br><br><br><br>

### IoC（Inversion of  Control）:

控制反转， 是一个理论，概念，思想

描述的：把对象的创建，赋值，管理工作都交给代码之外的**容器**实现， 也就是对象的创建是有其它外部资源完成。(把对象的创建交给别人去干)

- **控制**：创建对象，对象的属性赋值，对象之间的关系管理

- **反转**：把原来的开发人员管理、创建对象的权限转移给代码之外的容器来实现，由容器代替开发人员管理对象，创建对象，给属性赋值。

- 正传：由开发人员在代码中，使用new构造方法创建对象，开发人员主动管理对象。

  ```java
   public static void main(String args[]){
              Student student = new Student(); // 在代码中， 创建对象。--正转。
  
  		 }
  ```

- **容器**：是一个服务器软件，一个框架（spring）

- 为什么要使用 ioc ： 目的就是减少对代码的改动， 也能实现不同的功能。 实现**解耦合**。 

- java中创建对象有哪些方式：
    1. 构造方法 ， new Student（）
    2. 反射
    3. 序列化
    4. 克隆
    5. **ioc ：容器创建对象**
    6. 动态代理

- ioc的体现： 

     1. 创建类继承HttpServelt 

     2. 在web.xml 注册servlet ， 使用

        < servlet-name> myservlet < /servlet-name>
        < servelt-class>com.bjpwernode.controller.MyServlet1< /servlet-class>

     3.  没有创建 Servlet对象， 没有 MyServlet myservlet = new MyServlet()Servlet 是Tomcat服务器它能你创建的。 Tomcat也称为容器
        omcat作为容器：里面存放的有Servlet对象， Listener ， Filter对象

- IoC的技术实现 ： :依赖注入， 只需要在程序中提供要使用的对象名称就可以， 至于对象如何在容器中创建， 赋值，查找都由容器内部实现。

- **spring**是使用的**di**实现了**ioc**的功能， spring底层创建对象，使用的是**反射机制**。

<br><br><br><br>



### junit:

单元测试，一个工具类库，做测试方法使用的

单元：指定的是方法，一个类中有很多方法，一个方法称为单元

使用单元测试：

1. 需要加入junit依赖。

   ```java
   <dependency>
     <groupId>junit</groupId>
     <artifactId>junit</artifactId>
     <version>4.11</version>
     <scope>test</scope>
   </dependency>
   ```

2. 创建测试作用的类：叫做测试类

   src/test/java目录中创建类

3. 创建测试方法

   1. public方法
   2. 没有返回值void
   3. 方法名称自定义，建议名称是test+要测试方法名称

4. 方法没有参数

5. 方法上面加入@Test，这样的方法是可以单独执行的。不用使用main方法

   ```java
   @Test
   public void test01(){
       //使用spring容器创建的对象
       //1.指定spring配置文件的名称
       String config="ba01/applicationContext.xml";
       //2.创建表示spring容器的对象，ApplicationContext
       //ApplicationContext就是表示spring容器，通过容器获取对象
       //ClassPathXmlApplicationContext:表示从类路劲中加载spring配置文件
       ApplicationContext ac = new ClassPathXmlApplicationContext(config);//这句话执行时，对象创建成功
   
       //从容器中获取某个对象，你要调用对象的方法
       //getBean("配置文件中bean的id值")
       Student myStudent =(Student) ac.getBean("myStudent");
       System.out.println("student对象= "+myStudent);
   }
   ```

<br><br><br><br>

## 基于XML的DI

### 注入分类

1. set注入（掌握）

   1. 简单类型

      简单类型的set注入：spring调用类的**set方法**

      ```java
      <bean id ="xx" class="yyy">
      <property name ="属性名字" value="此属性的值"/>
      一个property只能给一个属性赋值
      <property...>
      </bean>
      ```

      

   2. 引用类型：

       引用类型的set注入：spring调用类的**set方法**

      ```java
      <bean id ="xxx" class="yyy">
          <property name="属性名" ref="bean的id（对象名称）"/>
      </bean>
      ```

2. 构造注入（理解）

   spring调用类有的**有参数构造方法**，在创建对象的同时，在构造方法中给属性赋值，构造注入使用
   

<constructor -arg>标签
   <constructor -arg> 标签： 一个<constructor -arg>表示构造方法的一个参数（构造方法中有三个参数则对应三个<constructor -arg>）
                   <constructor -arg>标签属性：
                       *name*:表示构造方法的形参名
                       *index*:表示构造方法的参数位置，参数从左网友位置是0,1,2
                       *value*：构造方法形参类型是简单类型，使用value
                       *ref*：构造方法形参类型是引用类型，使用ref



<br><br>

###  引用类型自动注入

spring框架根据某些规则可以给引用类型赋值。你不用再给引用类型赋值了

1. **byName**方式自动注入

   （按名称注入）：java类中**引用类型的属性名**和**spring容器中的（配置文件）< bean>的id名称**一样，且数据类型是**一致**的，这样的容器中的bean，spring能够赋值给引用类型

   ```java
   语法：
   <bean id ="xx" class="yy" autowire="byName">
     简单类型属性赋值
   </bean>
   ```

2. **byType**方法自动注入

   按类型注入）：java类中引用类型的数据类型和spring容器中（配置文件）< bean>的class的属性是同源关系的，这样的bean能够赋值给引用类型。
           同源就是一类的意思：
           1.java类中引用类型的数据类型和bean的class值是**一样**的或是父子类关系的。
           2.java类中引用类型的数据类型和bean的class值是**接口和实现类**的关系。

   ```java
   语法：
   <bean id ="xx" class="yy" autowire="byName">
     简单类型属性赋值
   </bean>
   ```

   **注意**：在byType中，在xml配置文件中声明bean只能有一个是符合条件的，否则错误

<br><br>

### 为应用指定多个Spring配置文件

-   多个配置优势：
  1. 每个文件的大小比一个文件要小很多。效率高
  2. 避免多人竞争带来的冲突。
  3.  如果你的项目有多个模块（相关的功能在一起） 一个模块一个配置文件。
       学生考勤模块一个配置文件，  张三
       学生成绩一个配置文件，      李四
-  多文件的分配方式：
  1. 按功能模块，一个模块一个配置文件
  2. 按类的功能，数据库相关的配置一个文件配置文件， 做事务的功能一个配置文件， 做service功能的一个配置文件等

<br><br><br><br>

## 基于注解的DI

### 定义bean的注解@Component

- @component: 创建对象，等同于< bean>的功能
	**属性**：value，就是对象的名称（不指定对象名称，有spring提供默认的名称：类名首字母小写），也就是bean的id值， value的值是唯一的，创建的对象在整个spring容器中就一个
	**位置**：在类的上面
	
- `@Component(value = "myStudent")`等同于
	`< bean id="myStudent" class="com.bjpowernode.ba01.Student" />`
	
-   spring中和@Component功能一致的，创建对象的注解还有：

  1. **@Repository**（用在持久层类的上面）：放在dao的实现类上面，表示创建dao对象，dao对象是能访问数据库的。

  2. **@Service**（用在业务层类的上面）：放在service的实现类上面，创建service对象，service对象是做业务出列，可以由事务等功能的。

  3. **@Controller**（用在控制器的上面）：放在控制器（处理器）类上面，创建控制器对象的，控制器对象，能够接受用户提交的参数，显示请求的处理结果。

     <br>

     以上三个注解的使用语法和@Component一样的。都能创建对象，但是这三个注解还有额外的功能。
       @Repository，@Service，@Controller是给项目的对象分层的。

  

  <br>
  
  <br>

### 简单类型属性注入@Value

**属性**：value是String类型的，表示简单类型的属性值
**位置**:   属性定义的上面，无需set方法，推荐使用；在set方法上面

<br>

<br>

### byType自动注入@Autowired

- spring框架提供的注解，实现**引用类型**的赋值
- spring中通过注解给引用类型赋值，使用的是自动注入原理，支持byName，**byType（默认）**
- **位置**:在属性定义的上面，无需set方法，推荐使用
- **属性**：required，是一个boolean类型的，默认true
  1. required=true:表示引用类型赋值失败，程序报错，并终止执行
  2. required=false:引用类型赋值失败，程序正常执行，引用类型为null

<br><br>

### byName自动注入@Autowired与@Qualifier

步骤：

1. 在属性上面加入@Autowired
2. 在属性上面加入@Qualifier(value="bean的id"):表示使用指定名称的bean来完成赋值

<br><br>

### JDK注解@Resource引用类型自动注入

- 来自jdk的注解，spring框架提供了对这个注解的功能支持，可以使用它给**引用类型赋值**。使用时也是自动注入原理，**默认是byName**
- **位置**：在属性定义的上面，无需set方法，推荐使用；在set方法的上面
- 默认先使用byName自动注入，如果byType赋值失败，在使用byType
-  如果只想通过byName，则需要添加一个属性：name，name的值是bean的id（名称）

<br><br><br><br>

## AOP面向切面编程

### 动态代理：

- **实现方式**：**jdk动态代理**，使用jdk中的Proxy，Method，InvocaitonHanderl创建代理对象。 jdk动态代理要求目标类必须实现接口

   **cglib动态代理**：第三方的工具库，创建代理对象，原理是继承。 通过继承目标类，创建子类。
   子类就是代理对象。 要求目标类不能是final的， 方法也不能是final的

- **动态代理的作用**：

  1. 在目标类源代码不改变的情况下，增加功能。
  2. 减少代码的重复
  3. 专注业务逻辑代码
  4. 解耦合，让你的业务功能和日志，事务非业务功能分离。

<br><br>

### AOP（Aspect Orient Programming）面向切面编程

**Aspect**: 切面，给你的目标类增加的功能，就是切面。 像上面用的日志，事务都是切面。
          切面的特点： 一般都是非业务方法，独立使用的。

**Orient**：面向， 对着。

  **Programming**：编程

  **oop: 面向对象编程**

-   **怎么理解面向切面编程 ？**
  1. 需要在分析项目功能时，找出切面。
  2. 合理的安排切面的执行时间（在目标方法前， 还是目标方法后）
  3. 合理的安全切面执行的位置，在哪个类，哪个方法增加增强功能
- 术语：
  1.  **Aspect**:切面，表示增强的功能， 就是一堆代码，完成某个一个功能。非业务功能，常见的切面功能有日志， 事务， 统计信息， 参数检查， 权限验证。
  2.  JoinPoint:连接点 ，连接业务方b法和切面的位置。 就某类中的业务方法
  3. **Pointcut** : 切入点 ，指多个连接点方法的集合。多个方法
  4. 目标对象： 给哪个类的方法增加功能， 这个类就是目标对象
  5. **Advice**:通知，通知表示切面功能执行的时间。
- 一个切面有三个关键的要素：
  1. 切面的功能代码，切面干什么
  2. 切面的执行位置，使用Pointcut表示切面执行的位置
  3. 切面的执行时间，使用Advice表示时间，在目标方法之前，还是目标方法之后

<br><br>

### AOP实现

 aop是一个规范，是动态的一个规范化，一个标准

aop的技术实现框架：

1. **spring**：spring在内部实现了aop规范，能做aop的工作。
   spring主要在事务处理时使用aop。
   我们项目开发中很少使用spring的aop实现。 因为spring的aop比较笨重。
2. **aspectJ**：一个开源的专门做aop的框架。spring框架中集成了aspectj框架，通过spring就能使用aspectj的功能。
   	  aspectJ框架实现aop有两种方式：
   1. 使用xml的配置文件 ： 配置全局事务
   2. 使用注解，我们在项目中要做aop功能，一般都使用注解， aspectj有5个注解。

<br><br>

### 学习aspectj框架的使用

1. 切面的执行时间， 这个执行时间在规范中叫做Advice(通知，增强)， 在aspectj框架中使用注解表示的。也可以使用xml配置文件中的标签
   1. @Before（前置）
   2. @AfterReturning（后置）
   3. @Around（环绕）
   4. @AfterThrowing（异常）
   5. @After（最终）
   6. @Pointcut （定义切入点）
2. 表示切面执行的位置，使用的是切入点表达式。



  



