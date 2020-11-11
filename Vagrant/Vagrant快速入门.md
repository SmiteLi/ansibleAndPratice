# Vagrant快速入门

## 1. vagrant是什么？

vagrant是一款用命令行管理虚拟机的开源软件，可与virtualbox、VMware等配合使用

## 2. 安装vagrant

vagrant默认是和virtualbox搭配使用，所以在安装vagrant前需要安装virtualbox。

- 安装最新的virtualBox：https://www.virtualbox.org/wiki/Downloads
- 安装最新版的Vagrant：https://www.vagrantup.com/downloads

## 3. 用vagrant快速创建ubuntu18.04虚拟机

在ubuntu目录内使用如下命令：

```
$ vagrant init hashicorp/bionic64
$ vagrant up
```

vagrant init命令在ubuntu目录内生成一个名为Vagrantfile的文件，该文件描述如何创建虚拟机，如定义用的OS镜像（vagrant这里称为box），虚拟机的CPU核数、内存的大小、网络的参数等

vagrant up命令根据Vagrantfile启动该虚拟机。

> 运行vagrant up时，如果本地没有hashicorp/bionic64这个box，vagrant会到hashicorp公司的box仓库（https://vagrantcloud.com/boxes/search）拉取。由于网络原因，速度很慢或者无法连接，这时候需要下载好box到本地，再导入。
>
> hashicorp/bionic64 的box放在百度网盘：
>
> 导入box命令：vagrant box add --name hashicorp/bionic64 ./hashicorpbionic64

有可能vagrant up时自动会更新系统的软件包，这时候可以另外打开一个terminal，然后用vagrant ssh进入系统，手动更新系统包到最新：

-  sudo -i 切换到root用户
- vi /etc/apt/sources.list，替换为阿里云的源：

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```

- apt update && apt -y upgrade

如果遇到apt命令正在被占用，那么需要杀死该apt进程，然后手动更新。

## 4. 如何添加box？

下面的命令用于添加box到本地：

```
$ vagrant box add hashicorp/bionic64
```

box类似虚拟机安装时的系统ISO镜像。

在`Vagrantfile` 指定使用的box：

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
end
```

通过config.vm.box_version参数指定box的版本，例如：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_version = "1.1.0"
end
```

通过config.vm.box_url参数指定box的来源URL：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_url = "https://vagrantcloud.com/hashicorp/bionic64"
end
```

## 5. ssh到虚拟机

上面我们已经通过vagrant up启动了虚拟机，下面通过vagrant ssh命令进入虚拟机。默认情况下我们是用vagrant用户通过私钥登录，并且进入虚拟机后，位于/home/vagrant目录下。

如图：

![image-20200623112210962](C:\Users\long\AppData\Roaming\Typora\typora-user-images\image-20200623112210962.png)

之后就和使用虚拟机一样了。

## 6. 目录同步

默认情况下，Vagrant会将Vagrantfile的目录同步到虚拟机的/ vagrant目录中，实践中你很大概率发现会不生效，因为这需要box镜像的 GuestAdditions和virtualbox配合起来才能实现。下面介绍利用vagrant-vbguest插件来解决。

安装vagrant-vbguest插件：

```
vagrant plugin install vagrant-vbguest
```

更多该插件的用法参考插件的github地址：https://github.com/dotless-de/vagrant-vbguest

如果需要自定义同步的文件夹，修改Vagrantfile如下:

```
Vagrant.configure("2") do |config|
  # other config here
  config.vm.synced_folder "data", "/data"
end
```

data为ubuntu目录的子目录，/data为虚拟机内的目录。

然后reload Vagrantfile：

```
vagrant reload
```

## 7. 网络设置

- 设置本地的4567端口转发到虚拟机的80端口：

Vagrantfile：

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```

- 配置私有静态ip：

```
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4"
end
```

- 通过DHCP配置public网络：

```
Vagrant.configure("2") do |config|
  config.vm.network "public_network"
end
```

- 配置公共的静态IP：

```
Vagrant.configure("2") do |config|
  config.vm.network "public_network", ip: "192.168.0.17"
end
```

一般选择“通过DHCP配置网络”这种方法。

## 8. 关闭虚拟机

不再使用时，有三种方法处理虚拟机：

1. 挂起：`vagrant suspend`。再次开始工作时，只需运行`vagrant up`，将从上次中断的地方恢复。 这种方法的主要优点是它超级快，通常只需要5到10秒即可停止和开始工作。 缺点是虚拟机仍会占用磁盘空间，甚至需要更多磁盘空间才能将虚拟机RAM的所有状态存储在磁盘上。
2. 关机：`vagrant halt`。准备再次启动时，可以使用vagrant up。 这种方法的好处是它将干净地关闭计算机，保留磁盘的内容，并允许它重新启动。 缺点是从冷启动开始将花费一些额外的时间，并且虚拟机仍会占用磁盘空间。
3. 清理虚拟机：`vagrant destroy`将删除虚拟机的所有内容。停止虚拟机，将其关闭电源，并卸下所有硬盘，回收来宾计算机消耗的磁盘空间和RAM，并使主机保持干净。 不利的一面是，再次vagrant up时将花费一些额外的时间，因为必须重新导入box并重新配置虚拟机。