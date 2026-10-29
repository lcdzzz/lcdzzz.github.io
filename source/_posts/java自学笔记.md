---
title: Java自学笔记
date: 2021-07-04 19:51:28
tags: 
	- 大学
cover: https://images.wallpaperscraft.com/image/single/lawn_forest_mountains_144578_300x168.jpg
---

## IDEA使用

### 新建

1. 新建一个Empty Project

   新建一个空的工程，选择创建工程窗口下面的Empty Project

2. 给空的工程起一个名字：javase

3. 会自动弹出一个：project structure，这个窗口先取消掉

4. 给空的工程下新建Module（模块）

   idea的组织方式projecrt-->module

5. 在New Module窗口上点击左下角的java，然后next

6. 给module起一个名字

7. 编写代码，在src目录下新建类，写代码，并运行。

<br><br>

### 关于IDEA工具的快捷键以及一些简单的设置

#### 字体设置

file-->setting-->输入font-->设置字体样式以及字号大小

<br><br>

#### 快捷方式

- psvm（main方法）        sout（system.out.println)

  

- 删除一行：ctrl+y

  

- 左侧窗口的列表怎么展开？怎么关闭？

  左箭头关闭，右箭头展开，上下箭头移动

  

- idea中退出任何窗口，都可以使用esc键

  

- 任何新增、新建、添加的快捷键：

  ​	alt+insert

  

- 窗口变大变小

  ​	ctrl+shift+F12

  

- 快速运行

  ctrl+shift+F10

  

- 切换java程序：

  alt+左/右箭头

  

- 切换窗口：

  alt+标号（打开/关闭）

  alt+1(打开、关闭)

  

- 提示方法的参数：ctrl+p

  

- 注释：

  单行注释：ctrl+/

  多行注释：ctrl+shift+/

  

- 怎么定位方法/属性/变量?

  光标停到单词下面，这个单词可能是方法名、变量名，停到单词下面之后，按住ctrl键，出现下划线，点击跳转。

  

- idea当中复制一行是ctrl+d

  

- idea工具中纠正错误的快捷键：alt+回车

  

- 快速查看源代码：ctrl按住。点击单词

  
  
- 查看一个类的属性和方法：ctrl + F12  

  <br>
  
  <br>
  
  <br>
  
  <br>

## super关键字

1. super是一个关键字，全部小写

2. super和this对比着学习

   this：

   * 能出现在实例方法和构造方法中

   * 语法是"this." 、 "this()"

   * **this不能出现在静态方法中**

   * 大部分条件下是可以省略的

   * this.什么时候不能省略呢？

     **在区分局部变量和实例变量的时候不能省略。**

     `public void setName(string name){`

     ​	`this.name=name;`

     `}`

     **this()只能出现在构造方法第一行**，通过当前的构造方法去调用“本类”中其他的构造方法，目的是：代码复用。

   super：

   * 能出现在实例方法和构造方法中

   * 语法是"super." 、 "super()"

   * **super不能出现在静态方法中**

   * 大部分条件下是可以省略的

   * super.什么时候不能省略呢？

     **父类和子类中有同名属性或者说有同样的方法，**

     **想在子类中访问父类的，super. 不能省略。**

     **super（)只能出现在构造方法第一行**，通过当前的构造方法去调用“父类”中其他的构造方法，目的是：创建子类对象的时候，先初始化父类型特征

3. super()

   表示通过子类的构造方法调用父类的构造方法。

   模拟现实世界中：要想要儿子，必须先有父亲。

   //一个类如果没有手动提供任何构造方法，系统会默认提供一个无参数构造方法。

   //一个类如果手动提供了一个构造方法，那么无参数构造方法系统将不会再提供。

4. 重要结论：

   **当一个构造方法第一行：**

   **既没有this（）又没有super（）的话，默认会有一个super（）；**

   **表示通过当前子类的构造方法调用父类的无参数构造方法。**

   **所以必须保证父类的无参数构造方法是存在的。**

5. 注意：

   this()和super()不能共存，他们都是只能出现在构造方法的第一行。

6. **无论怎么折腾，父类的构造方法是一定会执行的（100%）。**

   //在java语言中不管是new什么对象。最后老祖宗的object类的无参数构造方法一定会执行（object类的无参数构造方法是处于“栈顶部”）

   栈顶的特点：

   最后调用，但最先执行结束。

   后进先出原则

   以后写代码的时候，一个类的无参数构造方法还是建议手动写出来。

   如果无参数构造方法丢失的话，可能会影响到子类对象的构建

7. 注意：在构造方法执行过程中一连串调用了父类的构造方法，父类的构造方法有继续向下调用它的父类构造方法，但实际上对象只创建了一个。

   super（实参）的作用：初始化当前对象的父类型特征，并不是创建新对象，对象只创建了一个。

   //java中允许在子类中出现一个和父类同名的变量、同名属性。

8. super不是引用，也不保存内存地址，也不指向任何对象，super只代表当前对象内部的那一块父类型的特征。

9. 在父和子中有同名的属性或者相同的方法，如果此时想在子类中访问父类的数据，必须使用"super."加一区分。

10. 在子类中访问父类私有的数据，使用super.没有权限的

    但父类私有的构造方法，可以用super（实参）去构造。

    

    

*super的使用*
*supe*r.属性名    【访问父类的属性】*
    
*super.方法名(实参)   【访问父类的方法】*
    
super.(实参)   【调用父类的构造方法】*

<br>

<br>

<br>

<br>

## final关键字

1. final修饰的变量？

   **final修饰的局部标量，一但被赋值，就不能重新赋值【final修饰的变量只能赋一次值】**

   

2. fianl修饰的方法？

   **final修饰的方法无法被覆盖，被重写**

   

3. fianl修饰的类？

   **final修饰的类无法被继承**

   

4. final控制不了能不能调用的问题，final管的是：最后的，不能变的，不能改的。

   

5. final修饰的变量，如果这个变量是一个人”引用“

   引用也是一个变量，所以也是不能变的

   `*final person p=new person(30);*`
   `*p=new Person(30)//错误！无法为最终变量p分配值*`

   


   **final修饰的引用：**

  该引用只能指向1个对象，并且它只能永远指向该对象，无法指向其他对象。
  并且在该方法执行过程中，该引用指向对象后，该对象不会被垃圾回收器回收。
	
  直到当前方法结束，才会释放空间。

  虽然final的引用指向对象A后，不能再重新指向对象B。
  但是对象内部的数据可以被修改。

   


6. **final修饰的实例变量，系统不负责赋默认值，要求程序员必须手动赋值。**

   这个手动赋值，在变量后面赋值可以，在构造方法赋值也可以。

   final修饰的实例变量一般添加static修饰

   *解释：*
   *i永远都是10，创建100个对象，i也是10*
   *i是10是永远都不会改变的，既然这样，没必要声明为实例变量，最好是静态，节省内存空间*

   **static final联合修饰的变量成为“常量”。**

​	例：`public static final double PI=3.1415926;`

   常量名建议大写，每个单词之间采用下划线来衔接。

   *常量：实际上常量和静态变量一样，区别在于：*

   *常量的值不能变。*

   *常量和静态变量，都是存储在方法去，并且都是类加载时初始化*。

<br>

<br>

<br>

<br>

## 抽象类（abstract）

1. 抽象类怎么定义？

   在class钱添加abstract关键字。

2. **抽象类是无法实例化的，无法创建对象的，所以抽象类是用来被子类继承的**。

3. final和abstract不能联合使用，这两个关键字是对立的。

4. 抽象类的子类可以是抽象类，也可以不是。

5. 抽象类虽然无法实例化，但是抽象类有构造方法，这个构造方法是供子类使用的。

6. **抽象类中不一定有抽象方法，但是抽象方法必须出现在抽象类中。**

7. 抽象方法怎么定义？

   `public abstract void dosome();`

8. **一个非抽象的类，继承抽象类，必须将抽象类中的抽象方法进行覆盖/重写/实现。**

9. 面试题（判断）：java语言中凡是没有方法体的方法都是抽象方法。（×）

   *object类中就有很多方法没有方法体，都是以“;”结尾的，但他们不是抽象方法，例如：*

   *`public native int hashCode();`*

   *这个方法底层调用了C++写的动态链接库。*

   *前面修饰符列表中没有：abstract。有一个native表示调用JVM本地程序。*

   <br>

   <br>

   <br>
   
   <br>

## 接口（接口也是一种类）

### 接口的基础语法

1. 接口也是一种“引用数据类型”。编译之后也是一个class字节码文件。

2. 接口完全抽象。（抽象类是半抽象。）或者也可以说接口是特殊的抽象类。

3. 接口怎么定义的，语法是什么？

   [修饰符列表] interface 接口名{}

4. 接口支持多继承。

   `interface C extend A，B{`

   `}`

5. 接口只包含两个内容：常量+抽象方法。

6. 接口中所有元素都会public修饰的。

7. 接口中的抽象方法定义是：pubiic abstract 修饰符可以省略。

8. 接口中的常量的public static final可以省略。

9. 接口中的方法是抽象方法，所以接口中的方法不能有方法体（不能加大括号{}）。

10. 一个非抽象的类，实现接口的时候，必须将接口中所有方法加以实现。

11. 一个类可以实现多个接口

12. extends和implements可以共存，extends在前，implements在后。

    `class Cat extends Animal implements Flyable{`

    ​	`}`

    

13. 使用使用接口，在写代码时，可以使用多态（父类型引用指向子类型对象）。

    向下转型要养成习惯：转型之前先if+instanceof进行判断

    `if(m instanceof K){`

    `K k = (K) m；`

    `}`
    
    <br><br>

### 接口在开发中的作用

注意：接口在开发中的作用，类似于多态在开发中的作用。

多态：面向抽象编程，不要面向具体编程。降低程序耦合度。提高程序的扩展力。

```java
public class Master{
    public void feed(Dog d){}
    public void feed(Cat c){}
    //假设又要养其他宠物，那么这个时候需要再加1个方法。（需要修改代码）
    //这样扩展力太差，违背了ocp原则（对扩展开放，对修改关闭）
}

public class Master{
    public void feed(Animal a){
        /*
        面向Animal 父类编程，父类是比子类更抽象的。所以我们叫做面向抽象编程，不要面向具体编程。这样程序的扩展力就强。
        */
    }
}
```



接口在开发总共的作用？

抽象是完全抽象的，而我们以后正好要求，面向抽象编程。

面向抽象编程这句话以后可以修改为：面向接口编程。

有了剪口就有了可插拔，可插拔表示扩展力很强，不是焊接死的。

主板和内存条之间有插槽，这个插槽就是接口，内存条坏了，可以重新买一个换下来，这叫做高扩展性（低耦合度）。

<br><br>

**总结一句话：三个字<u>解耦合</u>**

面向接口编程，可以降低程序耦合度，提高程序扩展力。符合ocp开发原则 。接口的使用离不开多态机制。（接口+多态才可以打到降低耦合度。）

 接口可以解耦合，解开的是谁和谁的耦合？？？

任何一个接口都有调用者和实现着。

接口可以讲调用者和实现者解耦合。

调用者面向接口调用。

实现者面向接口编写实现。

<br>

以后进行大项目的开发，一般都是讲项目分离成一个模块一个模块的，模块和模块之间采用接口衔接，降低耦合度 

<br>

<br>

<br>

<br>

## 类型和类型之间的关系：

is a（继承）、has a（关联）、 like a（实现）

- is a:

  Ca is a Animal(猫是一个动物)

  凡是能够满足is a的表示“继承关系”

  `A extends B;`

- has a:

  I has a Pen(我有一支笔)

  凡是能够满足has a 关系的表示“关联关系”

  关联关系通常以“属性”的形式存在。

  `A{`

  `   B b;`

  `}`

- like a：

  Cooker like a FoodMenu(厨师想一个菜单一样)、

  凡是能够满足like a 关系的表示“实现关系”

  实现关系通常是：类实现接口。

  `A implements B;`

<br>

<br>

<br>

<br>

### 抽象类和接口有什么区别

*这里直说下抽象类和接口在语法上的区别。至于以后抽象类和接口应该怎么进行选择，通过后面的项目去体会。*

<br>

抽象类是半抽象的

接口是完全抽象的

<br>

抽象类中有构造方法。

接口中没有构造方法。

<br>

类与类之间只能单继承。

接口与接口直接支持多继承。

<br>

一个抽象类只能继承一个类（单继承）。

一个类可以同时实现多个接口。

<br>

接口中只允许出现常量和抽象方法。

<br>

以后接口使用的比抽象类多。一般抽象类使用的还是少。

接口一般都是对“行为”的抽象。

抽象类既可以抽象行为又可以抽象数据。

<br>

<br>

<br>

<br>

## package和import机制

### package

1. package出现在java源文件第一行。

2. 带有包名怎么编译？

   javac -d . xxx.java

3. 怎么运行？

   java 完整类名
   
   例：`java com.bjpowernode.javase.chapter17.HelloWorld`
   
   <br>
   
   <br>

### import

import什么时候不需要？

- java.lang不需要
- 同包下不需要
- 其他一律都需要

怎么用？

- import 完整类名
- import 包名.*;

<br><br><br><br>

## 访问控制权限

### 有哪些访问控制权限？

- private 私有
- protect 受保护
- public 公开
-  ____    默认

<br><br>

### 以上四种访问控制权限，控制的范围是什么？

| 访问控制修饰符 | 本类 | 同包 | 子类 | 任意位置 |
| -------------- | ---- | ---- | ---- | -------- |
| public         | Y    | Y    | Y    | Y        |
| protected      | Y    | Y    | Y    | N        |
| 默认           | Y    | Y    | N    | N        |
| private        | Y    | N    | N    | N        |

public>protected>默认>private

<br><br>

### 访问控制权限修饰符可以修饰什么

- 属性（4个都行）
- 方法（4个都能用）
- 类（public 和默认可以，其他不行）
- 接口（public 和默认可以，其他不行）

<br><br><br><br>

## JDK类库的根类：Object

这个老祖宗中的方法都是所有子类通用的，任何一个类默认继承Object，就算没有继承，也会间接继承

### Object类当中有哪些常用的方法？

怎么找？

1. 去源代码当中。（但这种方式比较麻烦，源代码也比较难）
2. 去查阅java的类库的帮助文档。

什么事API?

​      应用程序的编程接口。整个JDK的类库就是一个javase的API（application program interface）。每一个API都会配置一套API帮助文档。SUN公司提前写好的这套类库就是API。（一般每一份API都对应一份API帮助文档。）、

目前为止我们只需要知道这几个方法即可：

