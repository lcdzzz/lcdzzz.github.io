---
title: 解决 java “错误：编码GBK 的不可映射字符”
date: 2021-07-04 06:31:33
categories: Java基础
tags: [错误解决, 大学, 原创]
cover: https://images.wallpaperscraft.com/image/single/mclaren_p1_minimalism_art_98755_300x168.jpg
---

## 解决 java “错误：编码GBK 的不可映射字符”

* 当java程序在idea上可以执行，但在cmd时报错：编码GBK 的不可映射字符时，通常都是因为GB2312编码的问题。

* 比如这个程序。在idea上运行时不会有问题的。用cmd运行时问题就出在第10行的`int xd =0//向导数`上，其中的“向导数”出了问题。

```java
          Scanner in = new Scanner(System.in);
          while (in.hasNext()) {
             int line = in.nextInt();
              while (line-- > 0) {
                  int n = in.nextInt();//人数
                  int xd = 0;//向导数
                  int qx = n / 2;//抢险
                  int yl =(n-qx) *2 / 3;//医疗
                  int tx = n-qx - yl;//通讯
                  if (qx % 10 != 0) {
                      xd = xd + qx / 10 + 1;
                  } else {
                      xd = xd + qx / 10;
                  }
                  if (yl % 10 != 0) {
                      xd = xd + yl / 10 + 1;
                  } else {
                      xd = xd + yl / 10;
                  }
                  if (tx % 10 != 0) {
                      xd = xd + tx / 10 + 1;
                  } else {
                      xd = xd + tx / 10;
                  }
                  System.out.println(xd);
              }
          }
```

* 解决方法

  1. 检查

     查本机区域语言设置中文没有问题

     需要把当前系统区域设置成中文（简体，中国）

  2. 在cmd中输入`javac -encoding UTF-8 HelloWorlewww.java`

     其中  `HelloWorlewww.java`就是你需要运行的java程序

  3. 记事本打开java源文件，另存为选择ANSI编码

  接下来重新用javac等操作编译，编译通过

* 说明：ANSI：美国国家标准协会，系统预设的标准文字储存格式。简体中文编码GB2312，实际上它是ＡＮＳＩ的一个代码页936

  UTF-8：通用字集转换格式，这是为传输而设计的编码，2进制，以8位为单元对Unicode进行编码，如果使用只能在同类位元组内支持8个位元的重要资料一类的旧式传输媒体，可选择UTF-8格式。

  在UTF-8里，英文字符仍然跟ASCII编码一样，因此原先的函数库可以继续使用。而中文的编码范围是在0080-07FF之间，因此是2个字节表示（但这两个字节和GB编码的两个字节是不同的），用专门的Unicode处理类可以对UTF编码进行处理。

