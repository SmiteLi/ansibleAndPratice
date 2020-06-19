# [»](https://www.vagrantup.com/intro/getting-started/synced_folders#synced-folders)Synced Folders



While it is cool to have a virtual machine so easily, not many people want to edit files using just plain terminal-based editors over SSH. Luckily with Vagrant you do not have to. By using *synced folders*, Vagrant will automatically sync your files to and from the guest machine.

By default, Vagrant shares your project directory (remember, that is the one with the Vagrantfile) to the `/vagrant` directory in your guest machine.

Note that when you `vagrant ssh` into your machine, you're in `/home/vagrant`. `/home/vagrant` is a different directory from the synced `/vagrant` directory.

If your terminal displays an error about incompatible guest additions (or no guest additions), you may need to update your box or choose a different box such as `hashicorp/bionic64`. Some users have also had success with the [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) plugin, but it is not officially supported by the Vagrant core team.

Run `vagrant up` again and SSH into your machine to see:

如此轻松地拥有虚拟机虽然很酷，但没有多少人愿意仅通过SSH使用基于终端的普通编辑器来编辑文件。 幸运的是，与Vagrant一起您不必这样做。 通过使用同步的文件夹，Vagrant将自动将文件与来宾计算机同步。



默认情况下，Vagrant与您的客户机中的/ vagrant目录共享您的项目目录（请记住，该目录与Vagrantfile共享）。



请注意，当您将ssh放到计算机中时，您就在/ home / vagrant中。 / home / vagrant是与同步的/ vagrant目录不同的目录。



如果您的终端显示有关不兼容的来宾添加（或没有来宾添加）的错误，则您可能需要更新您的框或选择其他框，例如hashicorp / bionic64。 一些用户在vagrant-vbguest插件上也取得了成功，但Vagrant核心团队并未正式支持该插件。



再次运行流浪汉并通过SSH进入您的计算机以查看：

```shell-session
$ vagrant up
...
$ vagrant ssh
...
vagrant@bionic64:~$ ls /vagrant
Vagrantfile
```

Believe it or not, that Vagrantfile you see inside the virtual machine is actually the same Vagrantfile that is on your actual host machine. Go ahead and touch a file to prove it to yourself:

信不信由你，您在虚拟机中看到的Vagrantfile实际上与实际主机上的Vagrantfile相同。 继续触摸文件以向自己证明：

```shell-session
vagrant@bionic64:~$ touch /vagrant/foo
vagrant@bionic64:~$ exit
$ ls
foo Vagrantfile
```

Whoa! "foo" is now on your host machine. As you can see, Vagrant kept the folders in sync.

With [synced folders](https://www.vagrantup.com/docs/synced-folders/), you can continue to use your own editor on your host machine and have the files sync into the guest machine.

哇！ 现在，“ foo”在您的主机上。 如您所见，Vagrant保持文件夹同步。



使用同步的文件夹，您可以继续在主机上使用自己的编辑器，并将文件同步到客户机中。