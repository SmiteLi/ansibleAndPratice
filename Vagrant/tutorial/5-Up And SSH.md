# [»](https://www.vagrantup.com/intro/getting-started/up#up-and-ssh)Up And SSH

JUMP TO SECTION

It is time to boot your first Vagrant environment. Run the following from your terminal:

现在是时候启动您的第一个Vagrant环境了。 从终端运行以下命令：

```shell-session
$ vagrant up
```

In less than a minute, this command will finish and you will have a virtual machine running Ubuntu. You will not actually *see* anything though, since Vagrant runs the virtual machine without a UI. To prove that it is running, you can SSH into the machine:

在不到一分钟的时间内，此命令将完成，您将拥有运行Ubuntu的虚拟机。 但是，您实际上将看不到任何东西，因为Vagrant在没有UI的情况下运行虚拟机。 为了证明它正在运行，您可以SSH进入机器：

```shell-session
$ vagrant ssh
```

This command will drop you into a full-fledged SSH session. Go ahead and interact with the machine and do whatever you want. Although it may be tempting, be careful about `rm -rf /`, since Vagrant shares a directory at `/vagrant` with the directory on the host containing your Vagrantfile, and this can delete all those files. Shared folders will be covered in the next section.

Take a moment to think what just happened: With just one line of configuration and one command in your terminal, we brought up a fully functional, SSH accessible virtual machine. Cool. The SSH session can be terminated with `CTRL+D`.

此命令将使您进入完整的SSH会话。 继续与机器交互，并做您想做的任何事情。 尽管可能很诱人，但请注意rm -rf /，因为Vagrant与包含您的Vagrantfile的主机上的目录在/ vagrant共享一个目录，这可以删除所有这些文件。 共享文件夹将在下一部分中介绍。



花点时间思考一下发生了什么：只需在终端中进行一行配置和一条命令，我们就可以启动功能齐全的SSH可访问虚拟机。 凉。 可以使用CTRL + D终止SSH会话。

```shell-session
vagrant@bionic64:~$ logout
Connection to 127.0.0.1 closed.
```

When you are done fiddling around with the machine, run `vagrant destroy` back on your host machine, and Vagrant will terminate the use of any resources by the virtual machine.

当您摆弄机器时，请在主机上重新运行vagrant destroy，Vagrant将终止虚拟机对任何资源的使用。

The `vagrant destroy` command does not actually remove the downloaded box file. To *completely* remove the box file, you can use the `vagrant box remove` command.

vagrant destroy命令实际上并不会删除下载的文件。 要完全删除Box文件，可以使用`vagrant box remove`命令。

