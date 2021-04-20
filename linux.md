## 1.Linux环境启动命令

### 1. 常用

```shell
# 时间同步
ntpdate time.ntp.org

ntpdate 0.asia.pool.ntp.org

# 查看发行版本信息
lsb_release -a
```



### 2. FastDFS

 启动命令

```shell
# 启动tracker
service fdfs_trackerd start2

# 启动tracker
service fdfs_storaged start

# 重启
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart

# 终止
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf stop
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf stop
```

### 3. live-server

```powershell
live-server --port=9002
```



## 2. docker

### 1. docker 删除镜像

```shell
docker rmi image-name/id
```

### 2. 删除容器

```shell
docker rm -f CONTAINER_ID
```

### 3. redis

```shell
# 启动redis
docker run -itd --name redis-name -p 6379:6379 redis:tag

# 连接 redis 服务
docker exec -it redis-name /bin/bash
```

## 3. 虚拟机Centos

### 1.重启网络

```shell
# 启动
systemctl start network.service

# 重启网络
service network restart
```

### 2. 虚拟机 virtual box 网络设置

https://www.linuxidc.com/Linux/2017-01/139345.htm

### 3. 防火墙

#### (1) firewalld的基本使用

```shell
#启动： 
systemctl start firewalld
#查看状态： 
systemctl status firewalld 
#禁用，禁止开机启动： 
systemctl disable firewalld
#停止运行： 
systemctl stop firewalld
```

#### (2) 配置firewalld-cmd

```shell
#查看版本： 
firewall-cmd --version
#查看帮助： 
firewall-cmd --help
#显示状态： 
firewall-cmd --state
#查看所有打开的端口： 
firewall-cmd --zone=public --list-ports
#更新防火墙规则： 
firewall-cmd --reload
#更新防火墙规则，重启服务： 
firewall-cmd --completely-reload
#查看已激活的Zone信息:  
firewall-cmd --get-active-zones
#查看指定接口所属区域： 
firewall-cmd --get-zone-of-interface=eth0
#拒绝所有包：
firewall-cmd --panic-on
#取消拒绝状态： 
firewall-cmd --panic-off
#查看是否拒绝： 
firewall-cmd --query-panic
```

#### (3) 信任级别，通过Zone的值指定

```shell
drop: 丢弃所有进入的包，而不给出任何响应 
block: 拒绝所有外部发起的连接，允许内部发起的连接 
public: 允许指定的进入连接 
external: 同上，对伪装的进入连接，一般用于路由转发 
dmz: 允许受限制的进入连接 
work: 允许受信任的计算机被限制的进入连接，类似 workgroup 
home: 同上，类似 homegroup 
internal: 同上，范围针对所有互联网用户 
trusted: 信任所有连接
```



#### (4) firewall开启和关闭端口

```shell
#以下都是指在public的zone下的操作，不同的Zone只要改变Zone后面的值就可以添加：
firewall-cmd --zone=public --add-port=80/tcp --permanent    #（--permanent永久生效，没有此参数重启后失效）

#重新载入：
firewall-cmd --reload

#查看：
firewall-cmd --zone=public --query-port=80/tcp

#删除：
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```



#### (5) **管理服务**

```shell
#以smtp服务为例， 添加到work zone
#添加：
firewall-cmd --zone=work --add-service=smtp

#查看：
firewall-cmd --zone=work --query-service=smtp

#删除：
firewall-cmd --zone=work --remove-service=smtp
```



#### (6) 配置 IP 地址伪装

```shell
#查看：
firewall-cmd --zone=external --query-masquerade

#打开：
firewall-cmd --zone=external --add-masquerade

#关闭：
firewall-cmd --zone=external --remove-masquerade
```



#### (7) 端口转发

```shell
#打开端口转发，首先需要打开IP地址伪装
firewall-cmd --zone=external --add-masquerade
 
#转发 tcp 22 端口至 3753：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toport=3753

#转发端口数据至另一个IP的相同端口：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toaddr=192.168.1.112

#转发端口数据至另一个IP的 3753 端口：
firewall-cmd --zone=external --add-forward-port=22:porto=tcp:：toport=3753:toaddr=192.168.1.112
```



