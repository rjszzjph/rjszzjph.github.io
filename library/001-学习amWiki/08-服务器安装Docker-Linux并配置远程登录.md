# 服务器安装Docker-Linux并配置远程登录

## 一、前言

一直都有听说Docker，被传的神乎其神的，所以很早之前就想见识见识庐山真面目了

前几天做实验，服务器装的Centos7,环境实在是太落后，不想折腾环境了，于是正好趁此机会安装一下Docker（没错，我走向了另一条折腾之路～V～）



## 二、Linux安装Docker

本来以为安装Docker又会是一条折腾不归路，但是安装Docker的过程却顺利的让我惊奇，这也是我对Docker赞叹不已的原因之一



#### Centos

1、升级包

```bash
sudo yum update -y
```

2、安装Docker

```bash
sudo yum intsall docker -y
```

3、启动Docker后台服务

```bash
sudo service docker start
```

4、查看Docker版本

```bash
docker version
```

出现以下字样

```bash
$ docker version
Client:
 Version:         1.13.1
 API version:     1.26
```

完成



#### Ubuntu(没有亲自测试)

1、更新软件系统

```bash
sudo apt-get update
```

2、安装依赖

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

3、添加官方密钥

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

回车显示OK，成功

4、再次更新

```bash
sudo apt-get update
```

5、安装docker

```bash
sudo apt-get install docker-ce
```

6、查看版本

```bash
docker -v
```

成功



## 三、在Docker中安装Linux

当然，简单的安装不足以让我为其背书，更加吸引我的是它功能实现体现了一种艺术的美感，如今，许多东西都是越简单越科幻

Docker是一个容器，它将不同的软件隔离开，成为不同的进程，互不干扰

你可以在Docker中安装许许多多的软件，当然操作系统也是软件，因此我们可以在Docker中安装Linux!

更令人惊叹的是它的简单，就像是一个武林高手，没有一招是多余的，毫不拖泥带水



下面以Docker-Ubuntu为例作介绍

1、先从云上拉取一个ubuntu镜像

```bash
sudo docker pull ubuntu
```

默认是最新版本，不过也可以去网站选取自己喜欢的版本  >>>  [Ubuntu 镜像库](https://hub.docker.com/_/ubuntu?tab=tags&page=1)



2、查看镜像

```bash
sudo docker image ls 
```

此时会显示进行对应的ID，启动时需要用到镜像ID

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200721234449573.png)



3、启动镜像

```bash
sudo docker run -itd -p 6789:22 d27b9ffc5667
```

含义：后台启动镜像，-p表示端口映射，将6789端口映射为22(ssh登录端口)，d27b9ffc5667为要启动的镜像ID



4、查看启动的容器

```bash
sudo docker container ls
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200721235045844.png)

可以看到我这里启动了两个Ubuntu，其实用到是同一个镜像(镜像ID一致，容器ID与镜像ID不是同一个东西)



5、进入容器

```bash
sudo docker exec -it ee6281487c44 /bin/bash
```

ee6281487c44是容器ID(第一列)



恭喜，你已经在Docker中拥有了一个Ubuntu

不过服务器上的Linux，我们当然想远程登录，这是根本，还记得之前设置的端口映射不，没错，就是用来ssh远程登录的



## 四、在Docker-Ubuntu中配置ssh远程登录

1、进入容器

```bash
sudo docker exec -it ee6281487c44 /bin/bash
```

2、更新、下载vim与openssh

```ba
apt-get update
apt-get upgrade
apt-get install vim
apt-get install openssh-server
```

3、设置密码，用于远程登录

```bash
passwd
```

4、修改配置文件

```
vim /etc/ssh/sshd_config
```

注释`PermitRootLogin prohibit-password`

添加`PermitRootLogin yes`

保存退出

5、重启ssh服务

```bash
/etc/init.d/ssh restart
```



在本地进行连接

```bash
ssh root@119.3.190.28 -p 6789
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200722000620719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Jqc3p6MTMxNA==,size_16,color_FFFFFF,t_70)

成功！



## 五、后言

想必看到这里，大家都有所感慨，看起来这确实是一个令人兴奋的软件，当我接触它了解它时，我是如此的激动，以至于到处安利，还是那就话，极简之美，从内到外都散发这科幻感的气息



但是这却是冰山一角，连门都未入，不过还是希望这能激发大家探索的欲望与兴趣



再见江湖



### 参考

[CentOS7安装Docker](https://www.jianshu.com/p/3a4cd73e3272)

[Docker Ubuntu上安装ssh和连接ssh](https://blog.csdn.net/qq_43914736/article/details/90608587)