```java
protected Object clone()； //负责对象克隆的
int hashOde(); //换区对象哈希值的一个方法
boolean equals(Object obj); //判断两个对象是否相等
String toString(); //讲哦对象转换成字符串形式
protected void finalize(); //垃圾回收器负责调用的方法 
```



<br><br>

### toString()方法

以后所有类的toString()方法是需要重写的，重写规则，越简单越明了就好。

System.out.println(引用)；这里会自动调用“引用”的toString()方法。

String类是SUN写的，toString方法已经重写了。

<br><br>

### equals()方法

以后所有类的equals方法也需要重写，因为object中的equals方法比较的是两个对象的内存地址，我们应该比较内容，所以需要重写。

重写规则：自己定，主要看是什么和什么相等时表示里欧昂个对象相等。

基本数据类型比较实用：==

对象和对象比较：调用equals方法

String类是SUN编写的，所以Strng类的equals方法重写了。

以后判断两个字符串是否相等，最好不要实用==。要调用字符串对象的equals方法。

<br><br>

### finalize()方法

1. 在Object类中的源代码：

   `protected void finaliz() throw Throwable{ }`

   GC：负责带一把过finalize()方法

2. finalize()方法只有一个方法体，里面没有代码，而且这个方法是protected修饰的。

3. 这个方法不需要程序员手动调用，JVM的垃圾回收器会负责调调用这个方法。

   不像equals toString ,equals和toString（）方法是需要你写代码调用的。

   finalize（）只需要重写，重写玩将来会自动会有程序来调用。

4. finalize()方法的执行时机

   当一个java对象即将被垃圾回收器回收的时候，垃圾回收器负责调用fianlize()方法。

5. finalize()方法实际上是SUN公司为java程序员准备的一个实际，垃圾销毁时机。

   如果希望在对象销毁时机执行一段代码的话，这段代码要写在finalize()方法中。

6. 静态代码块的作用是什么？

   static{

      ...

   }

   静态代码块在类加载的时候执行，并且只执行一次，这是一个SUN准备的类加载时机。

   finalize()方法同样是sun为程序员准备的一个时机。

   这个时机是垃圾回收时机。

7. 有段代码可以建议垃圾回收期启动

   `System.gc();   // 建议启动垃圾回收器`

<br><br>

### hashCode方法：

1. 在object中的hashCode方法是怎么样的?

   public native int hashCode();

   这个方法不是抽象方法，带有native关键字，底层调用c++程序。

2. hashCode()方法返回的是哈希值：

   实际上就是java对象的内存地址，经过哈希算法，得出一个值。

   所以hashCode()方法的执行结果可以等同看做一个java对象的内存地址

```java
public class Test{
    public static void main (string[] args){
        
        Object o = new Object();
        int hashCodeValue = o.hashcode();
        //对象内存地址经过哈希算法转换的一个数字。可以等同看做内存地址。
        System.out.println(hashCodeValue);//89641564
      	
        MyClass mc=new MyClass;
        System.out.println(mc.hashCode());//5641864654
    }
}
class MyClass{
    
}
```

<br><br><br><br>

## 匿名内部类：

1. 内部类：在类的内部又定义了一个新的类。被称为内部类。

2. 内部类的分类：

   静态内部类：类似于静态变量

   实例内部类：类似于实例变量

   局部内部类：类似于局部变量

3. 使用内部类编写的代码，可读性差，尽量别用

4. 匿名内部类是局部内部类的一种。因为这个类没有名字而得名，叫做匿名内部类。可以直接实现接口

```java
mm.mySum(new Compute(){
    public int sum(int a,int b){
        return a+b;
}
},200,300);
/*
mm是对象，使用了mySum这个方法
compute是一个接口，不能直接new对象，这里使用了匿名内部类。如果不用匿名内部类，那么需要再创建一个class，去implements接口
*/

//以下是不使用匿名内部类的方法
public class ComputeImpl implements Compute{
    public int sum (int a,int b){
        return a + b;
    }
}
```

<br><br><br><br>

## 数组

1. 数组的优缺点，并且要理解为什么

   1. 空间存储上，内存地址是连续的

   2. 每个元素占用的空间大小相同

   3. 知道首元素的内存地址

   4. 通过下班可以计算出偏移量

      通过一个数学表达式，就可以快速计算出摸个下标为止上元素的内存1地址，直接通过内存地址定位，效率非常高。

   5. 优点：检索效率高。

      缺点随记增删改效率较低，数组无法存储大数据量

      注意：数组最后一个元素增删效率不受影响

      <br>

2. 以为数组的静态初始化和动态初始化

   - 静态初始化：

     `int[] arr = {1,2,3,4};`

     `object[] objs = {new Object(),new Object(),new Object() };`

   - 动态初始化

     `int []arr=new int[4];`

     `Object[] objs=new Object[4]; //元素默认值为null`

     <br>

3. 一维数组的遍历

   <br>

4. 二维数组的静态初始化和动态初始化

   - 静态初始化：

     ```java
     int [] [] arr={
         {1,2,3},
         {2,3,4,4,5,6},
         {2,8}
     };
     Object[][] arr={
         {new Object(),new Object()},
         {new Object(),new Object(),new Object(),new Object()},
         {new Object(),new Object(),new Object(),new Object(),new Object()}
     };
     
     ```

     

   - 动态初始化

     ```java
     int [] []arr = new int[3][4];//三行四列
     Object [] [] arr=new Object[4][4];
     Animal [][] arr=new Animal[3][4];
     //Animal累的数组，里面可以存储Animal对象以及它的子类
     ```

     

     

5. 二维数组的遍历

6. main方法上"String[] args"参数的使用

7. 数组的拷贝：System.arraycopy()方法的使用

   数组长度一旦确定，不可变。

   所以数组长度不够的时候需要扩容，扩容的机制是：新建一个大数组，讲小数组的数据拷贝到大数组，小数组被垃圾回收器回收

   ```java
   public class Selftest {
       public static void main(String[] args) {
           int[] arr={1,2,3,4,5,6,7,8,9,0};
           int[] targetArr=new int[4];
           System.arraycopy(arr,1,targetArr,0,targetArr.length);
           for (int i = 0; i <targetArr.length ; i++) {
               System.out.println(targetArr[i]);
           }
       }
   }
   ```

   

8. 对数组中存储应用数据类型的情况，要会画它的内存结构图

<br><br><br><br>

## Java JDK中内置的一个类：java.lang.String

1. String 表示字符串类型，属于引用数据类型，不属于基本数据类型。

2. 在java中随便使用双引号括起来的对象。例如："abc","hello world"，这是三个String对象

3. java中规定，双引号括起来的字符串是不可变的·，也就是说"abc"从出生到死亡，都不可变。

4. 在JDK当中双引号括起来的字符串。例如：**"abc" "def"都是直接存储在"方法区''的”字符串常量池"当中的。**

   为什么SUN公司把自付出存储在一个“字符串常量池”当中呢，因为字符串在实际开发中使用太频繁，为了执行效率，所以把字符串放到了方法区的字符串常量池当中。

```java
String s1 ="abcdef";
String s2 ="abcdef"+"xy";
/**
↑这两行代码在表示底层创建了3个字符串对象，都在字符串常量池当中
↓这是使用new方法创建的字符串对象。这个代码中的“xy”是从哪里来的？
凡是双引号括起来的都在字符串常量池当中有一份
new对象的时候一定是在堆内存当中开辟空间
*/
String s3 = new String("xy");

//i变量中保持的是100这个值
int i=100;
//s变量中保存的是字符串对象的内存地址
//s引用中保存的不是"abd"，是0x1111
//而0x1111是“abd”字符串对象在“字符串常量池”当中的内存地址。		
String s ="abc"

```

<br><br>

### 关于String类中的构造方法

1. String s = new String("");
2. String s = "";//最常用
3. String s = new String (char数组);
4. String s = new String (char数组，起始下标，长度);
5. String s = new String (byte数组)；
6. String s = new String (byte数组，起始下标，长度)；

<br><br><br><br>

# String 类当中常用方法

1. （掌握） **char charAt(int index)**

   `char c = "中国人".charAt(1);`

   `System.out.printlb(c);//国`

   <br>

2. （了解） **int compareTo(String anotherString)**

   int result = "abc".compareTo("abc");

   System.out.println(result);//0

   //输出0（等于0）-->前后一致

   //输出1（大于0）-->前大后小

   //输出-1（小于0）-->前小后大

   *按字典顺序*

   拿着字符串第一个字母和后面字符串的第一个字母比较。能分胜负就不再比较了。

   System.out.println("xyz".compareTo("yxz"));//-1

   <br>

3. （掌握） **boolean contains(CharSequence s)**

   <u>判断前面的字符串中是否包含后面的字符串。</u>

   System.out.println("hello.java".contains(".java"));//ture。如果没有，则输出false

   <br>

4. （掌握） **boolean endsWith(String suffix)**

   <u>判断当前字符串是否以某个字符串结尾</u>

   System.out.println("test.txt".endsWith("java"));//false

   <br>

5. （掌握） **boolean equals(Object anObject)**

   <u>比较两个字符串鼻血使用equals方法，不能使用“==”</u>

   System.out.println("abc".equals("abc"));//true;

   <br>

6. （掌握） **boolean equalsIgnoreCase(String anotherString)**

   <u>判断两个字符串是否相等，并且同时忽略大小写。</u>

   <br>

7. （掌握） **byte[] getBytes()**

   <u>将字符串转换成字节数组</u>

   byte[] bytes = "abcd".getBytes();

   //遍历以后输出的是 97 98 99 100

   <br>

8. （掌握） int indexOf(String str)**

   <u>判断某个字符串在当前字符串中**第一次**出现处的索引（下标）</u>

   System,out.println("12345java2222".indexOf("java"));//6

   <br>

9. （掌握） lastInt indexOf(String str)**

   <u>判断某个字符串在当前字符串中**最后一次**出现处的索引（下标）</u>

   System,out.println("12345java2222".lastIndexOf("2"));//12

   <br>

10. （掌握） **boolean isEmpty()**

    <u>判断某个字符串是否为"空字符串"</u>

    String s = ""；//不能是null，不然会空指针异常,同时必须为空，就算是空格，返回的也是false；

    System.out.println(s.isEmpty());

    <br>

11. （掌握） **int length()**

    <u>面试题：判断数组长度和判断字符串长度不一样</u>

    <u>判断数组长度是length属性，判断字符串长度是length()方法</u>

    System.out.println("abc".length());//3

    <br>

12. （掌握） **String replace(CharSequence target, CharSequence replacement)**

    <u>String的父接口就是：CharSequence</u>

    String s ="abc";
    String s1=s.replace("a","b");
    System.out.println(s1);

    <br>

13. （掌握） **String [] split(String regex)**

    <u>拆分字符串</u>

    String[] ymd = "2002-6-1".split("-");
    for (int i = 0; i < ymd.length ; i++) {
    System.out.println(ymd[i]);

    //可以继续向下拆分，比如通过"0"拆分

    }//输出2002  6  1

    <br>

14. （掌握） **boolean startWith(String prefix)**

    <u>判断某个字符串是否以某个子字符串开始</u>

    System.out.println("111222333".startWith("111"));//true

    System.out.println("111222333".startWith("123"));//false

    <br>

15. （掌握） **String substring(int beginIndex)**

    <u>截取字符串</u>

    System.out.println("111222333".substring(6));333

    <br>

16. （掌握） **String substring(int beginIndex,int endIndex)**

    <u>beginIndex包括，endIndex不包括</u>

    System.out.println("111222333444".substring(7,10));334【左闭右开】

    <br>

17. （掌握） **char[] toCharArray();**

    <u>将字符串转换成char数组</u>

    char[] chars = "我是中国人".toCharArray();

    //遍历得出结果：我 是 中 国 人

    <br>

18. （掌握） **String toLowerCase()**

    <u>转换成小写</u>

    System.out.println("ABCDef".toLowerCase());

    <br>

19. （掌握） **String toUpperCase()**

    <u>转换成大写</u>

    System.out.println("ABCDef".toLowerCase());

    <br>

20. （掌握） **String trim();**

    <u>去除字符串前后空白</u>

    System.out.println("   sda sdas  ".trim());//sda sdas

    <br>

21. (掌握) .String中有一个方法是静态的不需要new对象

    这个方法叫做valueOf，作用是“非字符串“转换成”字符串”

    String s1 = String.valueOf(true);

    String s1 = String.valueOf(123);

    System.out.println(s1);

    ```java
    //这个静态的valueOf()方法，参数是一个对象的时候，会自动调用这个对象的toString()方法
    String s1 = String.valueOf(new Customer());
    System.out.println(s1);//没有重写toString()方法之前是对象内存地址 
    System.out.println(s1);//我是一个vip客户！
    class Customer{
        //重写toString()方法
        public String toString(){
            return "我是一个vip客户！";
        }
    }
    
    ```

    本质上System.out.println()这个方法在输出任何数据的时候都是先转换成字符串，再输出 

    <br><br><br><br>

## StringBuffer和StringBulider

### StringBuffer

- 如果以后需要进行大量的字符串拼接操作，建议使用JDK自带的：
  java.lang.StringBuffer;
  java.lang.StringBuilder;

- 如何优化StringBuffer的性能？

  在创建StringBuffer的时候尽可能给定一个初始化容量。

  最好减少底层的扩容次数。预估计一下，给一个大一些的初始化容量。

```java
public static void main(String[] args) {
    //创建一个初始化容量为16个byte[]数组（字符串缓冲区对象）
    StringBuffer stringBuffer= new StringBuffer();

    //拼接字符串，以后拼接字符串同意调用append()方法
    //append是追加的意思
    stringBuffer.append("abd");
    stringBuffer.append(3.14);
    stringBuffer.append(true);
    stringBuffer.append(100l);
    //append方法底层在进行追加的时候，如果byte数组慢了，会自动扩容
    stringBuffer.append(100l);
    System.out.println(stringBuffer);//abd3.14true100100

    //指定初始化容量的StringBuffer对象（字符串缓冲区对象）
    StringBuffer sb = new StringBuffer(100);

}
```

<br><br>

### StringBuilder和StringBuffer的异同

- StringBuffer中的方法都有synchronized关键字修饰。表示StringBuffer在多线程环境下运行时安全的。StringBuilder则不安全
- 其余基本一样

<br><br><br><br>

## 8种基本数据类型对应的包装类型及方法

### 基本数据类型名对应的包装类型名

| 基本数据类型 | 包装类型            | 父类   |
| ------------ | ------------------- | ------ |
| byte         | java.lang.Byte      | Number |
| short        | java.lang.Short     | Number |
| int          | java.lang.Integer   | Number |
| long         | java.lang.Long      | Number |
| float        | java.lang.Float     | Number |
| double       | java.lang.Double    | Number |
| boolean      | java.lang.Boolean   | Object |
| char         | java.lang.Character | Object |

