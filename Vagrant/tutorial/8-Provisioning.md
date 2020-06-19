# [»](https://www.vagrantup.com/intro/getting-started/provisioning#provisioning)Provisioning

JUMP TO SECTION

Alright, so we have a virtual machine running a basic copy of Ubuntu and we can edit files from our machine and have them synced into the virtual machine. Let us now serve those files using a webserver.

We could just SSH in and install a webserver and be on our way, but then every person who used Vagrant would have to do the same thing. Instead, Vagrant has built-in support for *automated provisioning*. Using this feature, Vagrant will automatically install software when you `vagrant up` so that the guest machine can be repeatably created and ready-to-use.

好了，因此我们有一个运行Ubuntu基本副本的虚拟机，我们可以从我们的计算机中编辑文件并将它们同步到虚拟机中。 现在，让我们使用网络服务器来提供这些文件。



我们可以通过SSH进行安装并安装Web服务器并继续使用，但是使用Vagrant的每个人都必须做同样的事情。 相反，Vagrant具有对自动配置的内置支持。 使用此功能，Vagrant将在您无所事事时自动安装软件，以便可以重复创建来宾计算机并准备使用它。

## [»](https://www.vagrantup.com/intro/getting-started/provisioning#installing-apache)Installing Apache

We will just setup [Apache](http://httpd.apache.org/) for our basic project, and we will do so using a shell script. First, we need to add some html content which will be served by the Apache webserver. This will act as our DocumentRoot folder. To do this create a subdirectory named `html` in the project root directory. In the `html` directory create a html file named `index.html`. For example:

我们将为基本项目设置Apache，并使用Shell脚本进行设置。 首先，我们需要添加一些HTML内容，这些内容将由Apache Web服务器提供。 这将充当我们的DocumentRoot文件夹。 为此，在项目根目录中创建一个名为html的子目录。 在html目录中，创建一个名为index.html的html文件。 例如：

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>Getting started with Vagrant!</h1>
  </body>
</html>
```

The script below will symlink our shared folder `/vagrant` so that apache serves the `html` folder when accessing the root page locally. Now, create the following shell script and save it as `bootstrap.sh` in the same directory as your Vagrantfile:

下面的脚本将符号链接我们的共享文件夹/ vagrant，以便在本地访问根页面时apache提供html文件夹。 现在，创建以下shell脚本，并将其另存为bootstrap.sh与Vagrantfile相同的目录中：

```bash
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```

Next, we configure Vagrant to run this shell script when setting up our machine. We do this by editing the Vagrantfile, which should now look like this:

接下来，我们在配置计算机时将Vagrant配置为运行此Shell脚本。 我们通过编辑Vagrantfile来做到这一点，它现在看起来应该像这样：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

The "provision" line is new, and tells Vagrant to use the `shell` provisioner to setup the machine, with the `bootstrap.sh` file. The file path is relative to the location of the project root (where the Vagrantfile is).

“ provision”这一行是新的，它告诉Vagrant使用Shell Provisioner和bootstrap.sh文件来设置机器。 文件路径是相对于项目根目录（Vagrantfile所在的位置）的路径。

## [»](https://www.vagrantup.com/intro/getting-started/provisioning#provision)Provision!

After everything is configured, just run `vagrant up` to create your machine and Vagrant will automatically provision it. You should see the output from the shell script appear in your terminal. If the guest machine is already running from a previous step, run `vagrant reload --provision`, which will quickly restart your virtual machine, skipping the initial import step. The provision flag on the reload command instructs Vagrant to run the provisioners, since usually Vagrant will only do this on the first `vagrant up`.

After Vagrant completes running, the web server will be up and running. You cannot see the website from your own browser (yet), but you can verify that the provisioning works by loading a file from SSH within the machine:

配置完所有内容后，只需运行vagrant即可创建您的计算机，Vagrant会自动对其进行配置。 您应该看到shell脚本的输出出现在终端中。 如果来宾计算机已从上一步运行，请运行vagrant reload --provision，它将快速重新启动虚拟机，而跳过初始导入步骤。 reload命令上的Provisioning标志指示Vagrant运行预配器，因为通常Vagrant仅在第一个Vagrant上执行此操作。



Vagrant完成运行后，Web服务器将启动并运行。 您尚无法从自己的浏览器中看到该网站（但），但是您可以通过在计算机中从SSH加载文件来验证配置是否有效：

```shell-session
$ vagrant ssh
...
vagrant@bionic64:~$ wget -qO- 127.0.0.1
```

This works because in the shell script above we installed Apache and setup the default `DocumentRoot` of Apache to point to our `/vagrant` directory, which is the default synced folder setup by Vagrant.

You can play around some more by creating some more files and viewing them from the terminal, but in the next step we will cover networking options so that you can use your own browser to access the guest machine.

**For complex provisioning scripts**, it may be more efficient to package a custom Vagrant box with those packages pre-installed instead of building them each time. This topic is not covered by the getting started guide, but can be found in the [packaging custom boxes](https://www.vagrantup.com/docs/boxes/base) documentation.

之所以有效，是因为在上面的shell脚本中，我们安装了Apache，并设置了Apache的默认DocumentRoot指向我们的/ vagrant目录，这是Vagrant设置的默认同步文件夹。



您可以通过创建更多文件并从终端查看这些文件来进行更多操作，但是在下一步中，我们将介绍网络选项，以便您可以使用自己的浏览器访问客户机。



对于复杂的配置脚本，将自定义的Vagrant框与那些预先安装的软件包打包在一起而不是每次都构建它们，可能会更有效率。 入门指南未涵盖此主题，但可以在包装定制盒文档中找到该主题。