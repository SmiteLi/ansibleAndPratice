# [»](https://www.vagrantup.com/intro/getting-started/teardown#teardown)Teardown

JUMP TO SECTION

We now have a fully functional virtual machine we can use for basic web development. But now let us say it is time to switch gears, maybe work on another project, maybe go out to lunch, or maybe just time to go home. How do we clean up our development environment?

With Vagrant, you *suspend*, *halt*, or *destroy* the guest machine. Each of these options have pros and cons. Choose the method that works best for you.

**Suspending** the virtual machine by calling `vagrant suspend` will save the current running state of the machine and stop it. When you are ready to begin working again, just run `vagrant up`, and it will be resumed from where you left off. The main benefit of this method is that it is super fast, usually taking only 5 to 10 seconds to stop and start your work. The downside is that the virtual machine still eats up your disk space, and requires even more disk space to store all the state of the virtual machine RAM on disk.

**Halting** the virtual machine by calling `vagrant halt` will gracefully shut down the guest operating system and power down the guest machine. You can use `vagrant up` when you are ready to boot it again. The benefit of this method is that it will cleanly shut down your machine, preserving the contents of disk, and allowing it to be cleanly started again. The downside is that it'll take some extra time to start from a cold boot, and the guest machine still consumes disk space.

**Destroying** the virtual machine by calling `vagrant destroy` will remove all traces of the guest machine from your system. It'll stop the guest machine, power it down, and remove all of the guest hard disks. Again, when you are ready to work again, just issue a `vagrant up`. The benefit of this is that *no cruft* is left on your machine. The disk space and RAM consumed by the guest machine is reclaimed and your host machine is left clean. The downside is that `vagrant up` to get working again will take some extra time since it has to reimport the machine and re-provision it.

现在，我们有了一个功能齐全的虚拟机，可用于基本的Web开发。 但是现在让我们说是时候换档了，也许是在其他项目上进行工作，也许是去吃午餐，或者只是回家的时间。 我们如何清理开发环境？



使用Vagrant，您可以挂起，停止或破坏客户机。 这些选项各有利弊。 选择最适合您的方法。



通过调用vagrantsuspend挂起虚拟机将保存该计算机的当前运行状态并停止它。 当您准备再次开始工作时，只需运行流浪汉，它将从您上次中断的地方恢复。 这种方法的主要优点是它超级快，通常只需要5到10秒即可停止和开始工作。 缺点是虚拟机仍会占用磁盘空间，甚至需要更多磁盘空间才能将虚拟机RAM的所有状态存储在磁盘上。



通过调用vagrant暂停来暂停虚拟机将正常关闭来宾操作系统并关闭来宾计算机的电源。 当您准备再次启动时，可以使用vagrant up。 这种方法的好处是它将干净地关闭计算机，保留磁盘的内容，并允许它重新干净启动。 缺点是从冷启动开始将花费一些额外的时间，并且来宾计算机仍会占用磁盘空间。



通过调用vagrant destroy破坏虚拟机，将从系统中删除来宾计算机的所有痕迹。 它将停止来宾计算机，将其关闭电源，并卸下所有来宾硬盘。 同样，当您准备好再次工作时，只需发出一个无业游民。 这样做的好处是您的机器上没有残留物。 回收来宾计算机消耗的磁盘空间和RAM，并使主机保持干净。 不利的一面是，无所事事的人要重新开始工作将花费一些额外的时间，因为它必须重新导入机器并重新配置机器。

# Rebuild

JUMP TO SECTION

When you are ready to come back to your project, whether it is tomorrow, a week from now, or a year from now, getting it up and running is easy:

```shell-session
$ vagrant up
```

That's it! Since the Vagrant environment is already all configured via the Vagrantfile, you or any of your coworkers simply have to run `vagrant up` at any time and Vagrant will recreate your work environment.