---
title: windowsのwsl安装centos
date: 2025-07-31 00:25:54
tags: [Linux, WSL, 原创]
categories: 运维
cover: https://img0.baidu.com/it/u=4198488413,3086794076&fm=253&fmt=auto&app=138&f=JPEG?w=501&h=500
---

# 打开虚拟化

在安装 WSL 2 之前，必须启用虚拟机平台可选功能。计算机将需要虚拟化功能才能使用此功能。

**执行命令(管理员)**

```cmd
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```



# Chocolatey安装

NuGet（读作New Get）是用于微软.NET开发平台的软件包管理器，是一个Visual Studio的扩展。Chocolatey 是基于 NuGet 的一个软件包管理器，就像 Linux 中的 yum 或 apt 一样，在 Windows10 中也可以用命令行安装程序了

**以管理员身份打开powershell.exe,执行以下代码**

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

完成后，输入命令：`choco` ，如果能正确显示版本号，说明安装成功



# LxRunOffline安装（需要重启终端）

LxRunOffline 是非常实用的 WSL 管理软件，可以备份、创建、恢复、导出WSL子系统，也可以安装适配 WSL 的任何 Linux 发行版，可以将 WSL 子系统安装到任意目录中。

执行命令

```powershell
choco install lxrunoffline -y
```



# 镜像安装

依次执行以下命令

```cmd
 # 将wsl默认版本设置为2
 wsl --set-default-version 2
 
 # -n 后面是安装的实例名称，-d 后面是想将实例安装到的位置，-f 后面是镜像包所在路径。镜像要求为容器镜像，不能直接使用发布版。
 lxrunoffline install -n CentOS -d C:/work_space/wsl/centoszd -f C:/work_space/wsl/images/rootfs.tar.gz
 # 将已经安装的实例版本设置为 WSL2（可选，也可以将2改为1）
 wsl --set-version CentOS 2
```

可以从**[GitHub - mishamosher/CentOS-WSL: A GitHub Actions automated CentOS RootFS to use with WSL](https://github.com/mishamosher/CentOS-WSL)**获取centos镜像

也可以直接使用百度云：
链接: https://pan.baidu.com/s/15OODsqT6QXqDpc-EXe0kFg?pwd=1xpv 提取码: 1xpv 




# WSL简易维护命令行

```cmd
#升级wsl内核
wsl --update
#查看安装的子系统
wsl -l -v 
#启用子系统 
wsl -s centos 
#停用子系统
wsl -t centos 
#注销子系统
wsl --unregister centos 
#导出系统镜像
wsl --export centos D:\rootfs.tar

#【可选】centos8配置yum源
cd /etc/yum.repos.d
rm -rf ./*
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo 
yum -y clean all #清除所有文件
yum -y makecache #建立缓存
yum repolist  #查看yum仓库信息
```



# 使用systemctl

使用管理员权限打开文件`/etc/wsl.conf`，并加上以下内容

```conf
[boot] 
systemd=true
```



在cmd中运行

```cmd
# 配置选项仅适用于在 Windows 内部版本 19041 及更高版本中作为 WSL 2 运行的分发版。
# 请记住，可能需要运行 `wsl --shutdown`来关闭 WSL 2 VM，然后运行`wsl`重启 WSL 实例以使这些更改生效。

# 关闭
wsl --shutdown

#重启
wsl
```

官方文档：[WSL 中的高级设置配置 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)



# 映射windows程序

新建`/usr/mysoft`，并追加到环境变量path中，之后将windows软件软连接到这个目录即可

```bash
mkdir /usr/mysoft
vi /etc/profile

# 在/etc/profile末尾加上
export PATH=$PATH:/usr/mysoft

# 使之生效
source /etc/profile

```

如Notepad++

`ln -s "/mnt/d/software/Notepad++/notepad++.exe" /usr/mysoft/note`

接下来 就可以使用`note aaa.txt`等命令来编写文件了



> 推荐链接：[docker](https://lcdzzz.github.io/2024/06/05/docker-bi-ji/) 使用docker让wsl可以做更多的事



# 补档

这个错误表示你的系统在尝试从阿里云的镜像源下载 `vim-enhanced` 包时，无法解析镜像服务器的主机名。通常，这种问题可能是由于 DNS 配置问题、网络连接问题或镜像源不可用造成的。

你可以尝试以下步骤来解决该问题：

1. **检查网络连接**： 确保你的机器能够连接到互联网，可以通过 `ping` 测试其他网站来验证网络连接，例如：

   ```bash
   ping www.google.com
   ```

2. **检查 DNS 配置**： 如果网络连接正常，但 DNS 无法解析镜像主机名，你可以尝试更改 DNS 设置，使用公共 DNS 服务器（如 Google 的 `8.8.8.8` 和 `8.8.4.4`）。编辑 `/etc/resolv.conf` 文件，加入以下内容：

   ```bash
       nameserver 8.8.8.8
       nameserver 8.8.4.4
   ```

3. **切换镜像源**： 如果你怀疑阿里云的镜像源当前不可用，可以尝试切换到其他镜像源。你可以编辑 CentOS 的 YUM 配置文件，切换为其他镜像源，例如：

   ```bash
   sudo vi /etc/yum.repos.d/CentOS-Base.repo
   ```

   修改 `baseurl` 和 `mirrorlist` 为其他可用的镜像源地址，例如：

   ```bash
   baseurl=http://mirror.centos.org/centos/7/os/x86_64/
   ```

4. **清理缓存**： 清理 YUM 缓存后再试安装：

   ```bash
   sudo yum clean all
   sudo yum makecache
   sudo yum install vim
   ```

   
