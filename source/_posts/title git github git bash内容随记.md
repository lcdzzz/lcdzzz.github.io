---
title: git github git bash内容随记
date: 2021-07-04 01:31:33
tag: 
	- 大学
cover: https://images.wallpaperscraft.com/image/single/minimalism_sky_clouds_sun_mountains_lake_landscape_95458_300x168.jpg
---
### 使用git bash 创建文件及文件夹命令

1.cd : 切换到哪个目录下， 如 cd e:\fff 切换 E 盘下面的fff 目录。
　　当我们用cd 进入文件夹时,我们可以使用 通配符*, cd f*, 如果E盘下只有一个f开头的文件夹,它就会进入到这个文件夹.
2.cd … 回退到上一个目录， 注意，cd 和两个点点…之间有一个空格。
3.pwd : 显示当前目录路径。
4.ls(ll): 都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。
5.touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。
6.rm: 删除一个文件, rm index.js 就会把index.js文件删除.
7.mkdir: 新建一个目录,就是新建一个文件夹. 如mkdir src 新建src 文件夹.
8.rm -r : 删除一个文件夹, rm -r src 删除src目录， 好像不能用通配符。
9.mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,必须保证文件和目标文件夹在同一目录下.
10.reset 清屏，把git bash命令窗口中的所有内容清空。

<br><br>

### Vim编辑器的撤销与恢复撤销操作

1. 使用vim修改文档内容

2. 按下Esc，回到命令模式

3. 按下u，可以发现刚刚新加的内容全部消失了，vim编辑器底部还会显示“4 行被去掉的操作提示”，这就是撤销操作。

4. 如果我们想回复到撤销前的内容，按下ctrl+r

   可以看到刚刚被我们撤掉的四行内容又回来了，而且vim编辑器底部会显示“4 行被加入的操作提示”，这就是恢复撤销操作。

<br><br>

### github中查找资源的小技巧：

常用前缀后缀

- 找百科大全 awesome xxx
- 找例子 xxx sample
- 找空项目架子 xxx starter/xxx boilerplate
- 找教程 xxx tutorial

<br><br>

### github中Clone代码

1. 点击绿色的Code图标
2. 复制Clone中HTTPS的网址（git仓库在github上托管的地址）例：【https://github.com/akullpp/awesome-java.git 】
3. 进入一个想要保存这个项目的文件夹
4. 右键：git bash here
