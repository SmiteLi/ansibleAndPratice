# Share

JUMP TO SECTION

Now that we have a web server up and running and accessible from your machine, we have a fairly functional development environment. But in addition to providing a development environment, Vagrant also makes it easy to share and collaborate on these environments. The primary feature to do this in Vagrant is called [Vagrant Share](https://www.vagrantup.com/docs/share/).

Vagrant Share lets you share your Vagrant environment to anyone around the world with an Internet connection. It will give you a URL that will route directly to your Vagrant environment from any device in the world that is connected to the Internet.

First, follow the [installation guide](https://www.vagrantup.com/docs/share/#installation) before getting started. You need the `vagrant-share` plugin for the rest of the tutorial to work.

Next, run `vagrant share`:

既然我们已经启动并运行了Web服务器，并且可以从您的机器上访问它，那么我们已经有了一个功能正常的开发环境。 但是，除了提供开发环境外，Vagrant还使在这些环境上共享和协作变得容易。 在Vagrant中执行此操作的主要功能称为Vagrant Share。



Vagrant Share可让您通过Internet连接与世界各地的任何人共享您的Vagrant环境。 它会为您提供一个URL，该URL可从世界上连接到Internet的任何设备直接路由到您的Vagrant环境。



首先，请先按照安装指南进行操作。 您需要使用vagrant-share插件才能使本教程的其余部分正常工作。



接下来，运行无业游民份额：

```shell-session
$ vagrant share
...
==> default: Creating Vagrant Share session...
==> default: HTTP URL: http://b1fb1f3f.ngrok.io
...
```

Your URL will be different, so do not try the URL above. Instead, copy the URL that `vagrant share` outputted for you and visit that in a web browser. It should load the Apache page we setup earlier.

If you modify the files in your shared folder and refresh the URL, you will see it update! The URL is routing directly into your Vagrant environment, and works from any device in the world that is connected to the internet.

To end the sharing session, hit `Ctrl+C` in your terminal. You can refresh the URL again to verify that your environment is no longer being shared.

Vagrant Share is much more powerful than simply HTTP sharing. For more details, see the [complete Vagrant Share documentation](https://www.vagrantup.com/docs/share/).

Note: Vagrant Share now defaults to using the `ngrok` driver. The `classic` driver has been deprecated.

您的URL将有所不同，因此请勿尝试上面的URL。 而是复制流浪者共享为您输出的URL，然后在Web浏览器中访问该URL。 它应该加载我们之前设置的Apache页面。



如果您修改共享文件夹中的文件并刷新URL，您将看到它已更新！ 该URL直接路由到您的Vagrant环境中，并且可以在世界上连接到Internet的任何设备上工作。



要结束共享会话，请在终端中按Ctrl + C。 您可以再次刷新URL，以确认不再共享您的环境。



Vagrant Share比简单的HTTP共享功能强大得多。 有关更多详细信息，请参见完整的Vagrant Share文档。



注意：Vagrant Share现在默认为使用ngrok驱动程序。 经典驱动程序已被弃用。

**Vagrant share is not designed to serve production traffic!** Please do not rely on Vagrant share outside of development or Q/A. The Vagrant share service is not designed to carry production-level traffic.

无业游民份额并非旨在服务于生产流量！ 请不要在开发或Q / A之外依赖Vagrant的份额。 Vagrant共享服务并非旨在承载生产级别的流量。