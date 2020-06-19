# [»](https://www.vagrantup.com/intro/getting-started/providers#providers)Providers

JUMP TO SECTION

In this getting started guide, your project was always backed with [VirtualBox](https://www.virtualbox.org/). But Vagrant can work with a wide variety of backend providers, such as [VMware](https://www.vagrantup.com/docs/vmware/), [Hyper-V](https://www.vagrantup.com/docs/hyperv), and more. Read the page of each provider for more information on how to set them up.

Once you have a provider installed, you do not need to make any modifications to your Vagrantfile, just `vagrant up` with the proper provider and Vagrant will do the rest:

```shell-session
$ vagrant up --provider=vmware_fusion
```

Once you run `vagrant up` with another provider, every other Vagrant command does not need to be told what provider to use. Vagrant can automatically figure it out. So when you are ready to SSH or destroy or anything else, just run the commands like normal, such as `vagrant destroy`. No extra flags necessary.

For more information on providers, read the full documentation on [providers](https://www.vagrantup.com/docs/providers/).

