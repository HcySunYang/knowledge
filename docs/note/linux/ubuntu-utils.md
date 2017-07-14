## Ubuntu使用知识积累

<img src="http://7xlolm.com1.z0.glb.clouddn.com/20151226Ubuntu.jpg" />

#### Ubuntu下安装git和node

当时在做项目迁移的时候并没有服务器，所以只能用旁边的一台windows安装个双系统，来充当服务器了，安装方法网上很多，不过我推荐一个朋友的博客中的一篇文章，我所安装的Ubuntu的版本是14.04的，并且是server版的，与该博客不符，但是原理相同，贴出[博客地址](https://xuri.me/2013/04/09/easybcd-install-ubuntu.html)。

#### Ubuntu 下安装 git

Ubuntu的包管理是apt-get，可以使用如下命令安装git:

```sh
sudo apt-get install git
```

但是这样安装的git版本是比较老的，并不推荐这样安装，如果想安装最新可用版本的可以执行下面的命令：

```sh
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

如果提示 

```sh
sudo: add-apt-repository: command not found
```

则需要先安装依赖包：

```sh
sudo apt-get install python-software-properties
```

然后再次执行那三条命令即可，安装完成后可以查看git版本，确定安装是否成功

```
git --version
```

#### Ubuntu 下安装 node

首先非常推荐大家使用 [nvm](https://github.com/creationix/nvm/blob/master/README.md#installation) 来管理 `node`，nvm 允许我们安装多个版本的node并且轻松管理、切换、使用。

如果要采用的源码编译安装的方式，首先去[node得资源列表](https://nodejs.org/dist/)找到你要现在的资源包，如我所选择的资源包是：

https://nodejs.org/dist/v5.0.0/node-v5.0.0.tar.gz

选择好资源包后执行下面的命令即可：

```
# 下载资源
wget https://nodejs.org/dist/v5.0.0/node-v5.0.0.tar.gz
# 解压
tar zxvf node-v5.0.0.tar.gz
# 编译安装
cd node-v5.0.0
./configure 
make 
make install
cp /usr/local/bin/node /usr/sbin/
```

查看是否安装成功

```
node -v
```

如果正确输出版本号，那么说明我们安装成功了，也有其他安装方式，大家去网上看看吧

#### Ubuntu默认是用dash解析shell脚本

由于项目中我使用到了shell脚本，所以当项目迁移到Ubuntu的时候报错：

```
source xxx.sh时提示 source: not found
```

原因是当时项目在Mac下跑得，Mac解析shell脚本使用的是bash，而Ubuntu默认是用的dash，我们可以使用下面的命令查看：

```
ls -l `which sh`
```

如下图：

<img src="http://7xlolm.com1.z0.glb.clouddn.com/201512266.pic.jpg" width="500" />

我们可以修改Ubuntu解析shell的方式：

```
sudo dpkg-reconfigure dash
```

执行该命令后会出现如下图所示的选项，我们只需要选择否即可：

<img src="http://7xlolm.com1.z0.glb.clouddn.com/201512264.pic.jpg" width="500" />

这时在查看一下Ubuntu解析shell的方式：

```
ls -l `which sh`
```

如下图，已经变为了 bash

<img src="http://7xlolm.com1.z0.glb.clouddn.com/201512263.pic.jpg" width="500" />

#### Ubuntu下mongodb启动失败

如下图：

<img src="http://7xlolm.com1.z0.glb.clouddn.com/201512267.pic_hd.jpg" width="500" />

造成启动失败的原因有很多，其中最常见的就是非正常退出，下一次再启动的时候就会遇到如上情况，在Mac下或者windows下，一般我们需要把mongod.lock文件删除就可以成功重启了，但是今天在Ubuntu下却不行，在Ubuntu下记得要把mongodb-27017.sock文件也删除掉：

```
sudo rm -rf /var/lib/mongodb/mongod.lock
sudo rm -rf /tmp/mongodb-27017.sock
```

之后重启：

```
sudo service mongod start
```

就可以正常启动了

#### nohup 命令让进程在后台运行

当时我是使用ssh远程登录的，又不想多开窗口，所以希望node服务在后台运行，这时候可以使用nohup命令，该命令能够让进程在后台运行，nohup命令的格式如下：

```
nohup Command [ Arg ... ] [　& ] 
```

例如我想要node的web服务在后台运行，在我的项目中可以执行下面的代码带到目的：

```
nohup node admin.js &
```

#### Ubuntu下添加用户并赋予root权限

在Ubuntu下可以使用 adduser 命令添加一个用户：

```
adduser <username>
```

执行该命令之后会让你为该用户设置密码，但是这样添加的用户是没有使用root权限的用户，我们需要一些配置来赋予该用户使用root权限的能力，修改配置需要编辑 /etc/sudoers 文件，这个文件默认是只读的，所以第一步修改文件为可写：

```
chmod u+w /etc/sudoers
```

然后编辑该文件：

```
sudo vim /etc/sudoers
```

找到下面这句话：

```
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

在其 %sudo   ALL=(ALL:ALL) ALL 下面再添加一行：

```
%username   ALL=(ALL:ALL) ALL
```

username为你创建用户的用户名，之后保存退出，并还原 sudoers 文件只读的权限：

```
chmod u-w /etc/sudoers
```

接下来新用户就可以登录并使用root权限了。

#### 安装完Ubuntu后乱码

修改/etc/default/locale

```
sudo vim /etc/default/locale
```

将：

```
LANG=zh_CN.UTF-8
LANGUAGE=zh_CN:zh
```

修改为：

```
LANG="en_US.UTF-8"
LANGUAGE="en_US:en"
```

最后reboot一下就可以了：

```
sudo reboot
```

#### 查看端口占用情况，并杀掉进程

```sh
# 其中 xxxx 代表你要查的端口号
lsof -i:xxxx
```

上面的命令会列出占用该端口的进程，找到对应的PID，杀掉即可：

```sh
# 其中 xxxxx 表示PID
kill -9 xxxxx
```