- 以上八种包装类中，重点以java.lang.Integer为代表进行学习，其他的类型照葫芦画瓢就行

### 拆装箱及方法

- 八种包装类其中6个都是数字对应的包装类，他们的父类都是Nunber可先研究下Number中公共的方法：

  Number是一个抽象类，无法实例化对象

  Number类中有这样的方法：

  | **Modifier and Type** |                  **Method and Description**                  |
  | --------------------- | :----------------------------------------------------------: |
  | byte                  | `byteValue()`返回指定号码作为值 `byte` ，这可能涉及舍入或截断。 |
  | abstract double       | `doubleValue()`返回指定数字的值为 `double` ，可能涉及四舍五入。 |
  | abstract float        | `floatValue()`返回指定数字的值为 `float` ，可能涉及四舍五入。 |
  | abstract int          | `intValue()`返回指定号码作为值 `int` ，这可能涉及舍入或截断。 |
  | abstract long         | `longValue()`返回指定数字的值为 `long` ，可能涉及四舍五入或截断。 |
  | short                 | `shortValue()`返回指定号码作为值 `short` ，这可能涉及舍入或截断。 |

<br>

```java
public static void main(String[] args) {
    //123这个基本数据类型，进行构造方法的包装打到了：基本数据类型向引用数据类型的转型；
    //基本数据类型（转换为）引用数据类型：装箱
    Integer i = new Integer(123);

    //引用数据类型（转换为）基本数据类型：拆箱
    float f = i.floatValue();
    System.out.println(f);//123.0
    int revalue = i.intValue();
    System.out.println(revalue);//123

}
```

- 关于Integer类的构造方法，有两个：

  Integer(int)

  Integer(String)

- 通过访问包装类的常量，来获取最大值和最小值：

  System.out.println(Inter.MAX_VALUE);

  System.out.println(Inter.MIN_VALUE);
  
  <br><br>

### JDK1.5以后，支持自动拆装箱：

- 自动装箱：int-->Integer

  Integer x = 100;

- 自动拆箱：Integer-->int

  int y = x;

  <br><br><br><br>

## 关于方法权益中的“整数型常量池”

java中为了提高程序的执行小效率，讲[-128,127]之间的所有数据提前创建好，放到一个方法去的”整数型常量池“当中了，目的是只要这个区间的数据不需要再new了，可以直接从整数型常量池中取出来。 



## Integer常用方法

1. **static int parseInt(String s)**

   <u>静态方法，传参String，返回int</u>

   parse方法，把字符串转化成基本数据类型

   int retValue = Integer.parseInt("123");//123,String-->int

   照葫芦画瓢：

   double retValue2 = Double.parseDouble("3.14");

   System.out.println(retValue2+1); //4.140000000000001(精度问题)

   
   
2. **valueOf**方法作为了解

   static Integer valueOf(int i)

   <u>静态的：int -->Integer</u>

   `Integer i1 = Integer.valueOf(100);`

   <br>

   static Integer valueOf(String s)

   <u>静态的：String-->Integer</u>

   `Integer i2 = Integer.valueOf("100");`

   <br><br><br><br>

## String int Integer三种类型的互相转换：

```java
public static void main(String[] args) {
  //String -->int
    String s1="100";
    int i1 = Integer.parseInt(s1);
    System.out.println(i1+1);//101

  //int -->String
    String s2 = i1+"";
    System.out.println(s2+1);//1001
        long i=System.currentTimeMillis();
        Integer ig = new Integer(100);
        String s= String.valueOf(i);//long-->String
        String s2 = String.valueOf(ig);//Integer-->String

  //int -->Integer
  //自动装箱
  Integer x=1000;

  //Integer -->int
  //自动拆箱
  int y = x;
  //String -->Integer
    Integer k = Integer.valueOf("123");

  //Integer -->String
    String s = String.valueOf(k);


}
```

   <br><br><br><br>

## java对日期的处理：

1. 怎么获取系统当前时间

2. String -->Date

3. Date -->String

   注意：字符串的日期格式和SimpleDateFormat对象指定的日期格式要一直，不然会出现异常

```java
   import java.text.SimpleDateFormat;
   import java.util.Date;
   
   
   public class Main {
       public static void main(String[] args) throws Exception {
        //获取系统当前时间
           Date nowTime = new Date();
           System.out.println(nowTime);//Thu Feb 04 23:20:25 CST 2021
   
           /**
            * 日期可以格式化吗？
            * 将日期类型Date,按照指定格式进行转换：Date (转换成具有一定格式的日期字符串) -->String
            * SimpleDateFormat是java。text包下的。专门负责日期格式化
            * yyyy年
            * MM月
            * dd日
            * HH小时
            * mm分
            * ss秒
            * SSS毫秒
            */
           SimpleDateFormat sdf = new SimpleDateFormat("yyy-M-d H:m:s:S");//格式随意组织，特殊字母不能动,这里的字母是java定好的，不能动
           String nowTimeStr = sdf.format(nowTime);
           System.out.println(nowTimeStr);
   
           //String-->Date
           String time = "2008-08-08 08:08:08 888";
           SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-M-d H:m:s S");
           //上面两个格式必须相同
           Date dateTime = sdf2.parse(time);
           System.out.println(dateTime);
       }
   }
```

<br><br><br><br>

## System类的相关属性和方法：

- System.out【out是System类的静态变量】
- System.outt.println() 【println()方法不是System类的，是PrintStream类的方法】
- System.gc() 建议启动垃圾回收器
- System.currentTimeMillis() 获取1970年1月1日到系统当前时间的总毫秒数
- System.exit(0) 退出JVM

 <br><br><br><br>

## 关于数字的格式化

由于用的是在太少，想了解的点击https://www.bilibili.com/video/BV1Rx411876f?p=623&spm_id_from=pageDriver的623集，笔记就不做了。

 <br><br><br><br>

## BigDecimal

1. BigDecimal 属于大数据，精度极高。不属于基本数据类型，属于java对象（引用数据类型）。

   这是SUN提供的一个类。专门用在财务软件当中

   

2. 问：你处理过财务数据吗？用的哪一种类型？

   答：java.math.BigDecimal

 <br><br><br><br>

## 随机数

- 第一种方法

  ```java
  Random random=new Random();
  int num=random.nextInt();//任何数
  int num2=random.nextInt(6);//0到5任何整数
  ```

