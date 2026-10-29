---
title: java语法随记
date: 2021-07-04 22:31:11
tags: 
    - 大学
cover: https://images.wallpaperscraft.com/image/single/pyramids_starry_sky_night_161840_300x168.jpg
---

## java.math.BigDecimal的用法                                                                                                                                                                     

1. BigDecimal类型用来存储金额等有小数，需要精度的数据

2. BigDecimal不能直接使用int类型赋值！

3. 两种使BigDecimal初始化为0的方法:  
   BigDecimal a=BigDecimal
   BigDecimal b=new BigDecimal(0);
   BigDecimal a = in.nextBigDecimal();

   * 除法运算：

   ​      `System.out.println(new BigDecimal(153.5).divide(new BigDecimal(100), 2, BigDecimal.ROUND_HALF_UP));`

   　  //new BigDecimal(100)除数，2精确的位数，BigDecimal.ROUND_HALF_UP：舍入模式

    * 加法运算：

      `System.out.println(new BigDecimal("2.005").add(new BigDecimal("0.03")));`

    * 减法运算：

      `System.out.println(new BigDecimal("2.00").subtract(new BigDecimal("1.10")));`

    * 乘法运算：

      `System.out.println(new BigDecimal("2.05").multiply(new BigDecimal("10")));`

<br><br><br><br>

## char [] SS = s.toCharArray();

* `char [] SS = s.toCharArray();`

  //把字符串中的每个字符放进ss的字符数组里
  
  <br><br><br><br>



##  s.contains("4")||s.contains("62")

*  `(s.contains("4")||s.contains("62")`

  //判断s字符串里有没有包含"4"或"62"



<br><br><br><br>

## java自定义函数调用

### 一：主类中的自定义函数

在主类中，如果想要在main函数中调用自定义的其他方法，则这个函数需要使用<u>static关键字</u>定义，否则会报错Cannot make a static reference to the non-static method xx from the type xx，调用的时候<u>直接用函数名</u>就可以了，如下：

```java
        public class create_function {   
        public static void main(String []args){　　
       int s = jia(5,4);
                 System.out.println(s);
             }
             static int jia(int a, int b){
                 System.out.println("我是自定义相加函数，我被调用了！！");
                 return a+b;
             }             
         }     
```

下面使用static关键字定义了一个“+”运算的函数，在main中调用时，直接使用函数名，括号中加参数就可以了。输出结果如下：

​        我是自定义相加函数，我被调用了！！
​        9

<br><br>

### 二：自定义类中函数的调用

自定义类中函数的调用有两种情况，静态函数和非静态函数，<u>非静态函数的调用需要先声明一个类实例，通过实例调用</u>。静态函数的调用可以通过实例，也可以直接使用类名调用(建议使用这种，要不会造成内存空间的浪费。)，如下：

```java
        public class create_function {    
        public static void main(String []args){
                   Y y =new Y();                

                int s2 = y.cheng(3,5);//声明了y这个实例
                System.out.println(s2);//直接用Y这个类调用
                
                int s3 = Y.chu(8, 2);
                System.out.println(s3);
            } 
        }

        class Y{
            int cheng(int a,int b){
                System.out.println("我是自定义类中的乘方法，我被调用了！");
                return a*b;
            }
            static int chu(int c,int d){
               System.out.println("我是自定义类中的除方法，我被调用了！");
                return c/d;
            }
        }
```



在下面，自定义了一个Y类，cheng方法是非静态方法，必须通过实例y调用，chu方法是静态方法，使用类名直接调用。输出结果如下：

​		我是自定义类中的乘方法，我被调用了！
​		15
​		我是自定义类中的除方法，我被调用了！
​		4

<br><br><br><br>



## String.valueOf()方法的使用

### 一：由基本数据形态转换成String

String 类别中已经提供了将基本数据型态转换成 String 的 static 方法 ，也就是 **String.valueOf()** 这个参数多载的方法 ：

