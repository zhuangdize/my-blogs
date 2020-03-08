---
title: CentOS 7 下的 Jenkins 安装方法
categories:
- server
---

## Jenkins

Jenkins是一款开源的软件，它可以帮助我们自动化构建程序，在日常工作中，最常见的一个应用场景就是用来帮助我们自动化部署代码。

## 安装环境准备

由于Jenkins是一款基于Java开发的持续集成工具，所以我们需要安装依赖环境 Java JDK

- Java8 JDK

## 利用 yum 进行安装 Java JDK

```bash
yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel
```

## 查看 Java 安装是否生效

出现以下信息则证明是安装生效的

{% asset_img QQ20200307-1@2x.png This is an example image %}

## 安装 Jenkins

- 首先利用curl工具导入GPS秘钥来启用Jenkins的存储库

```bash
curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

// 或者也可以使用这个方式进行安装
// 事先要安装 wget 工具
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

- 启用Jenkins的存储库成功后，可以通过以下命令安装 Jenkins

```bash
sudo yum install jenkins
```

- 使用systemctl启动jenkins，当然您也可以使用service start jenkins

```bash
sudo systemctl start jenkins

// or

sudo service jenkins start
```

- 查看jenkins服务的状态，判断是否开启成功

```bash
sudo systemctl status jenkins

// or

sudo service jenkins status
```
启动成功的效果类似以下图片

{% asset_img QQ20200307-2@2x.png %}

- 设置为开机启动

```bash
systemctl enable jenkins
```
设置完成之后就可以打开 http://your_ip_address:8080 来进行可视化的 Jenkins 安装步骤 - 这里暂不展示，跟随Jenkins安装提示进行就可以。

- 其他

如果遇到 Jenkins 下载插件缓慢的问题，可以按照以下步骤进行更换 Jenkins 插件的下载源

```
// cd 到你的Jenkins工作目录 以下地址为默认 您也可以自行修改配置
// Tips: 使用 rpm -ql Jenkins 可以查看 Jenkins 默认安装相关的路径

$ cd /var/lib/jenkins/updates 
$ vim default.json
```

编辑 default.json 文件 然后使用如下方式替换插件下载的所有URL

```bash
1,$s/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g
```

替换连接测试url

```bash
1,$s/http:\/\/www.google.com/https:\/\/www.baidu.com/g
```

注意：进入 vim 先输入 ```:``` 在填入上面的地址，修改完成后保存退出。之后重新启动Jenkins服务

```bash
service jenkins restart
```

参考链接：

https://www.cnblogs.com/hellxz/p/jenkins_install_plugins_faster.html
https://linuxize.com/post/how-to-install-jenkins-on-centos-7/#top