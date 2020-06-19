# [»](https://www.vagrantup.com/intro/getting-started/networking#networking)Networking

JUMP TO SECTION

At this point we have a web server up and running with the ability to modify files from our host and have them automatically synced to the guest. However, accessing the web pages simply from the terminal from inside the machine is not very satisfying. In this step, we will use Vagrant's *networking* features to give us additional options for accessing the machine from our host machine.

至此，我们已经启动并运行了Web服务器，并能够修改主机中的文件并将其自动同步到来宾。 但是，仅从机器内部从终端访问网页不是很令人满意。 在这一步中，我们将使用Vagrant的网络功能为我们提供从主机访问该计算机的其他选项。

## [»](https://www.vagrantup.com/intro/getting-started/networking#port-forwarding)Port Forwarding

One option is to use *port forwarding*. Port forwarding allows you to specify ports on the guest machine to share via a port on the host machine. This allows you to access a port on your own machine, but actually have all the network traffic forwarded to a specific port on the guest machine.

Let us setup a forwarded port so we can access Apache in our guest. Doing so is a simple edit to the Vagrantfile, which now looks like this:

一种选择是使用端口转发。 端口转发使您可以指定来宾计算机上的端口，以通过主机上的端口进行共享。 这使您可以访问自己计算机上的端口，但实际上会将所有网络流量转发到来宾计算机上的特定端口。



让我们设置一个转发端口，以便我们可以在客户机中访问Apache。 这样做是对Vagrantfile的简单编辑，现在看起来像这样：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```

Run a `vagrant reload` or `vagrant up` (depending on if the machine is already running) so that these changes can take effect.

Once the machine is running again, load `http://127.0.0.1:4567` in your browser. You should see a web page that is being served from the virtual machine that was automatically setup by Vagrant.

运行无用的重新加载或无用的操作（取决于计算机是否已经在运行），以便这些更改可以生效。



机器再次运行后，在浏览器中加载http://127.0.0.1:4567。 您应该看到由Vagrant自动设置的虚拟机提供的网页。

## [»](https://www.vagrantup.com/intro/getting-started/networking#other-networking)Other Networking

Vagrant also has other forms of networking, allowing you to assign a static IP address to the guest machine, or to bridge the guest machine onto an existing network. If you are interested in other options, read the [networking](https://www.vagrantup.com/docs/networking/) page.