（1）**String.valueOf(boolean b)** : 将 boolean 变量 b 转换成字符串 
（2）**String.valueOf(char c)** : 将 char 变量 c 转换成字符串 
（3）**String.valueOf(char[] data)** : 将 char 数组 data 转换成字符串 
（4）**String.valueOf(char[] data, int offset, int count)** : 将 char 数组 data 中 由 data[offset] 开始取 count 个元素 转换成字符串 

（5）**String.valueOf(double d)** : 将 double 变量 d 转换成字符串 
（6）**String.valueOf(float f)** : 将 float 变量 f 转换成字符串 
（7）**String.valueOf(int i)** : 将 int 变量 i 转换成字符串 
（8）**String.valueOf(long l)** : 将 long 变量 l 转换成字符串 
（9）**String.valueOf(Object obj)** : 将 obj 对象转换成 字符串, 等于 obj.toString() 

　　**用法如下:** 
　　`int i = 10;` 
　　`String str = String.valueOf(i);` 
　　这时候 str 就会是 "10" 

<br><br>

### **二、 由 String 转换成 数字的基本数据型态** 

要将 String 转换成基本数据型 ，大多需要使用基本数据型态的包装类别 

（1） **Byte.parseByte(String s)** : 将 s 转换成 byte 

（2）**Byte.parseByte(String s, int radix)** : 以 radix 为基底（进制基数） 将 s 转换为 byte ，比如说 Byte.parseByte("11", 16) 会得到 17。

（3） **Double.parseDouble(String s)** : 将 s 转换成 double 

（4） **Double.parseFloat(String s)** : 将 s 转换成 float 

（5） **Integer.parseInt(String s)** : 将 s 转换成 int 

（6） **Long.parseLong(String s)** : 将 s 转换成 long







## 终止程序的执行，退出JVM

> system.exit(0);



<br><br><br><br>





## 强行转换类型（强制类型转换/向下转型）

调用子类中特有的方法是，需要向下转型

```java
if(c instanceof D){//c指向的对象如果确实是一个D类型的
     D d1=(D)c;
    d1.doOther();//doOther是D类特有的方法
   
}
```

<br><br><br><br>

## 局部变量和成员变量

局部变量是没有初始值的

成员变量有初始值（0  0.0  null）

<br><br><br><br>

## next部分相关用法

1. nextInt()它只读取int值，nextInt()在读取输入后将光标放在同一行。

2. next()只读取输入直到空格。它无法阅读两个被空格隔开的单词。

   同样，next()在读取输入后将光标放在同一行中。

3. nextLine()读取包括单词之间的空格在内的输入(也就是说，读取到行n的末尾)。
   读取输入后，nextLine()将光标定位到下一行。

<br><br><br><br>

## java中，按数组下标来输出内容

`System.out.println(line.substring(6,11));`
意思是：输出line字符串中下标6到11的字符

<br><br><br><br>

## 怎么把一个变量塞到一个字符串当中

口诀：**加一个引号，双引号中间加两个加号，两个加号之间加变量名**

例如:

```java
return " [ "+no+",单人间，空闲] "；
```

<br><br><br><br>

## 三目运算符

```java
status ? "空闲" : "占用";
/**
如果status是true，那么输出空闲，否则就输出占用
*/
```

<br><br><br><br>

## println(引用)，会自动调用引用toString()方法

<br><br><br><br>

## arrays工具(排序和二分法查找)

```java
public static void main (String [] args){
    
int [] arr={1,3,434,3,232,45,67,5,3}
Arrays.sort(arr);//排序
int index = Arrays.binarySearch(arr,3);//3是所要查询的数据，arr是查询的对象（数组）
System.out.println(index==-1 ? "该元素不存在" : "该元素下标是：" +index);
}
```



## 类和接口的强制类型转换

类在强制类型转换过程中，如果是类转换成接口类型。那么类和接口之间不需要存在继承关系，也可以转换	