- 第二种方法

  第二种方法返回的数值是[0.0,1.0）的double型数值，由于double类数的精度很高，可以在一定程度下看做随机数，借助（int）来进行类型转换就可以得到整数随机数了，代码如下。

  ```java
  public static void main(String[] args)  {
      int max=100,min=1;
      int ran2 = (int) (Math.random()*(max-min)+min);
      System.out.println(ran2);
  }
  ```

 <br><br><br><br>

## 枚举：

1. 一枚一枚可以列举出来的，才建议使用枚举类型
2. 枚举编译后产生的也是class文件
3. 枚举也是一种引用数据类型
4. 枚举中的每一个值可以看做是常量

```java
public class Main {
    public static void main(String[] args) {
        Result r = divide(10,3);
        System.out.println(r);
            System.out.println(r==Result.SUCCESS ? "计算成功":"计算失败");

    }
    public static Result divide(int a,int b){
        try{
            int c = a/b;
            return Result.SUCCESS;
        }catch(Exception e){
            return Result.FAIL;
        }

    }

enum Result{
        SUCCESS,FAIL
    }
}
```

 <br><br><br><br>

## 异常

1. 什么事异常，java提供异常处理机制有什么用？

   以下程序执行过成功发生了不正常的情况，这种不正常的情况叫做：异常

   java是很完善的语言，提供了异常的处理方式，以下程序执行过程中出现了不正常的情况

   java把改异常信息打印输出到控制糖，供程序员参考。程序员看到异常信息后，可以对程序进行修改，让程序更加健壮。

    <br><br>

2. java语言中异常是以什么形式存在的呢？

   1. 异常在java中以类的形式存在，每一个异常类都可以创建对象。

   2. 异常对应的现实生活中是怎么样的

      火灾（异常类）：

      2008年8月8日，小明家着火了（异常对象。

      2008年8月9日，小刚家着火了（异常对象。

      2008年9月8日，小红家着火了（异常对象。

      <br>

      类是：模板

      对象是：实际存在的个体

      <br>

      钱包丢了（异常类）

      2008年1月8日，小明的钱包丢了（异常对象）

       <br><br>

3. java的异常处理机制：

   1. 异常在java中以类和对象的形式存在。那么异常的继承结构是怎么样的？

      我们可以使用UML图来描述一下继承结构。画UML图有很多工具，例如:Rational Rose 、starUML等

      object

      object下有Throwable（可抛出的）

      Throwable下面有两个分支：Error(不可处理，直接退出JVM)和Exception(可处理的)	

      Excepion下面有两个分支

      1. **Exception的直接子类：编译时异常**（要求程序员在编写程序阶段必须预先对这些异常进行处理，如果不处理编译器就会报错，因此得名编译时异常）
      2. **RuntimeException：运行时异常。**（在编写程序阶段程序员可以预先处理也可以不管，都行）

      <br>

   2. 编译时异常和运行时异常，都是发生在运行阶段，编译阶段异常是不会发生的。

      所有异常都是在运行阶段发生的，因此只有程序运行阶段才可以new对象。

      因此异常的发生就是inew异常对象。

      <br>

   3. 编译时异常和运行时异常的区别？

      1. **编译时异常一般发生的概率比较高//  又叫受检异常或受控异常**

      - 例：你看到外面下雨了，倾盆大雨的。

        你出门之前预料到：如果不打伞，我可能会生病（生病是一种异常）

        而且这个异常发生的概率比较高，所以我们出门之前要拿一把伞。

        “拿一把伞”就是对“生病异常”发生之前的一种处理方式

      - 对于一些发生概率较高的异常，需要在运行之前对其进行预处理。

        <br>

      2. **运行时异常一般发生的概率比较低//  又叫未受检异常或非受控异常**

      - 例：小明走在大街上，可能会被天上的飞机轮子砸到。

        被飞机轮子砸到也算一种异常。

        但是这种异常发生概率较低。

        在出门之前你没必要提前对这种发生概率较低的异常进行预处理。

        如果你预处理这种异常，你讲获得很累。

        <br>

      3. 假设java中没有对异常进行划分，没有分为：编译时异常和运行时异常，所有异常需要再编写程序阶段对其进行预处理，是怎么样的效果呢？

      - 首先，如果这样的话，程序肯定绝对安全的

        - 但是程序员编写程序太累的，代码到处都是处理异常的代码。	

        <br><br><br><br>

### java语言中对异常的处理包括两种方式：

   1. 在方法声明的位置上使用throws关键字，抛给上一级

   谁调用我，我就抛给谁。抛给上一级。

注意：**子类不能抛出比父类更宽泛/更多的异常，比如继承了一个父类，但是这个父类没有thorws过异常，所以这个子类只能try..catch**

   2. 使用try...catch语句进行异常的捕捉

   这件事发生了，谁也不知道，因为我给抓住了

   举个例子：
    
   - 我是某集团的一个销售员，因为我的失误，导致公司损失了1000元
   - “损失”1000元，可以看做一个异常发生了。我有两种处理方式
   - 第一种方式：我把这件事告诉领导【异常上抛】
   - 第二种方式：我自己掏腰包把这钱补上【异常捕捉】

   **java中异常发生后如果一直上抛，最后抛给了main方法，main方法继续上抛，抛给了调       用者JVM，JVM知道这个异常发生，只有一个结果。终止java程序的执行。**
    
   注意：只要异常没有捕捉，采用上报的方式，此方法的后续代码不会执行。
    
   另外需要注意，try语句块的某一行出现异常，该行后面的代码不会执行。
    
   try..catch捕捉后，后续代码可以执行。

   <br><br>

### 深入try..catch

   1. catch后面的小括号中的类型可以是具体的异常类型，也可以是该异常类型的父类型。
   2. catch可以写多个。建议catch的时候，精确的一个一个处理，这样有利于程序的调试。
   3. catch写多个的时候，从上到下，必须遵守从小到大

   <br><br>

### 上报和捕捉的选择：

   如果希望调用者来处理，选择throws上报

   <br><br>

   异常对象有两个非常重要的方法：、

   1. 获取异常简单的描述信息：
   
      String msg = exception.getMessage();
   
   2. 打印异常追踪的栈堆信息：
   
      exception.printStackTrace();【exception是对象】
      
       <br><br>

### 关于try..catch中的fianlly子句

   1. 在finally子句中的代码是最后执行的，并且是一定会执行，即使try语句块中的代码出现了异常。
   
      finally子句必须和try一起出现，不能单独编写。
   
   2. finally语句通常使用在哪些情况下呢？

 <br><br>

### java中任何自定义异常

两步：

1. 编写一个类继承Exception或者RuntimeException
2. 提供两个构造方法，一个无参数的，一个带有String

```java
public class MyException extends Exception{
    public MyException(){
        
    }
    public MyException(String s){
        super(s);
    }
}

public static void main(String[] args){
    MyException e = new MyException("用户民不能为空！");
    e.printStackTrace();//打印异常堆栈信息
    
    //获取异常简单描述信息
    String msg = e.getMessage();
    System.out.print(msg);
}
```

<br><br><br><br>

## 集合概述

1. 什么事集合？有啥用？

   数组其实就是一个集合。集合实际上就是一个容器，可以用来容纳其他类型的数据。

   集合为什么说在开发中使用较多？

   - 集合是一个容器，一个载体，可以一次容纳多个对象，在实际开发中，假设连接数据库，数据库当中有10条记录，那么假设把这10条记录查询出来，在java程序中会将10条数据封装成10个java对象，然后将10个java对象放到某一个集合当中，将集合传动前端，然后遍历集合，将一个数据一个数据展现出来。

     <br>

2. 集合不能直接存储数据类型，另外集合也不能直接存储java对象，集合当中存储的都是ijava对象的内存地址。（或者说集合中存储的是引用

   - list.add(100);//自动装箱Integer
   - 注意：集合在java中本事是一个容器，也是一个对象。集合在任何时候存储的都是引用。

   <br>

3.  在java中年每一个不同的集合，底层会对应不同的数据结构。往不同的集合中存储元素，等于将数组放到了不同的数据结构当中。什么是数据结构？数据存储的结构就是数据结构。不同的数据结构，数据存储的方式不同。例如：

   - 数组、二叉树、链表、哈希表...图

   - 以上这些都是常见的数据结构

   - 使用不同的集合等同于使用了不同的数据结构。

   - 往集合c1中放数据，可能放到数组上了，往集合c2中放数据，可能放到二叉树上了....

   - 在这一章节，需要掌握的不是精通数据结构。java中已经将数据结构实现了，已经写好了这些常用的集合类，你只需要掌握掌握怎么用。在什么情况下选择哪一种合适的集合去使用即可。

   - new ArrayList();创建一个集合对象，底层是数组

   - new LinkedList();创建一个集合对象，底层是链表

   - new TreeSet();创建一个集合对象，底层是二叉树 

     <br>

4. 集合在java JDK中哪个包下？

   java.util.*;  所有的集合类和集合接口都在java.uti包下。 

5. 最后能将集合的继承结构图背会。

   集合整个这个体系是怎么样的一个结构，你需要有印象。

6. 在java中结合分为两大类

   - 一类是单个方式存储元素：

     单个方式存储元素，这一类集合中超级父接口：java.util.Collection;

   - 一类是以键值对儿的方式存储元素，这一类集合中的超级父接口：java.util.Map;




​       <br><br> <br><br>

## 关于java.util.Collection接口中常用的方法

1. Collection中能存放什么元素？

   没有使用泛型之前，Collection中可以存储Object的所有子类型。

   使用了“泛型”后，Collection中只能存储某个具体的类型。

   集合后期我们会学习“泛型”的语法，目前先不用管。Collction中什么都能存，只要是Object的子类型就行。（集合中不能直接存储基本数据类型，也不能存java对象，只是存储java对象的内存地址。）

2. Collection中的常用方法

   `boolean add(Object e)` 向集合中添加元素

   `int size()` 获取集合中元素的个数

   `void clear()`清空集合

   `boolean contain(Object o)` 判断当前集合中是否包含元素o，包含返回true，不包含返回false

   `boolean remove(Object 0)` 删除集合中的某个元素

   `boolean isEmpty()` 判断集合中元素的个数是否为0

   `Object[] toArray()` 调用这个方法可以把集合转换成数组。

3. 迭代集合

   ```java
   Collection c  new HashSet();
   c.add("abc");
   c.add("def");
   c.add(100);
   c.add(new Object());
   //进行迭代
   //第一步，获取集合对象的迭代器对象Iterator
   Iterator it = c.iterator();
   //第二步，通过以上获取的的迭代器对象开始迭代集合
   /*
   一以下两个方法是迭代器对象Iterator中的方法
   boolean hasNext()如果仍有元素可以迭代，则返回true。
   Objext next()返回迭代的下一个集合。
   */
   while(it.hasNext()){
       Object obj = it.next();
       System.out.println(obj);
       
   /*存进去是什么类型，取出来就是什么类型，
   只不过输出的时候回转换成字符串。因为println会调用toString()方法。
      */
   }
   ```
   
   总结重点：
   
1. 把集合继承结构图背会。
   
   2. 把Collection接口中常用方法测试几遍
3. 把迭代器弄明白
   4. Collection接口中的remove方法和contain方法底层都会调用（重写）equals。
   
   
   
    <br><br>

### 关于集合元素的remove

- 重点：当集合的结构发生改变是，迭代器萹蓄重新获取，如果还是用以前老的迭代器，会出现异常：java.util.ConcurrentModificationException

- 重点：在迭代集合元素的过程中，不能调用集合对象的remove方法，删除元素：

  c.remove(0); 迭代过程中不能这样
  
  否则会出现 java.util.ConcurrentModificationException
  
- 重点：在迭代元素的过程中，一定要使用迭代器的Itertator的remove方法，删除元素，不要使用集合自带的remove方法。

```java
while(it2.hasNext()){
    Object o = it2.next();
    /*
    chu出异常的根本原因：集合中元素删除了，但没有更新迭代器（迭代器不知道集合变化了）
    c2.remove(o)； 直接通过集合去删除元素，没有通知迭代器。（导致迭代器的快照和原集合状态不同
    */
    //使用迭代器去删除时，会自动更新迭代器，并且更新集合（删除集合中的元素）
   it2.remove();//删除的一定是迭代器指向的当前元素
    System.out.println(o);
}
```



 <br><br> <br><br>

## 关于java.util.list接口中常用的方法      

1. ​      List集合存储元素的特点：有序可重复

   有序：List集合中的元素有下标。

   从0开始，以1递增

   可重复：存储一个1，还可以在存储1

2. List接口既然是Collection接口的子接口，那么肯定List接口有自己的"特色"的方法(以下只列出list特有的，常用的方法)

   - `void add(int index，E element)` 在列表的指定位置插入指定元素（第一个参数是下标）（后面的元素依次顺移一位）
   - `Object get(int index)` 按下标获取元素
   - `int indexOf(Object o)` 返回指定对象第一次出现处的索引
   - `int lastIndexOf(Object o) `返回指定对象最后一次出现处的索引
   - `Object remove(int index)` 删除指定下标位置的元素
   - `Object set(int index, E element)` 修改指定位置的元素


 <br><br> <br><br>

## ArrayList集合：

1. 默认初始化容量为10（底层先创建了一个长度为0的数组，当前添加第一个元素的时候初始化容量10）

2. 集合底层是一个Object[]数组

3. 构造方法：

   `new ArrayList();`

   `new ArrayList(20);`

4. ArrayList集合的扩容：

   增长到原容量的1.5倍。

   ArrayList集合底层是数组，怎么优化：尽可能少的扩容。因为数组扩容效率比较低，建议在使用ArrayList集合的时候预估元素的个数，给定一个初始化容量。

5. 这么多集合中，用哪个集合最多？

   ArrayList集合

   因为往数组末尾添加元素，效率不受影响。

   另外，我们检索、查找某个元素的操作比较多

6. 另一个构造方法

   ```java
   Collection c = new HashSet();//创建HashSet集合
   c.add(100);
   c.add(200);
   c.add(900);
   //通过这个构造方法可以将HashSet结合转换成List集合
   List myList3 = new ArrayList(c);
   for(int i = 0 ; i < myList3.size() ; i++){
       System.out.println(myList.get(i));
   }
   ```


 <br><br> <br><br>



## 链表：

- 优点：

  由于链表上的元素在空间存储上的内存地址不连续。

  所有随记增删元素的时候不会有大量的元素位移，因此随记增删效率较高。

  在以后的开发中，如果遇到随机增删集合中元素的业务比较多时，建议使用LinkedList。

- 缺点：

  不能通过数学表达式计算被查找元素的内存地址，每一次查找都是从头节点开始遍历，直到找到为止。所以LinkedList集合检索、查找的效率较低。

  <br>

- ArrayList 数组的特点，把检索发挥到机制（但ArrayList是非线程安全的。不是线程安全的集合。）

- LinkedList 链表特点（双向链表），把随机增删发挥到机制

  LinkedList有初始容量吗？没有

  最初这个链表中没有任何元素。first和last引用都是null

  不管是LinkedList还是ArrayList，以后写代码不需要关心具体是哪个集合。

  因为我们要面向接口编程，调用的方法都是接口中的方法

 <br><br> <br><br>

## Vector

1. 底层也是一个数组

2. 初始化容量：10

3. 怎么扩容的？

   扩容之后是原容量的2倍

4. ArrayList集合扩容特点：

   ArrayList集合扩容是原容量的1.5倍

5. Vector所有方法都是线程同步的，都带有synchronized关键字，是线程安全的。效率比较低，使用较少了。

6. 怎么讲一个线程不安全的ArrayList集合转换成线程安全的呢？

   使用集合工具类：

   java.util.Collections;

   java.util.Collection是集合接口；java.util.Collections是集合工具类

   ```java
   List myList = new ArrayList();//非线程安全的
   //变成线程安全的
   Collections.synchronizedList(myList);
   
   //myList集合线程就是安全的了
   myList.add("111");
   myList.add("222");
   myList.add("333");
   ```

    <br><br> <br><br>

## 泛型

1. JDK5.0之后推出的新特性：泛型

2. 泛型这种语法机制，只在程序编译阶段起作用，只是给编译器参考的。（运行阶段泛型没用！）

   ```java
   //创建Animal Cat Bird类的代码省略
   List<Animal> myList = new ArrayList<Animal>();
   cat c = new Cat();
   Bird b = new Bird();
   myList.add(c);
   myList.add(b);
   /*
   JDK8之后，ArrayList<这里的类型会自动推断>(),前提是JDK8之后才允许
   自动类型推断，钻石表达式
   List<Animal> myList = new ArrayList<>();
   */
   Iterator<Animal> it = myList.iterator();
   while(it.hasNext()){
       Animal a = it.next();
       
   }
   ```

3. 使用了泛型的好处是什么？

   - 集合中存储的元素类型同意了。
   - 从集合中取出的元素类型是泛型指定的类型，不需要进行大量的“向下转型”

4. 泛型的缺点是什么？

   - 导致集合中存储的元素缺乏多样性！
   - 大多数业务中，集合中元素的类型还是统一的。所以这种·泛型特性被大家所认可

5. 自定义泛型可以吗？可以

   - 自定义泛型的时候，<>尖括号中的是一个标识符，随便下。

   - java源代码中经常出现的是：`<E>`和`<T>`

     E:Element元素

     T:Type类型

 <br><br> <br><br>

## 增强for循环：foreach

```java
/*
for(元素类型 变量名 : 数组或集合){
 System.out.println(变量名);
}
*/
int [] arr ={1,2,3,4}
for(int data : arr){
    //data就是数组中的元素
    System.out.println(data);
}
```

 <br><br> <br><br>

## Set下的子类

- HashSet集合：无序不可重复

- TreeSet集合存储元素的特点：

  1. 无序不可重复，但是存储的元素可以自动按照大小顺序排序！ 

     称为：可排序集合

  2. 无序：这里的无序指的是存进去的顺序和取出来的顺序不同。并且没有下标
  
   <br><br> <br><br>

## java.util.Map接口中常用的方法

1. Map和Collection没有继承关系

2. Map集合以 key 和 value 的方式存储数据：键值对

   key和value都是引用数据类型，都是存储对象的内存地址

   key起到主导地位，value是key的一个附属品

3. Map接口中常用方法

   - `V put(K key，V vaLue)` 向Map集合中添加键值对

   - `V get(Object key)`  通过key获取vaLue

   - `void clear()` 清空Map集合

   - `boolean containsKey(Object key)` 判断Map中是否包含某个key

   - `boolean containsVaLue(Object vaLue)` 判断Map中是否包含某个value

   - `boolean isEmpty()` 判断Map集合中元素个数是否为0

   - `Set<K> keySet()` 获取Map集合所有的key（所有的键是个set集合）

   - `V remove(Object key)`通过key删除键值对

   - `int size()`获取Map集合中键值对的个数

   - `ColLection<V> vaLues()` 获取Map集合中所有的value，返回一个Collection

     <br>

   - `Set<Map.Entry<K,V>> entry Set()` 将Map集合转换成set集合

     例：`Set<Map.Entry<Integer,String>> set = map.entry Set()` 

     假设现在有有一个Map集合 "map1"

     Set set = map1.entrySet();

     【注意：Map集合通过entrySet()方法转换成的这个Set集合，Set集合中元素的类型是Map.Entry<K,V>】

     【Map.Entry和String一样，都是一种类的名字，只不过：Map.Entry是静态内部类，是Map中的静态内部类】

     ```java
      package SelfTest;
      import java.util.HashSet;
      import java.util.Set;
      
      public class Myclass{
          //声明一个静态内部类
          public static class InnerClass{
              //静态方法
              public static void m1(){
                  System.out.println("静态内部类的m1方法实行" );
              }
              //实例方法
              public void m2(){
                  System.out.println("静态内部类中的实例方法执行！");
              }
          }
      
          public static void main(String[] args) {
              //类名叫做：MyClass.InnerClass
              Myclass.InnerClass.m1();
      
              //创建静态内部类对象
              Myclass.InnerClass mi = new Myclass.InnerClass();
              mi.m2();
      
              //给一个Set集合
              //该Set集合中存储的对象是Myclass.InnerClass类型
              Set<Myclass.InnerClass> set = new HashSet<>();
      
              //这个Set集合中存储的是字符串对象
              Set<String> set2 = new HashSet<>();
              Set<MyMap.MyEntry<Integer,String>> set3= new HashSet<>();
          }
      }
      class MyMap{
          public static class MyEntry<K,V>{
      
          }
      }
     ```

     


4. 遍历Map集合（重点！！！）遍历集合

   - 第一种方式：

     获取所有·的key，通过遍历key，来遍历value

     获取所有key，所有key是一个Set集合

     `Set<Integer> keys = map.keySet();`

     通过key获取value

   - 第二种方式：

     `Set<Map.Entry<K,V>> entrySet()`

     以上方法就是把Map集合直接全部转换成Set集合

     Set集合中的元素的类型是：Map.Entry

     <br>

   ```java
   package SelfTest;
   
   import java.util.HashMap;
   import java.util.Iterator;
   import java.util.Map;
   import java.util.Set;
   
   public class MapTest01 {
       public static void main(String[] args) {
           Map<Integer,String> map = new HashMap<>();
           map.put(1,"zs");
           map.put(2,"ls");
           map.put(3,"ww");
           map.put(4,"zl");
           /**
            * 第一种方法 获取所有key，所有key是一个Set集合
            */
           Set<Integer> keys = map.keySet();
           //foreach
           for (Integer key: keys) {
               System.out.println(key + "=" + map.get(key));
           }
           //迭代器
           Iterator it = keys.iterator();
           while(it.hasNext()){
               Object o = it.next();
               System.out.println(o + "=" + map.get(o));
           }
           /**
            * 第二种方法 把Map集合直接全部转换成Set集合
            */
           Set<Map.Entry<Integer,String>> set = map.entrySet();//将Map集合变成Set集合
           //迭代器
           Iterator<Map.Entry<Integer,String>> it2 = set.iterator();
           while(it2.hasNext()){
               Map.Entry<Integer,String> node = it2.next();
               Integer key = node.getKey();
               String value = node.getValue();
               System.out.println(key+"="+value);
           }
           //foreach
           for (Map.Entry<Integer,String> node : set) {
               System.out.println(node.getKey()+"="+node.getValue());
           }
       }
   }
   ```

 <br><br> <br><br>

## HshMap集合

1. HashMap集合底层是哈希表/散链表数据结构

2. 哈希表是一个怎样的数据结构？

   哈希表是一个数组和单向链表的结合体

3. HashMap集合的key部分特点：

   无序，不可重复

   为什么无序？因为不一定挂到哪个单向链表上。

   不可重复是怎么保证的？equals方法来保证HashMap集合的key不可重复。

   如果key重复了，value会覆盖

   <br>

   放在HashMap集合key部分的元素其实就是放到HashSet集合中了。

   所以HashSet集合中的元素也需要同时重写hashCode()+equals()方法

4. 哈希表HashMap使用不当时无法发挥性能！

   要求散列分布均匀，需要你重写hashCode()方法时有一定的技巧。

5. 重点：放在HashMap集合key部分的元素，以及放在HashSet集合中的元素，同时重写equals方法和hashCode方法

6. HashMap集合的默认初始化容量为16，默认加载因子是0.75

   这个默认加载因子是当HashMap集合底层数组的容量达到75%的时候，数组开始扩容，扩容之后是原容量的2倍

   重点：HashMap初始化容量必须是2的倍数，这是为了散列分布均匀，提高HashMap集合的存储效率，所必须的。

 <br><br>

### HashMap的equals方法和hashCode方法重写

1. 向Map集合中存以及取，都是先调用key的hashCode方法，然后再调用equals方法！

   equals方法有可能调用，也有可能不调用

   比如put(k,v):k.hashCode()方法返回哈希值，哈希值经过哈希算法转换成下标，数组下标位置如果是null，equals不需要执行，get(k)方法同理

2. 注意：如果一个类的equals方法重写了，那么hashCode()方法必须重写。

   并且equals方法返回如果是true，HashCode()方法返回的值必须一样。

   - equals方法返回true表示两个对象相同，在同一个单向链表上比较，那么对于同一个单向链表的节点来说，他们的哈希值是相同的，所以hashCode()方法返回值也应该相同

3. hashCode()方法和equals()方法不用研究了，直接使用IDEA工具生成，不过这两个方法需要同时生成

4. 对于哈希表数据结构来说：

   如果o1和o2的值先沟通，一定是放在同一个单向链表上。

   当然如果o1和o2的hash值不同，但由于哈希算法，执行结束之后转换的数组下标可能相同，此时会发生“哈希碰撞”

5. 终极结论：放在HashMap集合key部分，以及方法HashSet集合中的元素，需要同时重写hashCode方法和equals方法。

 <br><br> <br><br>

## Properties类

- 目前只需要掌握Properties属性类对象的相关方法即可。

- Properties是一个Map集合，继承Hashtable，Properties的key和value都是String类型

- Properties被称为属性类对象

- Properties是线程安全的

- 需要掌握Properties的两个方法，一个存一个取

  ```java
  package SelfTest;
  
  import java.util.Properties;
  
  public class PropertiesTest {
      public static void main(String[] args) {
          //创建一个Properties对象
          Properties pro = new Properties();
          //存
          pro.setProperty("zd","nb");
          pro.setProperty("xjb","sb");
          //通过key获取value
          String zd = pro.getProperty("zd");
          String xjb = pro.getProperty("xjb");
          System.out.println(zd);
          System.out.println(xjb);
  
  
      }
  }
  ```

 <br><br> <br><br>

## TreeSet和TreeMap

1. TreeSet集合底层实际上是一个TreeMap

2. TreeMap集合底层是一个二叉树

3. 放在TreeSet集合中的元素，等同于放到TreeMap集合key部分了

4. TreeSet集合中的元素：无序不可重复，但是可以按照元素的大小顺序自动排序（升序）。

   称为：可排序集合
   
5. TreeSet无法对自定义类型排序

   - 放在TreeSet集合中的元素需要实现java.lang.Comparable接口

   - 并且实现compareTo方法，equals可以不写

     ```java
     class Customer implements Comparable<Customer>{
         ...
         public int compareTo(Customer c){
             ...
         }
     }
     ```

   - 怎么写比较规则

     compareTo方法的返回值很重要：

     返回0表示相同，value会覆盖

     返回>0，会继续在右子树上找，

     返回<0，会继续在左子树上找。

     ```java
     public int compareTo(vip v){
         int (this.age==v.age{
          //年龄相同时按照名字排序
          //姓名是String类型，可以直接比。调用compareTo来完成比较
             return this.name.compareTo(v.name);
         }else{
             return this.age - v.age;
         }
     }
     ```

   - 实现比较器接口

     ```java
     package SelfTest;
     import java.util.Comparator;
     import java.util.TreeSet;
     
     //Tree集合中元素课排序的第二种方式：使用比较器
     public class TreeMapTest {
         public static void main(String[] args) {
             //创建Tree集合的时候，需要使用这个比较器
             //通过构造方法传递一个比较器进去
     
             //给构造方法传递一个比较器
             TreeSet<WuGui> wuGuis = new TreeSet<>(new WuGuiComparator());
             
     
             wuGuis.add(new WuGui(1000));
             wuGuis.add(new WuGui(800));
             wuGuis.add(new WuGui(810));
             for(WuGui wuGui:wuGuis){
                 System.out.println(wuGui);
             }
         }
     }
     class WuGui{
         int age;
     
         public WuGui() {
         }
     
         public WuGui(int age){
             this.age=age;
         }
     
         @Override
         public String toString() {
             return "WuGui{" +
                     "age=" + age +
                     '}';
         }
     }
     //单独在这里编写一个比较器
     //比较器实现java.util.Comparator接口。（Comparable是java.lang包下的  。Comparator是java1。util包下的。Comparable是第一种方式）
     class WuGuiComparator implements Comparator<WuGui> {
     
         @Override
         public int compare (WuGui o1,WuGui o2) {
             return o1.age-o2.age;
         }
     
     }
     ```

   - ```java
     /*
     
     使用匿名内部类的方式，作用和上面一样
     
     */
     package SelfTest;
     
     import java.util.Comparator;
     import java.util.TreeSet;
     
     //Tree集合中元素课排序的第二种方式：使用比较器
     public class TreeMapTest {
         public static void main(String[] args) {
             //创建Tree集合的时候，需要使用这个比较器
             //通过构造方法传递一个比较器进去
     
             //给构造方法传递一个比较器,匿名内部类
             TreeSet<WuGui> wuGuis = new TreeSet<>(new Comparator<WuGui>() {
                 @Override
                 public int compare(WuGui o1, WuGui o2) {
                     return o1.age-o2.age;
                 }
             });
     
     
             wuGuis.add(new WuGui(1000));
             wuGuis.add(new WuGui(800));
             wuGuis.add(new WuGui(810));
             for(WuGui wuGui:wuGuis){
                 System.out.println(wuGui);
             }
         }
     }
     class WuGui{
         int age;
     
         public WuGui() {
         }
     
         public WuGui(int age){
             this.age=age;
         }
     
         @Override
         public String toString() {
             return "WuGui{" +
                     "age=" + age +
                     '}';
         }
     }
     ```

6. 最终结论：放TreeSet或者TreeMap集合key部分的元素要想做到排序，包括以下两种方式：

   1. 放在集合中的元素实现java.lang.Comparable接口
   2. 在构造TreeSet或者TreeMap集合的时候给它传一个比较器对象

7. Comparable和Comparator怎么选择呢？

   - 当比较规则不会发生改表的时候。或者说比较规则只有一个的时候，建议实现Comparable接口

   - 如果比较规则有多个，并且是需要多个比较规则之间频繁切换，建议使用Comparator接口

     Comparator接口的设计符合OCP原则

 <br><br> <br><br>

## Collections工具类

注意：java.util.Collection 集合接口；java.util.Collections集合工具类，方便集合的操作

 <br><br> <br><br>

## IO流，什么是IO?

- I:input ; O:output

- java IO流这块有四大家族：

  四大家族首领：

  java.io.InputStream 字节输入流

  java.io.OutputStream 字节输出流

  <br>

  java.io.Reader 字符输入流

  java.io.Writer 字符输出流

  <br>

  四大家族的首领都是抽象类。（abstract class）

- **所有流**都实现了：java.io.Closeable接口，都是可关闭，都有close（）方法。

  养成好习惯，用完流一定要关闭。

- **所有的输出流**都实现了java.io.Flushable接口，都是可刷新的，都有flush()方法。养成好习惯，输出流在最终输出之后，一定要记得flush()刷新一下，表示将通道、管道当中剩余未输出的数据，强行输出玩（清空管道！）刷新的作用就是清空管道。

  如果没有flush()可能会导致丢失数据

- **注意：在java中只要“类名”以Stream结尾的都是字节流。以"Reader/Writer"结尾的都是字符流**	

- java.io包下需要掌握的流有16个：

  **文件专属**：

  java.io.FileInputstream

  java.io.FileOutputstream

  java.io.FileReader

  java.io.FileWriter

  <br>

  **转换流**：（将字节流转换成字符流）

  java.io.InputStreamReader

  java.io.OutputStreamWriter

  <br>

  **缓冲流专属**：

  java.io.BufferedReader

  java.io.BufferedWriter

  java.io.BufferedInputStream

  java.io.BufferedOutputStream

  <br>

  **数据流专属**：

  java.io.DataInputStream

  java.io.DataOutputStream

  <br>

  **标准输出流**：

  java.io.PrintWriter

  java.io.PrintStream

  <br>

  **对象专属流**：

  java.io.ObjectInputStream 反序列化

  java.io.ObjectOutputStream 序列化

 <br><br> <br><br>  

## 关于FileInputStream

- 读取文件内容

  ```java
  package SelfTest;
  
  
  import java.io.FileInputStream;
  import java.io.FileNotFoundException;
  import java.io.IOException;
  
  public class FileInputStreamTest {
      public static void main(String[] args) {
          FileInputStream fis = null;
          try{
              fis = new FileInputStream("F:\\idea\\IOstudy\\temp");
              //准备一个byte数组
              byte[] bytes = new byte[4];
             /* while( true){
                  int readCount = fis.read(bytes);
                  if(readCount==-1) break;
                  //把byte数组转换成字符串，读到多少个就转换成多少个
                  System.out.print(new String(bytes,0,readCount));
              }*/
              //对前一个方法的优化
              int readCount = 0;
              while( (readCount=fis.read(bytes))!=-1){
                  System.out.println(new String(bytes,0,readCount));
              }
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              if (fis!=null);{
                  try{
                      fis.close();
                  }catch (IOException e){
                      e.printStackTrace();
                  }
              }
          }
      }
  }
  ```

- IDEA的相对路径：相对路径一定是从当前的位置作为起点开始找！

  IDEA默认的当前路径是哪里？ 工程Project的根就是IDEA的默认当前路径

- FileInputStream类的其他常用方法：

  `int available()`  :返回流当中剩余的没有读到的字节数量

  `long skip(long n)` :跳过几个字节不读

 <br><br> <br><br>

## 关于FileOutputStream

```java
package SelfTest;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamTest {
    public static void main(String[] args) {
        FileOutputStream fos = null;
        try{
            //myfile文件不存在的时候会自动新建
            //这种方式谨慎使用，这个方式会将源文件清空，然后重新写入
            fos = new FileOutputStream("myfile");
            byte[] bytes={97,98,99,100};
            //将byte数组全部写出！
            fos.write(bytes);

            //将byte数组的一部分写出！
            fos.write(bytes,0,2);//再写出ab

            //以追加的方式在文件末尾写入。不会清空源文件内容
            fos = new FileOutputStream("myfile",true);
            fos.write(bytes,0,2);//再写出ab
            String str = "我是中国人！";
            byte [] bt = str.getBytes();//会将字符串转换成数组
            fos.write(bt);

            //写完之后，最后一定要刷新
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e){
            e.printStackTrace();
        } finally{
            if(fos !=null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

-  以追加的方式在文件末尾写入。不会清空源文件内容
              `fos = new FileOutputStream("myfile",true);`

- 想要换行的话：

  `out.write("\n");`

 <br><br> <br><br>

## 文件的复制

使用FileInputStream + FileOutputStream完成文件的拷贝。

拷贝的过程应该是一边读，一边写。

使用以上的字节流拷贝文件的时候，文件类型随意，万能的。什么样的文件都能拷贝。

```java
package SelfTest;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyTest {
    public static void main(String[] args) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try{
            fis = new FileInputStream("C:\\Users\\lcdzzz\\Videos\\Captures\\解决 java “错误：编码GBK 的不可映射字符” _ lcdzzz的博客 - Google Chrome 2021-01-21 15-50-05.mp4");//创建一个输入流对象
            fos = new FileOutputStream("E:\\解决 java “错误");//创建一个输出流对象

            //最核心的：一边读，一边写
            byte[] bytes = new byte[1024*1024];//1MB(一次最多拷贝1MB)
            int readCount=0;
            while((readCount= fis.read(bytes))!=-1){
                fos.write(bytes,0,readCount);
            }

            //刷新，输入流最后要刷新
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally{
            //分开try,不要一起try
            if(fos!=null);{
                try{
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fis!=null);{
                try{
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```







 <br><br> <br><br>

## java.io.File类

1. File类和四大家族没有关系，所以File类不能完成文件的读和写

2. File对象代表什么？

   文件和目录名的抽象表达形式。

   C:\Drivers

   C:\Drivers\Lan\Realtek\Readme.txt 也是File对象

3. 需要掌握File类的常用方法

```java
package SelfTest;

import java.io.File;

public class FileTest01 {
    public static void main(String[] args)throws Exception {
        //创建一个File对象
        File f1 = new File("D:\\zhoudianniubi.txt" );

        //判断是否出在！
        System.out.println(f1.exists());

        //如果文件不存在，则以文件的形式创建出来
        if (!f1.exists()){
            f1.createNewFile();
        }

        //如果文件不存在，则以目录的形式创建出来
        if(!f1.exists()){
            f1.mkdir();
        }
        //创建多层目录
        File f2 = new File("D:a/b/c/d");
        if (!f2.exists()){
            //以多重目录的形式新建、
            f2.mkdirs();
        }

    }
}
```



<br>

```java
package SelfTest;

import java.io.File;
import java.text.SimpleDateFormat;
import java.util.Date;

public class FileTest02 {
    public static void main(String[] args) {
        //获取文件、目录的父路径
        File f3 = new File("D:\\javase\\homework\\HotelMgtSystem.png");
        String parentpath = f3.getParent();
        System.out.println(parentpath);

        File parentFile = f3.getParentFile();
        System.out.println("获取绝对路径：" + parentFile.getAbsolutePath());

        //获取文件名
        System.out.println("文件名："+ f3.getName());

        //判断是否是一目录
        System.out.println(f3.isDirectory());//false

        //判断是否是一个文件
        System.out.println(f3.isFile());//true

        //获取文件最后一次修改时间
        long haoMiao = f3.lastModified();//从1970年到现在的总毫秒数
        //将总毫秒数转换成日期？？？
        Date time = new Date(haoMiao);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm;ss SSS");
        String strTime = sdf.format(time);
        System.out.println(strTime);

        //获取文件大小
        System.out.println(f3.length());//单位是字节
    }
}
```



<br>

```java
package SelfTest;

import java.io.File;

/*
File中的listFile方法
 */
public class FileTest03 {
    public static void main(String[] args) {
        //File [] listFiles()
        //获取当前目录下的所有子文件、子目录
        File f = new File("D:\\javase\\homework");
        File [] files = f.listFiles();

        //foreach
        for (File file:files
             ) {
            System.out.println(file.getAbsoluteFile());
        }
    }
}
```

<br><br><br><br>

### 关于序列化和反序列化

1. java.io.NotSerializablieException

   Student对象不支持序列化

2. 参与序列化和反序列化的对象，必须实现Serializable接口。

3. 注意：通过源代码发现，Serializable接口只是一个标志接口

   public interface Serializable{

   }

   这个接口当中什么代码都没有。

   那么它起到什么作用呢？

     起到标识的作用，java虚拟机看到这个类实现了这个接口，可能会对这个类进行特殊待遇。

   Serializable 这个标志接口是给java虚拟机参考的，java虚拟机看到这个接口后，会为该类自动生成一个序列化版本号。

   

   
   
   ```java
   /*
   序列化
   */
   
   package SelfTest;
   
   import java.io.FileOutputStream;
   import java.io.ObjectOutputStream;
   import java.io.Serializable;
   
   public class ObjectOutputStreamTest01 {
       public static void main(String[] args) throws Exception {
           //创建java对象
           Student s = new Student(1111,"zhangsan");
           //序列化
           ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"));
           //序列化对象
           oos.writeObject(s);
           //刷新
           oos.flush();
           //关闭
           oos.close();
       }
   }
   class Student implements Serializable {
       private int no;
       private String name;
       public Student() {
       }
   
       public Student(int no, String name) {
           this.no = no;
           this.name = name;
       }
   
       public int getNo() {
           return no;
       }
   
       public void setNo(int no) {
           this.no = no;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

<br>

```java
/*
反序列化
*/
package SelfTest;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ObjectInputStreamTest {
    public static void main(String[] args) {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("students"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        //开始反序列化，读
        Object obj = null;
        try {
            obj = ois.readObject();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //反序列化回来是一个学生对象，所以可以调用学生对象的toString方法
        System.out.println(obj);
        try {
            ois.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br><br>

### 序列化多个对象（序列化集合）并反序列化

注意：存多个对象必须用List集合，直接存的话会报错

```java
package SelfTest;

import exception.homework.UserService;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;

public class ObjectOutputStream02 {
    public static void main(String[] args)throws Exception {
        List<User> userList= new ArrayList<>();
        userList.add(new User(1,"zs"));
        userList.add(new User(2,"ls"));
        userList.add(new User(3,"ww"));
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("Users"));
        oos.writeObject(userList);
        oos.flush();
        oos.close();
    }
}
class User implements Serializable {
    private int no;
    private String name;

    public User(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public User() {
    }

    @Override
    public String toString() {
        return "user{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

<br>

```java
package SelfTest;

import java.io.FileInputStream;
import java.io.ObjectInputStream;

import java.util.List;

public class ObjectInputStream02 {
    public static void main(String[] args)throws Exception {
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("users"));
        List<User> userList =(List<User>)ois.readObject();
        for (Object user:userList
             ) {
            System.out.println(user);
        }
        ois.close();
    }
}
```

<br>
<br>

### transient关键字表示有力的，不参与序列化

`private transient String name`  //name不参与序列化操作

反序列化以后，name=null

<br>
<br>
<br>
<br>

## IO+Properties的联合应用

非常好的一个设计理念：

- 以后经常改变的数据，可以单独写到一个文件中，使用程序动态获取。

  将来只需要修改这个文件的内容，java代码不需要改动，不需要重新编译，服务器也不需要重启，就可以拿到动态的信息。

- 类似于以上机制的这种文件被称为配置文件。

  并且当配置文件的内容格式是：

  key1=value

  key2=value
  的时候，我们把这种配置文件叫做属性配置文件

- java规范中要求，属性配置文件建议以.properties结尾，但这不是必须的。

  这种以.properties结尾的文件在java中被称为：配置文件

  其中Properties是专门存放属性配置文件内容的一个类

```java
package SelfTest;

import java.io.FileReader;
import java.util.Properties;

public class IoPropertiesTest01 {
    public static void main(String[] args) throws Exception {
        /*
        properties是一个Map集合，key和value都是String类型
        香江userinfo文件中的数据加载到Propertie对象当中
         */
        //新建一个输入流对象
        FileReader reader= new FileReader("");
        //新建一个Map集合
        Properties pro = new Properties();
        //调用Properties对象的lode方法将文件中的数据加载到Mpa集合中
        pro.load(reader);
        //通过key来获取
        String username = pro.getProperty("");
        System.out.println(username);
    }
}
```

<br><br><br><br>

## 多线程

1. 进程是一个应用程序，软件

   线程是一个进程中的执行场景、执行单元

   一个进程可以启动多个线程

2. 进程A和进程B的内容独立不共享

3. 线程A和线程B，堆内存和方法区内存共享，但是栈内存独立，一个线程一个栈

4. java中有多线程机制，目的就是为了提高程序的处理效率

5. 使用多线程之后，main方法结束，程序有可能不会结束，main方法结束之时主线程结束了，主栈空了，其他的栈（线程）可能还在压栈弹栈。

6. 关于线程对象的声明周期：

   新建，就绪。运行，阻塞，死亡状态

### 实现线程的两种方式

- 第一种

  用类继承Thread

  ```java
  package SelfTest;
  
  public class ThreadTest02 {
      public static void main(String[] args) {
          //这里是main方法，这里的代码属于主线程，在主栈中运行
          //新建一个分支线程对象
          MyThread myThread = new MyThread();
          //启动线程
          myThread.start();//start()方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成后，瞬间结束了
          //接下来的代码还是运行在主线程中
          for (int i = 0; i <1000 ; i++) {
              System.out.println("主线程"+i);
          }
  
      }
  
  }
  
  class MyThread extends Thread{
      @Override
      public void run() {
          for (int i = 0; i <1000 ; i++) {
              System.out.println("支线程"+i);
          }
      }
  }
  ```

  

- 第二种

  ```java
  package SelfTest;
  
  public class ThreadTest03 {
      public static void main(String[] args) {
          /*
          创建一个可运行的对象
          MyRunnable r = new MyRunnable();
          将可运行的对象封装成一个线程对象
          Thread t = new Thread(r);
           */
          Thread t = new Thread(new MyRunnable());
          //启动线程
          t.start();
          for (int i = 0; i <1000 ; i++) {
              System.out.println("主线程"+i);
          }
      }
  }
  class MyRunnable implements Runnable{
  
      @Override
      public void run() {
          for (int i = 0; i <1000 ; i++) {
              System.out.println("支线程"+i);
          }
      }
  }
  ```

### 获取线程对象的名字

1. 怎么获取**当前线程**对象

   `Thread t = Thread.currentThread();`

2. 获取线程对象的名字

   `String name=线程对象.getName();` 

   主线程名字就叫main

3. 修改线程对象的名字

   线程对象.setName("线程名字")

4. 当前没有设置名字的话，默认Thread-0，Thread-1，Thread-2

<br><br><br><br>

## 关于线程的sleep方法：

ststic void sleep(long millis)

1. 静态方法：Thread.sllep(1000)

2. 参数是毫秒

3. 作用：让当前线程进入休眠，进入“阻塞状态”，放弃占有CPU时间片，让给其他线程使用（出现在哪，就让谁睡）。

   注意！**，sleep是静态方法，sleep方法，出现在哪个线程，就 休眠，和哪个线程对象调用它没有关系！**

4. 用法：

   ```java
   package SelfTest;
   
   public class ThreadTest06 {
       public static void main(String[] args) {
           //让当前线程进入休眠，睡眠5秒
           //当前线程是主线程！！！
           try {
               Thread.sleep(5*1000);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
   
           //5秒之后执行以下代码
           System.out.println("hello world!");
       }
   }
   ```

5. 间隔某段特定的时间去执行某段特定的代码，每隔多久执行一次

   ```java
   package SelfTest;
   
   public class ThreadTest06 {
       public static void main(String[] args) {
           for (int i = 0; i <10 ; i++) {
               System.out.println(Thread.currentThread().getName()+"-->"+i);
               try {
                   Thread.sleep(1000);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
       }
   }
   ```

### 终断睡眠

t.interrupt;//t是线程的名字

这种终端睡眠的方式依靠了java的异常的处理机制

```java
package SelfTest;

public class ThreadTest08 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable2());//将可运行对象封装成一个线程对象
        t.start();
        t.setName("t");
        try {
            Thread.currentThread().sleep(5*1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        /*
         这种终端睡眠的方式依靠了java的异常的处理机制
         */
        t.interrupt();//干扰，一盆冷水过去！给爷醒！
    }
}
class MyRunnable2 implements Runnable{
    public MyRunnable2() {
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"--->begin");
        try {
            Thread.sleep(1000*60*60*24*365);
        } catch (InterruptedException e) {
            e.printStackTrace();
            System.out.println("线程被唤醒！终止睡眠！");
        }
    }
}
```

### java中怎么合理终止一个线程的执行

```java
package SelfTest;

public class ThreadTest10 {
    public static void main(String[] args) {
        MyRunable4 r = new MyRunable4();
        Thread t = new Thread(r);
        t.setName("t");
        t.start();
        //模拟五秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //终止线程
        r.run=false;
    }
}
class MyRunable4 implements Runnable {
    //打一个布尔标记
    boolean run = true;

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            if (run) {
                System.out.println(Thread.currentThread().getName() + "--->" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }else{
                break;
            }
        }
    }
}
```

<br><br><br><br>

## 怎么解决线程的安全问题呢？

当多线程并发的环境下，有共享数据，并且这个数据还会被修改，此时就存在线程安全问题，怎么解决这个问题？

- 线程排队执行。（不能并发）

  用排队执行解决线程安全问题。

  这种机制被称为：线程同步机制

- 专业术语叫做：线程同步，实际上就是线程不能并发了，线程必须排队执行

- 怎么解决线程安全问题？

  使用“线程同步机制”

- 线程同步就是线程排队

说道线程同步，设计两个专业术语

- 异步编程模型：并发
- 同步编程模型：排队

<br><br>

### java中有三大变量

- 实例变量：在堆中

- 静态变量：在方法去中

- 局部变量：在栈中

- 以上三大变量中：局部变量永远都不会存在线程安全问题。因为局部变量不共享

- 实例变量在堆中，堆只有一个

  静态变量在方法区中，方法去只有一个。

  堆和方法区都是多线程共享的，所以可能存在线程安全问题。

<br><br>

### 在开发中怎么解决线程安全问题？

- 是上来就选择线程同步吗？

  不是，synchronized会让程序的执行效率降低

- 尽量使用局部变量代替“实例变量和静态变量”

- 如果必须是实例变量，那么可以考虑创建多个对象，这样实例变量的内存就不共享了。（一个线程对应一个对象）

- 如果不能使用局部变量，对象也不能创建多个，这个时候就之后能选择synchronized了。线程同步机制。

<br><br>

### synchronized有三种用法：

1. 同步代码块：灵活

   synchronized(线程共享对象){

   同步代码块

   }

2. 在实例方法上使用synchronized

   `public synchronized void withdraw(double money)`

   表示共享对象一定是this

3. 在静态方法上使用synchronized

   `class MyClass{`

   `public synchronized static void doSome(){`

   `}`

   `}`

   表示找类锁

   类锁只有一把

   就算创建了100个对象，那么类锁也只有一把

<br><br><br><br>

## 守护线程

1. java语言中线程分为两大类：

   一类是：用户线程

   一类是：守护线程（后台线程）

   其中具有代表性的就是：垃圾回收线程（守护线程）。

   - 守护线程的特点：

     一般守护线程就是一个死循环，所有的用户线程只要结束，守护线程自动结束。

     注意：主线程main就是一个用户线程

   - 守护线程用在什么地方呢？

     每天00：00的时候系统数据自动备份，这个需要使用定时器，并且我们可以将定时器设置为守护线程。一直在那里看着，没到00:00的时候就备份一次。所有的用户线程如果结束了，守护线程自动退出，没有必要进行数据备份了。

   - 用法：在启动线程之前，将线程设置为守护线程

     `t。setDaemon(true)`

     `t.start`

   <br><br><br><br>

   

   ## 定时器

   定时器的作用：

   间隔特定的时间，执行特定的程序。

   每周要进行银行的总账操作。

   每天要进行数据的备份操作。

   在实际的开发中，每隔多久执行一段特定的程序，这种需求很常见，那么在java中其实可以采用多种方式实现
   
- 可以使用sleep方法，睡眠设置睡眠时间，每到这个时间点醒来执行任务，这种方式是最原始的。定时器（比较low）
   - 在java的类库中已经写好了一个定时器：java.util.Timer,可以直接哪来用。
   
   ```java
   package SelfTest;
   
   import java.text.ParseException;
   import java.text.SimpleDateFormat;
   import java.util.Date;
   import java.util.Timer;
   import java.util.TimerTask;
   
   /*
   使用定时器定时任务
    */
   public class TimeTest {
   
   
       public static void main(String[] args) throws ParseException {
           //创建定时器对象
           Timer timer = new Timer();
           //Timer timer = new Timer(true); //守护线程的方法
   
           //指定定时任务
           //timeer.schedule(定时任务，第一次执行任务，间隔多久执行一次）
           SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
           Date firstTime = sdf.parse("2021-03-04 14:39:50");
           timer.schedule(new LogTimerTask(),firstTime,1000*10);
   
       }
   }
   //编写一个定时任务类
   //假设这是一个记录日志的定时任务
   class LogTimerTask extends TimerTask{
   
       @Override
       public void run() {
           //编写你需要执行的任务就行了
           SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
           String strTime = sdf.format(new Date());
           System.out.println(strTime+":成功完成了一次数据备份");
   
       }
   }
   ```
   
   

<br><br><br><br>

## 实现线程的第三种方法，实现Callable接口。（JDK8新特性。）

这种方式实现的线程可以获取线程的返回值。

之前讲解的那两种方式是无法获取线程返回值的，因为run方法返回void

思考：系统委派一个线程去执行一个任务，该线程执行完任务呢之后，可能会有一个执行结果，我们怎么能拿到这个执行结果呢？

​	使用第三种方法：实现Callable接口。

- 优点：可以获取线程的执行结果
- 缺点：效率比较低，在获取t线程的执行结果的时候，当前线程受阻塞，效率较低



```java
package SelfTest;

import jdk.swing.interop.SwingInterOpUtils;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/*
实现线程的第三种方式
实现Callable接口
 */
public class ThreadTest15 {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //第一步：创建一个“未来任务对象“对象。
        FutureTask task = new FutureTask(new Callable() {
            @Override
            public Object call() throws Exception {//call()方法就相当于run方法，只不过这个方法有返回值
                System.out.println("call method begin");
                Thread.sleep(1000*2);
                System.out.println("call method end!");
                int a= 100;
                int b= 200;
                return  a+b;

            }
        });
        Thread t = new Thread(task);
        t.start();
        
        //这里是main方法，这里在主线当众。
        //在主线程中，怎么获取t线程的返回结果？
//        Object obj = new Object();
         Object obj = task.get();
        System.out.println("线程执行结果："+obj);
        System.out.println("hello world");


    }
}
```

<br><br><br><br>

## 关于Object类中的wait和notify方法（生产者和消费者模式！）

1. wait和notifu方法不是线程对象的方法，是java中任何一个对象都有的方法，因为这两个方式是Object类中自带的

2. wait()方法作用？

   `Object  o = new Object();`

   `o.wait();`

   表示：让正在o对象上活动的线程进入等待状态，无期限等待，

   直到被唤醒位置。

   o.wait()方法的调用，会让“当前线程（正在o对象上活动的线程）”进入等待状态

3. notify()方法作用？

   `Object 0 = new Object();`

   `o.notify();`

   表示：欢迎正在o对象上等待的线程。

   还有一个notifyAll()方法：这个方法是唤醒o对象上处于等待的所有线程。
   
4. 生产者和消费者程序实现！

   ```java
   package SelfTest;
   
   import java.util.ArrayList;
   import java.util.List;
   
   public class ThreadTest16 {
       public static void main(String[] args) {
           //创建一个仓库对象，共享的
           List list = new ArrayList();
           //创建两个线程对象
           //生产者线程
           Thread t1= new Thread(new Producer(list));
           //消费者线程
           Thread t2= new Thread(new Consumer(list));
           t1.setName("生产者线程");
           t2.setName("消费者线程");
           t1.start();
           t2.start();
   
       }
   }
   //生产线程
   class Producer implements Runnable{
       private List list;
   
       public Producer(List list) {
           this.list = list;
       }
   
       @Override
       public void run() {
   //一直生产(用死循环模拟生产）
           while(true){
               //给仓库对象list枷锁
               synchronized (list){
                   if(list.size()>0){
                       try {
                           //当前线程进入等待状态，并且释放Producer先前占有list集合的锁
                           list.wait();
                       } catch (InterruptedException e) {
                           e.printStackTrace();
                       }
                   }
                   //程序能够执行到这里说明仓库是空的，可以生产
                   Object obj = new Object();
                   list.add(obj);
                   System.out.println(Thread.currentThread().getName()+"--->"+obj);
                   //唤醒消费者进行消费
                   try {
                       Thread.sleep(1000*2);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   list.notify();
               }
           }
       }
   }
   //消费线程
   class Consumer implements Runnable{
       private List list;
   
       public Consumer(List list) {
           this.list = list;
       }
   
       @Override
       public void run() {
   //一直消费
           while(true){
               //给仓库对象list枷锁
               synchronized (list){
                   if(list.size()==0){
                       try {
                           //当前线程进入等待状态，并且释放list集合的锁
                           list.wait();
                       } catch (InterruptedException e) {
                           e.printStackTrace();
                       }
                   }
                   //程序能够执行到次数说明仓库中有数据，进行消费
                   Object obj = list.remove(0);
                   System.out.println(Thread.currentThread().getName()+"--->"+obj);
                   //唤醒生产者生产
                   try {
                       Thread.sleep(1000*2);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   list.notify();
               }
           }
       }
   }
   ```

<br><br><br><br>

## 反射机制

1. 反射机制有什么用？

   通过java语言中的反射机制可以操作字节码文件。

   有点类似于黑客。（可以读和修改字节码文件。）

   通过反射机制可以操作代码片段。（class文件。）

2. 反射机制的相关类在哪个包下？

   `java.lang.reflect.*;`

3. 反射机制相关的重要的类有哪些？

   java.lang.class代表整个字节码，代表一个类型，代表整个类

   java.lang.reflect.Method代表字节码中的方法字节码，代表类中的方法

   java.lang.class.Constructor代表字节码中的构造方法字节码，代表类中的构造方法

   java.lang.class.Field代表字节码中的属性字节码， 代表类中的属性

### 要操作一个类的字节码，需要首先获取到这个类的字节码，怎么获取java.lang.Class实例？

1. Class c = Class.forName("完整类名带包名")；

   Class.forName()

   1. 静态方法
   2. 方法的参数是一个字符串
   3. 字符串需要的是一个完整的类名
   4. 完整类名必须带有包名。java.lang包也不能省略

2. Class c = 对象.getClass();

   java中任何一个对象都有一个方法：getClass()

3. Class c = 任何类型.class

   java语言中任何一种类型，包括基本数据类型，它都有.class属性

```java
package ReflectTest;

import java.util.Date;

public class ReflectTest01 {
    public static void main(String[] args) {
        Class c1 = null;
        Class c2 = null;
        try {
            c1 =Class.forName("java.lang.String");
            c2 = Class.forName("java.util.Date");
            System.out.println(c1);//class java.lang.String
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //java中任何一个类对象都有一个方法：getClass()
        String s = "abc";
        Class x = s.getClass();
        System.out.println(x);//class java.lang.String
        System.out.println(c1==x);//true

        Date time = new Date();
        Class y = time.getClass();
        System.out.println(c2==y);//true

        Class c = String.class;
        System.out.println(c);//class java.lang.String
    }
}
```

<br><br>

### 获取到Class，能干什么？

**获取了Class之后，可以调用无参数构造方法来实例化对象**

一定要注意：

newInstance()底层调用的是该类型的无参数构造方法。

如果没有这个无参数构造方法会出现异常

```java
package ReflectTest;

/*
获取到Class。能干什么？
    通过Class的newInstacne()方法来实例化对象。
    注意：newInstance()方法内部实际上调用了无参构造方法，必须保证无参构造方法存在才可以
 */
public class ReflectTest02 {
    public static void main(String[] args) {
        try {
            //c代表的就是日期Date类型
            Class c =Class.forName("ReflectTest.User");

            //newInstance() 这个方法会调用User这个类的午餐构造方法，完成对象的创建
			//实例化一个Date日期类型的对象
            Object obj = c.newInstance();
            
            System.out.println(obj);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        }
    }
}
```

<br><br>

### 验证反射机制的灵活性

java代码写一遍，在不该表java源代码的基础上，可以做到不同对象的实例化。非常灵活（符合OCP开发原则，对扩展开放，对修改关闭）

```java
package ReflectTest;

import java.io.FileReader;
import java.util.Properties;

public class ReflectTest03 {
    public static void main(String[] args) throws Exception {
        //通过IO流读取classinfo.properties文件
        FileReader reader = new FileReader("selfstudy\\src\\classinfo.proerties");
        //创建属性类对象Map
        Properties pro = new Properties();//key 和value都是String
        //加载
        pro.load(reader);
        //关闭流
        reader.close();

        //通过key获取value
        String classsName = pro.getProperty("className");
        System.out.println(classsName);
        //通过反射机制实例化
        Class c =Class.forName(classsName);
        Object obj = c.newInstance();
        System.out.println(obj);

    }
}
```

以下文件是classinfo.properties

```java
className=java.util.Date
className1= ReflectTest.User
```

<br><br>

### 研究一下：Class.foeName()发生了什么？

记住，重点：**如果你只是希望一个类的静态代码块执行，其他代码一律不执行。**

你可以使用Class.forName("完整类名");

这个方法的执行会导致类加载，类加载时，静态代码块执行。



```java
package ReflectTest;

public class ReflectTest04 {
    public static void main(String[] args) {
        try {
            Class.forName("ReflectTest.MyClass");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

```java
package ReflectTest;

public class MyClass {
    static {
        System.out.println("MyClass类的静态代码执行了！");
    }

}
```

<br><br><br><br>

## 研究一下文件路径的问题

`FileReader reader = new FileReader("xxx/xxx")`

- 这个方式的路径缺点是：移植性差，在IDEA中默认的当前路径是project的根
- 这个代码假设离开了IDEA，换到了其他位置，可能当前路径就不是project的根了，这是这个路径就无效了

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

- 接下来说一种比较通用的路径

- 注意：使用以下通用方式的前提是：这个文件必须在类路径下，换句话说就是放在src下面

- 什么是类路径下？就是在src下的都是类路径下。

- src是类的根路径

  `Thread.currentThread().getContextClassLoader().getResource("    ").getPath();`

- 解释：

  Threader.currentThread() 当前线程对象

  getContextClassLoader() 是线程对象的方法，可以获取当前线程的类加载器对象。

  getResource() 【获取资源】这是类加载器对象的方法，当前线程的类加载器默认从类的根路径下加载资源

  `String path =Thread.currentThread().getContextClassLoader().getResource("classinfo.properties").getPath(); `

  `System.out.println(path);`

- 以下实操、

  ```java
  package SelfTest;
  
  
  import java.io.FileReader;
  import java.util.Properties;
  
  public class IoPropertiesTest {
      public static void main(String[] args) throws Exception {
          String path = Thread.currentThread().getContextClassLoader().getResource("classinfo.properties").getPath();
          FileReader reader = new FileReader(path);
          Properties pro = new Properties();
          pro.load(reader);
          reader.close();
          //通过key获取value
          String className = pro.getProperty("className");
          System.out.println(className);
      }
  }
  ```

- 使用流的方式直接返回

  ```java
  package SelfTest;
  
  
  
  import java.io.InputStream;
  import java.util.Properties;
  
  public class IoPropertiesTest {
      public static void main(String[] args) throws Exception {
         /* String path = Thread.currentThread().getContextClassLoader().getResource("classinfo.properties").getPath();
          FileReader reader = new FileReader(path);*/
          InputStream reader = Thread.currentThread().getContextClassLoader()
                  .getResourceAsStream("classinfo.properties");
  
          Properties pro = new Properties();
          pro.load(reader);
          reader.close();
          //通过key获取value
          String className = pro.getProperty("className");
          System.out.println(className);
      }
  }
  ```

- 使用资源绑定器（最终！！！！）

  java.util包下提供了一个资源绑定器，便于获取属性配置文件中的内容

  使用以下这种方式的时候，属性配置文件xxx.properties必须放在类路径下

  ```java
  package SelfTest;
  
  import java.util.ResourceBundle;
  
  public class ResourceBundleTest {
      public static void main(String[] args) {
          //资源绑定器，只能绑定xxx.properties文件。并且这个文件必须在类路径下。文件的扩展名必须是properties
          //并且写路径的时候，路径后面扩展名不能写
          ResourceBundle bundle = ResourceBundle.getBundle("classinfo");
          String className = bundle.getString("className");
          System.out.println(className);
      }
  }
  ```

  

<br><br><br><br>

## 获取field（了解一下）

```java
package ReflectTest;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class ReflectTest05 {
    public static void main(String[] args) throws Exception {
        //获取整个类
        Class studentClass = Class.forName("ReflectTest.Student");
        String className = studentClass.getName();
        System.out.println("完整类名 "+className);
        String simpleName = studentClass.getSimpleName();
        System.out.println("简类名 "+simpleName);

        //获取类中所有的Field
        Field[] fields=studentClass.getFields();
        System.out.println(fields.length);//1
        //取出这个Field
        Field f = fields[0];
        System.out.println(f);//public int ReflectTest.Student.no
        String fieldName = f.getName();
        System.out.println(fieldName);//no
        System.out.println("---------------------------------------");

        //获取所有的Field
        Field[] fs = studentClass.getDeclaredFields();
        System.out.println(fs.length);
        for (int i = 0; i < fs.length ; i++) {
            System.out.println(fs[i].getName()+"的类型是："+fs[i].getType());
        }
        System.out.println("以下第二种遍历，使用SimpleName和getType获取类型");
        for (Field field:fs
             ) {
            int i = field.getModifiers();//返回修饰符的代号
            String modifierString = Modifier.toString(i);//将代号转换成字符串
            Class fieldType = field.getType();//获取完整类型
            String simpleType = fieldType.getSimpleName();//获取类型的简化
            String fName = field.getName();//字节名
            System.out.println(modifierString+" "+simpleType+"--->"+fName);
        }
    }
}
/*
另一种拿到修饰符字符串的方法：Modifier.toString(studentClass.getModifiers())
*/
```

运行后会返回

```运行结果
完整类名 ReflectTest.Student
简类名 Student
1
public int ReflectTest.Student.no
no
---------------------------------------
4
no的类型是：int
name的类型是：class java.lang.String
age的类型是：int
sex的类型是：boolean
以下第二种遍历，使用SimpleName和getType获取类型
public int--->no
private String--->name
protected int--->age
 boolean--->sex
```

<br><br><br><br>

## 反编译Field（了解一下）

给我一个class文件，能够拿到java源码

```java
package ReflectTest;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class ReflectTest06 {
    public static void main(String[] args) throws ClassNotFoundException {
        //创建这个是为了拼接字符串
        StringBuilder s = new StringBuilder();
        Class studentClass = Class.forName("ReflectTest.Student");
        s.append(Modifier.toString(studentClass.getModifiers())+"class"+studentClass.getSimpleName()+"{\n");
        Field[] fields = studentClass.getDeclaredFields();
        for (Field field:fields
             ) {
            s.append("\t");//转义符：缩进
            s.append(Modifier.toString(field.getModifiers()));
            s.append(" ");
            s.append(field.getType().getSimpleName());
            s.append(" ");
            s.append(field.getName());
            s.append(";\n");
        }

        s.append("}");
        System.out.println(s);

    }
}
```

<br><br><br><br>

## 通过反射机制访问对象属性

```java
package ReflectTest;


import java.lang.reflect.Field;

public class ReflectTest07 {
    public static void main(String[] args) throws Exception {
        //不使用反射机制，怎么去访问一个对象的属性呢？
        Student s = new Student();
        s.no=1111;//给属性赋值
        System.out.println(s.no);//读属性值

        //使用反射机制，怎么去访问一个对象的属性
        Class studentClass = Class.forName("ReflectTest.Student");
        Object obj = studentClass.newInstance();

        //获取no属性（根据属性的名称来获取Field
        Field noField = studentClass.getDeclaredField("no");
        //给obj对象（Student对象）的no属性赋值
        /*
        虽然使用了反射机制，但是三要素还是缺一不可：
            要素1.obj对象
            要素2：no属性
            要素3:2222值
        注意：反射机制虽然让复杂了，但是为了“灵活，是值得的。
         */

        noField.set(obj,22222);//给obj对象的no属性赋值22222

        //读取属性的值
        //两个要素：获取obj对象no属性的值
        System.out.println(noField.get(obj));


        //可以反问私有属性吗？
        Field nameField = studentClass.getDeclaredField("name");

        //打破封装（反射机制的特点：打破封装（反射机制的特点）
        //这样设置完后，在外部也可以反问private的
        nameField.setAccessible(true);

        //给name属性配置
        nameField.set(obj,"jackson");
        //获取name属性的值
        System.out.println(nameField.get(obj));

    }
}
```

<br><br><br><br>

## 可变长度参数（掌握)

- int... args这就是可变长度参数
- 语法是：`类型... ` 注意：一定是3个点

1. 可变长度参数要求的参数个数是0~N
2. 可变长度参数在参数列表中必须在最后一个位置上，而且可变长度参数只能有一个

```java
package ReflectTest;

public class ArgsTest {
    public static void main(String[] args) {
        m();
        m(10);
        m(10,20);
        m2(100);
        m2(200,"abd","xyz");
        m3("abd","def","wqeq");

        String[] strs={"a","b","c"};
        //也可以传一个数组进去
        m3(strs);

        m3(new String[]{"你","很","牛","蛙"});//没必要
        m3("你","很","牛","蛙");
    }
    public static void m(int... args){
        System.out.println("m方法执行了！" );
    }
    public static void m2(int a,String ... args){

    }
    public static void m3(String...args){
        //args有length属性，说明args是一个数组！
        for (int i = 0; i <args.length ; i++) {
            System.out.println(args[i]);
        }
    }
}
```

<br><br><br><br>

## 反射Method（了解内容）

```java
package ReflectTest;

import java.lang.reflect.Method;
import java.lang.reflect.Modifier;

public class ReflectTest08 {
    public static void main(String[] args) throws ClassNotFoundException {
        //获取类
        Class userServiesClass = Class.forName("ReflectTest.UserService");
        //获取所有的Method
        Method[] methods= userServiesClass.getDeclaredMethods();
//        System.out.println(methods.length);//2
        for(Method method:methods){
            //获取修饰符列表
            System.out.println(Modifier.toString(method.getModifiers()));
            //获取方法的返回值类型
            System.out.println(method.getReturnType().getSimpleName());
            //获取方法名
            System.out.println(method.getName());
            //方法的参数列表（一个方法的参数可能会有多个
            Class[] parameterTypes=method.getParameterTypes();
            for (Class cla:parameterTypes
                 ) {
                System.out.println(cla.getSimpleName());
            }
        }
    }
}
```

## 反编译Method一个类的方法	

```java
package ReflectTest;

import java.lang.reflect.Method;
import java.lang.reflect.Modifier;

public class ReflectTest09 {
    public static void main(String[] args)throws Exception {
        StringBuilder s = new StringBuilder();
        Class userServiceClass = Class.forName("ReflectTest.UserService");
        s.append(Modifier.toString(userServiceClass.getModifiers())+" class "+userServiceClass.getSimpleName()+"{\n");
        Method[] methods=userServiceClass.getDeclaredMethods();
        for (Method method:methods
             ) {
            System.out.println("\t");
            s.append(Modifier.toString(method.getModifiers()));
            s.append(" ");
            s.append(method.getReturnType().getSimpleName());
            s.append(" ");
            s.append(method.getName());
            s.append("(");
            Class[] parameterTypes = method.getParameterTypes();
            for (Class parameterType:parameterTypes
                 ) {
                s.append(parameterType.getSimpleName());
                s.append(",");
            }
            s.deleteCharAt(s.length()-1);
            s.append("){}\n");
        }
        s.append("}");
        System.out.println(s);
    }
}
```

<br><br><br><br>

## 重点：通过反射机制怎么调用一个对象的方法* * * * * 

反射机制，让代码很具有通用性，可变化的内容都是写到配置文件当中，将来修改配置文件之后，创建的对象不一样了，调用的方法不同了，但是java代码必须要做任何改动，这就是反射机制。

```java
package ReflectTest;

import java.lang.reflect.Method;

public class ReflectTest10 {
    //使用反射机制来调用一个对象的方法怎么做？
    public static void main(String[] args) throws Exception {
    Class userServiceClass = Class.forName("ReflectTest.UserService");
    //创建对象
    Object obj = userServiceClass.newInstance();
    //获取Method
    Method loginMethod = userServiceClass.getDeclaredMethod("login",String.class,String.class);
        System.out.println(loginMethod);

    //调用方法
    /*
    要素分析：
    对象userService
    login方法名
    实参列表
    返回值
     */

    //调用方法有几个要素，也需要4要素
    Object retvalue = loginMethod.invoke(obj,"admin","123");
        System.out.println(retvalue);
    }

}
```



*注：重点是ReflectTest07和10*

<br><br><br><br>

## 反编译一个类的Constructor构造方法

```java
package ReflectTest;


import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;

public class ReflectTest11 {
    public static void main(String[] args) throws Exception {
        StringBuilder s = new StringBuilder();
        Class vipClass = Class.forName("ReflectTest.Vip");

        s.append(Modifier.toString(vipClass.getModifiers()));//public
        s.append(" class ");
        s.append(vipClass.getSimpleName());//vip
        s.append("{\n");

        //拼接构造方法
        Constructor[] constructors = vipClass.getDeclaredConstructors();//获取所有构造方法
        
        for(Constructor constructor:constructors){
            s.append("\t");
            s.append(Modifier.toString(constructor.getModifiers()));
            s.append(" ");
            s.append(vipClass.getSimpleName());
            s.append("(");
            //拼接参数
            Class[] parameterTypes = constructor.getParameterTypes();//获取所有参数类型
            for(Class parameterType:parameterTypes){
                s.append(parameterType.getSimpleName());
                s.append(",");
            }
            //删除最后下标位置上的字符
            if(parameterTypes.length>0){
                s.deleteCharAt(s.length()-1);
            }

            s.append("){}\n");
        }
        s.append("}");
        System.out.println(s);

    }
}
```

运行结果

```运行结果
public ReflectTest.Vip(int)
public ReflectTest.Vip(int,java.lang.String)
public ReflectTest.Vip(int,java.lang.String,java.lang.String)
public ReflectTest.Vip(int,java.lang.String,java.lang.String,boolean)
public ReflectTest.Vip()
public class Vip{
	public Vip(int){}
	public Vip(int,String){}
	public Vip(int,String,String){}
	public Vip(int,String,String,boolean){}
	public Vip(){}
}

```

<br><br><br><br>

## 通过反射机制new对象（比上一个例子重要一些）（但也不是重点）

```java\
package ReflectTest;


import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;

public class ReflectTest11 {
    public static void main(String[] args) throws Exception {
        StringBuilder s = new StringBuilder();
        Class vipClass = Class.forName("ReflectTest.Vip");

        s.append(Modifier.toString(vipClass.getModifiers()));//public
        s.append(" class ");
        s.append(vipClass.getSimpleName());//vip
        s.append("{\n");

        //拼接构造方法
        Constructor[] constructors = vipClass.getDeclaredConstructors();//获取所有构造方法

        for(Constructor constructor:constructors){
            s.append("\t");
            s.append(Modifier.toString(constructor.getModifiers()));
            s.append(" ");
            s.append(vipClass.getSimpleName());
            s.append("(");
            //拼接参数
            Class[] parameterTypes = constructor.getParameterTypes();//获取所有参数类型
            for(Class parameterType:parameterTypes){
                s.append(parameterType.getSimpleName());
                s.append(",");
            }
            //删除最后下标位置上的字符
            if(parameterTypes.length>0){
                s.deleteCharAt(s.length()-1);
            }

            s.append("){}\n");
        }
        s.append("}");
        System.out.println(s);

    }
}
```



<br><br><br><br>

## 获取父类和父接口

```java
package ReflectTest;

public class ReflectTest13 {
    public static void main(String[] args)throws Exception {

        //String 举例
        Class stringClass = Class.forName("java.lang.String");

        //获取String的父类
        Class superClass= stringClass.getSuperclass();
        System.out.println(superClass.getName());

        //获取String类实现的所有接口（一个类可以实现多个接口）
        Class[] interfaces = stringClass.getInterfaces();
        for (Class in: interfaces
             ) {
            System.out.println(in.getName());
        }

    }
}
```

<br><br><br><br>

## 注解

- 又叫注释，英文单词是Annotation

- 注解Annotation是一种引用数据类型。编译之后也是生成xxx.class文件。

- 怎么自定义注解呢？语法格式：

  [修饰符列表] @ interface 注解类型名{

  }

- 注解怎么使用，用在什么地方？

  1. 注解使用时的语法格式是：

     @注解类型名

  2. 注解可以出现在类上，属性上，方法上，变量上....注解还可以出现在注解类型上。

- JDK中内置了有哪些注解呢？

  java.lang包下的注释类型：

  - Deprecated用@Deprecated 注释的程序元素，不鼓励程序员使用这样的元素，通常是因为它很危险或存在更好的选择。

    <u>说明该类/方法以过时，为了把这个信息传达出去，比如~~doOther~~</u>（掌握）

  - Override 表示一个方法声明打算重写超类中的另一个方法声明。（掌握）

  - SuppressWarnings 只是应该在注释元素（以及包含在该注释元素中的所有程序元素）中取消显示只是的编译器警告。（不用掌握）

  <br><br>

  

  

  ### 元注解

  - 什么是元注解？

    用来“标注”注解类型的“注解”,称为元注解

  - 常见的元注解有哪些？

    Target

    Retention

  - 关于Target注解：

    这个一个元注解，用来标注“注解类型”的“注解”

    这个Target注解用来标注“被标注的注解”可以出现在哪些位置上。

    <br>

    `@Target(ElementType.METHOD)`:表示“被标注的注解只能出现在方法上

  - 关于Retention注解

    这个一个元注解，用来标注“注解类型”的“注解”

    这个Retention注解用来标注“被标注的注解”最终保存在哪里。

    <br>

    `@RetentionA(RetentionPolicy.SOURCE)`:表示该注解只能被保留在java源文件中。

    `@RetentionA(RetentionPolicy.CLASS)`:表示该注解被保存在class文件中。

    `@RetentionA(RetentionPolicy.RUNTIME)`:表示该注解被保存在class文件中，并且可以被反射机制所读取。

  <br><br><br><br>

  

  ### 反射注解，获取注解里的属性值
  
  - 有三个类
  
  - MyAnnotation.java【注解】
  
    ```java
    package ReflectAnnotationTest;
    
    
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;
    
    //只允许注解可以标注类和方法
    @Target({ElementType.TYPE,ElementType.METHOD})
    //希望这个注解可以被反射
    @Retention(RetentionPolicy.RUNTIME)
    public @interface MyAnnotation {
        String value() default "宁波江东区";
    
        /*
        username属性
       */
        String username();
  
        String password();
    }
    ```
  
  - MyAnnotationTest.java【里面包含注解的java类】
  
    ```java
  package ReflectAnnotationTest;
    @MyAnnotation(username = "admin",password = "123")
  public class MyAnnotationTest {
        
    }
    ```
  
  - ReflectAnnotation.java【用于测试】【所有需要了解的代码都在这里】
  
    ```java
    package ReflectAnnotationTest;
    
    public class ReflectAnnotation {
        public static void main(String[] args) throws Exception {
            Class c = Class.forName("ReflectAnnotationTest.MyAnnotationTest");
    
            //判断类上面是否有@MyAnnotation
    //        System.out.println(c.isAnnotationPresent(MyAnnotation.class));
            if (c.isAnnotationPresent(MyAnnotation.class)) {
                //获取该注解对象
                MyAnnotation myAnnotation = (MyAnnotation) c.getAnnotation(MyAnnotation.class);
                System.out.println("类上面的注解对象" + myAnnotation);
                System.out.println(myAnnotation.username());
                System.out.println(myAnnotation.password());
    
                //获取注解对象的属性怎么办？和调接口没区别。
                String value = myAnnotation.value();
                System.out.println(value);
          }
    
    
        }
    }
    ```

  

  <br><br><br><br>
  
### 注解在开发中有什么用呢？

需求：

- 假设有这丫昂的一个注解，叫做：@ID
  - 这个注解只能出现在类上面，当这个类上有这个注解的时候要求这个类中必须有一个int类型的id属性，如果没有这个属性就报异常，如果有这个属性则正常执行！
  
  答案
  
  - ID.java【注解】
  
    ```java
    package AnnotationHomework;
    
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;
    //表示这个注解只能出现在类上面
    @Target(ElementType.TYPE)
    //该注解可以被反射机制读取到
  @Retention(RetentionPolicy.RUNTIME)
    public @interface ID {
  
    }
    //这个注解@ID用来标注类，被标注的类中必须又一个int类型的id属性，没有的话就报异常
    ```
  
  - User.java【定义类，要求有int类型的id属性】
  
    ```java
    package AnnotationHomework;
    @ID
    public class User {
      int id;
        String name;
      String password;
    
    }
    ```
  
  - Test.java【测试类，使用了反射注解】
  
    ```java
    package AnnotationHomework;
    
    import java.lang.reflect.Field;
    
    public class Test {
        public static void main(String[] args) throws Exception {
    
            //获取类
            Class userClass = Class.forName("AnnotationHomework.User");
            //判断类上是否存在ID注解
            boolean isOk = false;
            if(userClass.isAnnotationPresent(ID.class)){
                //当一个类上面有@ID注解的时候吗，要求类中必须存在int类型的id属性
                //如果没有int类型的id属性则报异常
                //获取类的属性
                Field[] fields = userClass.getDeclaredFields();
    
                for (Field field:fields){
                    if ("id".equals(field.getName())&&"int".equals(field.getType().getSimpleName())){
                        //表示这个类是合法的类。有@ID注解，则这个类必须有int类型的id
                        isOk = true;
                        break;
                    }
                }
                if (!isOk){
                  throw new HasnotIDPropertyException("被@ID注解标注的类必须要有一个int类型的id属性！");
                }
          }
        }
  }
    ```
  
    
  
  - AnnotationHomework.java【自定义异常类】
  
    ```java
    package AnnotationHomework;
    /*
    自定义异常
     */
    public class HasnotIDPropertyException extends RuntimeException{
        public HasnotIDPropertyException(){
    
      }
        public HasnotIDPropertyException(String s){
            super(s);
        }
    }
    ```
