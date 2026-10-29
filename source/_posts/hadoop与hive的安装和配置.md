---
title: hadoop与hive的安装和配置
date: 2024-06-20 18:00:09
tags: [Hadoop, Hive, 大数据,Hadoop,原创]
categories: 运维
cover: https://img2.baidu.com/it/u=3645694001,4033166664&fm=253&fmt=auto&app=138&f=PNG?w=300&h=248
---

# ssh登录过程

1. **连接请求**

   用户通过 SSH 客户端发起连接请求，指定目标主机和用户名，即`ssh username@hostname`

2. **服务器响应**

   服务器收到连接请求后，会发送一个公钥给客户端。

3. **公钥验证**

   客户端收到服务器发送的公钥后，会检查这个公钥是否在自己的 `known_hosts` 文件中。如果没有找到，会发出警告，询问用户是否信任这个公钥，用户可以选择是或否。

   ```bash
   ssh 111.229.25.127
   The authenticity of host '111.229.25.127 (111.229.25.127)' can't be established.
   ECDSA key fingerprint is SHA256:1OLYAxTlNkqPEPqX4n2Pvcg8hBzYbqD+UOfVTseEMYo.
   Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
   ```

4. **生成会话密钥**

   如果客户端接受了服务器的公钥，客户端会生成一个称为**会话密钥**（session key）的随机数，并用服务器的公钥对其进行加密。这样只有服务器可以解密这个会话密钥。

5. **发送加密数据**

   客户端使用生成的会话密钥加密一个随机数，并发送给服务器。

6. **验证身份**

   服务器使用自己的私钥解密客户端发来的数据，得到客户端生成的随机数。服务器验证客户端是否能够正确地生成和解密这个随机数，以确认客户端的身份。

7. **建立加密会话**

   如果身份验证成功，服务器和客户端将使用之前生成的会话密钥来加密和解密后续的通信数据，保证通信的安全性和完整性。









# hadoop


```bash
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_rsa
ssh-copy-id hadoop@DESKTOP-FTQ64CF
#cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
#如果是想要本地wsl去免密登录远程服务器，就把本地的 ~/.ssh/id_rsa.pub的内容复制到远程服务器的/root/.ssh/authorized_keys 中
```



```bash
vi /etc/profile

export JAVA_HOME=/usr/local/java/jdk1.8.0_221
export PATH=$JAVA_HOME/bin:$PATH 

export HADOOP_HOME=/home/hadoop/hadoop-3.3.6

export PATH=$PATH:$HADOOP_HOME/bin

export PATH=$PATH:$HADOOP_HOME/sbin

source /etc/profile

hadoop version

```



`/home/hadoop/hadoop-3.3.6/etc/hadoop`



 `hadoop-env.sh`

```bash
export JAVA_HOME=/home/hadoop/java/jdk1.8.0_221

export HADOOP_HOME=/home/hadoop/hadoop-3.3.6

export HDFS_NAMENODE_USER=hadoop
export HDFS_DATANODE_USER=hadoop
export HDFS_SECONDARYNAMENODE_USER=hadoop
export YARN_RESOURCEMANAGER_USER=hadoop
export YARN_NODEMANAGER_USER=hadoop
```



`core-site.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
<!-- 指定 NameNode 的地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9090</value>
    </property>

    <!-- 指定 hadoop 数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/hdfs/tmp</value>
    </property>

    <property>
        <name>hadoop.proxyuser.root.hosts</name>
        <value>*</value>
    </property>

    <property>
        <name>hadoop.proxyuser.root.groups</name>
        <value>*</value>
    </property>
	<!--配置root用户能够代理的用户为任意用户-->
<property>
    <name>hadoop.proxyuser.root.users</name>
    <value>*</value>
	
</property>
<property>
  <name>hadoop.proxyuser.hadoop.hosts</name>
  <value>*</value>
</property>

<property>
  <name>hadoop.proxyuser.hadoop.groups</name>
  <value>*</value>
</property>

</configuration>

```



`hdfs-site.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
<property>
        <name>dfs.replication</name>      
        <value>1</value>
        <description>副本个数，配置默认是3,应小于datanode机器数量</description>
    </property>  

    <property>
        <name>dfs.namenode.name.dir</name>  
        <value>/home/hadoop/hdfs/name</value>  
        <final>true</final>
    </property>  

    <property>
        <name>dfs.datanode.data.dir</name>  
        <value>/home/hadoop/hdfs/data</value>  
        <final>true</final>
    </property>  

    <property>
        <name>dfs.http.address</name>
        <value>0.0.0.0:50070</value>
        <description>将绑定IP改为0.0.0.0，而不是本地回环IP，这样，就能够实现外网访问本机的50070端口了</description>
    </property>

    <property>
        <name>dfs.permissions</name>  
        <value>false</value>
    </property>
</configuration>

```



`yarn-site.xml`

```xml
<configuration>

<!-- Site specific YARN configuration properties -->
<property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```



```bash
hdfs namenode -format

cd /home/hadoop/hadoop-3.3.6/sbin/
./start-all.sh
```



# hive 

```bash
cd /home/hadoop
curl https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz -O
tar -zxvf apache-hive-3.1.3-bin.tar.gz 
mv apache-hive-3.1.3-bin/ hive
```



```bash
vi /home/hadoop/.bash_profile

export HIVE_HOME=/home/hadoop/hive #hive文件夹的路径
export PATH=$PATH:$HIVE_HOME/bin

#修改/root/hive/conf下的hive-site.xml
cd /home/hadoop/hive/conf
mv hive-default.xml.template hive-default.xml
```



`hive-site.xml`

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://111.229.25.127:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false</value>
    <description>JDBC connect string for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.cj.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
    <description>username to use against metastore database</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>wqeq</value>
    <description>password to use against metastore database</description>
  </property>
  <property>
  <name>hive.security.authorization.enabled</name>
  <value>true</value>
</property>
<property>
  <name>hive.server2.thrift.bind.host</name>
  <value>172.26.160.119</value>  <!-- 服务器的 IP 地址 -->
</property>
<property>
  <name>hive.server2.thrift.port</name>
  <value>10000</value> 
</property>
<property>
  <name>hive.server2.enable.doAs</name>
  <value>true</value>
</property>
<property>
  <name>hive.server2.enable.impersonation</name>
  <value>true</value>
</property>
</configuration>
```



设置mysql【可选】

```mysql
create database hive; 
#用户hive的密码是hive
CREATE USER 'hive'@'localhost' IDENTIFIED BY 'hive';
#授权，将所有数据库的所有表的所有权限赋给hive用户，后面的hive是配置hive-site.xml中配置的连接密码
GRANT ALL PRIVILEGES ON *.* TO 'hive'@'localhost';
#刷新mysql系统权限关系表
FLUSH PRIVILEGES;

```



下载MySQL JDBC驱动程序

https://mvnrepository.com/artifact/com.mysql/mysql-connector-j/8.4.0

把jar包放进lib里面

```bash
cp mysql-connector-j-8.4.0.jar /home/hadoop/hive/lib
```

初始化元数据存储的数据库模式

```bash
cd /home/hadoop/hive
 ./bin/schematool -initSchema -dbType mysql
 
#此时mysql数据库中应该有表了
```

**启动Hive**
启动hive之前，先启动hadoop集群。

```bash
start-all.sh #启动Hadoop
hive --service hiveserver2
hive --service metastore
hive  #启动Hive
```