#### (8) systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体

```shell
#启动一个服务：
systemctl start firewalld.service

#关闭一个服务：
systemctl stop firewalld.service

#重启一个服务：
systemctl restart firewalld.service

#显示一个服务的状态：
systemctl status firewalld.service

#在开机时启用一个服务：
systemctl enable firewalld.service

#在开机时禁用一个服务：
systemctl disable firewalld.service

#查看服务是否开机启动：
systemctl is-enabled firewalld.service

#查看已启动的服务列表：
systemctl list-unit-files|grep enabled

#查看启动失败的服务列表：
systemctl --failed
```

## 4. SCP命令

```shell
#从本地复制到远程
# 拷贝文件
scp /home/test/test.txt root@192.168.0.2:/home/test/
# 拷贝目录
scp -r /home/test/ root@192.168.0.2:/home/test/


#从远程复制到本地
# 拷贝文件
scp root@192.168.0.2:/home/test/ /home/test/test.txt
# 拷贝目录
scp -r root@192.168.0.2:/home/test/ v/home/test/
```

## 5. vim 命令

### (1). 启动

```shell
# 在打开文件前，先执行指定的命令
vim -c cmd file

# 恢复上次异常退出的文件；
vim -r file

# 以只读的方式打开文件，但可以强制保存
vim -R file

# 以只读的方式打开文件，不可以强制保存；
vim -M file

# 将编辑窗口的大小设为num行；
vim -y num file

# 从文件的末尾开始；
vim + file

# 从第num行开始；
vim +num file

# 打开file，并将光标停留在第一个找到的string上。
vim +/string file

# 用已有的vim进程打开指定的文件。 如果你不想启用多个vim会话，这个很有用。但要注意， 如果你用vim，
# 会寻找名叫VIM的服务器；如果你已经有一个gvim在运行了， 你可以用gvim –remote file在已有的gvim
# 中打开文件。
vim –remote file

```

### (2). 文档操作

```shell

```

## 6. maven 配置mirros不生效

问题描述
有时候出现一种奇怪的问题，就是无论如何修改远程库，镜像库的地址，好像都不生效。
下面给出排查方法与思路。

解决步骤
1) 首先检查eclipse或者IDEA中配置的settings文件是否是自己编辑的那个文件。
2) 如果是，那么检查配置的镜像库mirror，看是否有mirrorOf配置的是* ,这个值表示匹配所有的镜像库，相当于会屏蔽掉配置的其他的镜像库。
3) 注意配置的优先级，pom 配置 > settings中的配置
4) 如果配置没有问题，那么检查settings文件是否有语法错误。特别是当出现不管怎么改远程库地址，拉取jar的时候都跑去中央仓库下载的现象时。有时候频繁修改或者复制粘贴，导致settings文件语法出现了错误。可以在命令行执行下面的命令，如果有语法错误，会得到提示。



```shell
mvn help:effective-settings
```

## 7.java程序Cpu占用过高排查

### 1). top 命令查看占用CPU的进程id

![image-20210420100331615](C:\Users\liuk\AppData\Roaming\Typora\typora-user-images\image-20210420100331615.png)

### 2). 定位到异常进程id后 再通过ps命令查看这个程序的线程信息

​		

```shell

# pid 进程id
# tid 线程id
# time 该线程已运行时间
ps -mp pid -o THREAD,tid,time
```

![image-20210420101114118](C:\Users\liuk\AppData\Roaming\Typora\typora-user-images\image-20210420101114118.png)

### 3). 将tid 转换为16进制，方便jstack进行定位

```shell
printf "%x\n" number
```

![image-20210420101459875](C:\Users\liuk\AppData\Roaming\Typora\typora-user-images\image-20210420101459875.png)

### 4). 使用jstack工具查看进程信息

```shell
jstack pid | grep tid
```

![image-20210420101748097](C:\Users\liuk\AppData\Roaming\Typora\typora-user-images\image-20210420101748097.png)

可以看到该线程处于运行状态，使用jstack查看详细信息

```
jstack pid
```

![image-20210420102115256](C:\Users\liuk\AppData\Roaming\Typora\typora-user-images\image-20210420102115256.png)

定位到具体行