# [»](https://www.vagrantup.com/intro/getting-started/boxes#boxes)Boxes

JUMP TO SECTION

Instead of building a virtual machine from scratch, which would be a slow and tedious process, Vagrant uses a base image to quickly clone a virtual machine. These base images are known as "boxes" in Vagrant, and specifying the box to use for your Vagrant environment is always the first step after creating a new Vagrantfile.

Vagrant不会从头开始构建虚拟机（这是一个缓慢而乏味的过程），而是使用基础映像来快速克隆虚拟机。 这些基础映像在Vagrant中被称为“框”，在创建新的Vagrantfile之后，指定用于Vagrant环境的框始终是第一步。

## [»](https://www.vagrantup.com/intro/getting-started/boxes#installing-a-box)Installing a Box

If you ran the commands on the [getting started overview page](https://www.vagrantup.com/intro/getting-started/), then you've already installed a box before, and you do not need to run the commands below again. However, it is still worth reading this section to learn more about how boxes are managed.

Boxes are added to Vagrant with `vagrant box add`. This stores the box under a specific name so that multiple Vagrant environments can re-use it. If you have not added a box yet, you can do so now:

如果您在“入门概述”页面上运行了这些命令，则说明您之前已经安装了一个框，因此无需再次运行以下命令。 但是，仍然值得阅读本节以了解有关如何管理包装箱的更多信息。



通过添加无用的盒子将盒子添加到无用的盒子。 这会以特定名称存储该框，以便多个Vagrant环境可以重复使用它。 如果尚未添加框，则可以立即添加：

```shell-session
$ vagrant box add hashicorp/bionic64
```

This will download the box named "hashicorp/bionic64" from [HashiCorp's Vagrant Cloud box catalog](https://vagrantcloud.com/boxes/search), a place where you can find and host boxes. While it is easiest to download boxes from HashiCorp's Vagrant Cloud you can also add boxes from a local file, custom URL, etc.

Boxes are globally stored for the current user. Each project uses a box as an initial image to clone from, and never modifies the actual base image. This means that if you have two projects both using the `hashicorp/bionic64` box we just added, adding files in one guest machine will have no effect on the other machine.

In the above command, you will notice that boxes are namespaced. Boxes are broken down into two parts - the username and the box name - separated by a slash. In the example above, the username is "hashicorp", and the box is "bionic64". You can also specify boxes via URLs or local file paths, but that will not be covered in the getting started guide.

这将从HashiCorp的Vagrant Cloud盒子目录中下载名为“ hashicorp / bionic64”的盒子，您可以在其中找到并存放盒子。 从HashiCorp的Vagrant Cloud下载盒子是最简单的，但您也可以从本地文件，自定义URL等添加盒子。



框是为当前用户全局存储的。 每个项目都使用一个框作为要克隆的初始映像，并且从不修改实际的基础映像。 这意味着如果您有两个项目都使用我们刚刚添加的hashicorp / bionic64框，则在一台客户机中添加文件不会对另一台计算机产生影响。



在上面的命令中，您将注意到框已命名。 框分为两部分-用户名和框名-用斜杠分隔。 在上面的示例中，用户名是“ hashicorp”，框是“ bionic64”。 您也可以通过URL或本地文件路径指定框，但是入门指南中不会涉及。

**Namespaces do not guarantee canonical boxes!** A common misconception is that a namespace like "ubuntu" represents the canonical space for Ubuntu boxes. This is untrue. Namespaces on Vagrant Cloud behave very similarly to namespaces on GitHub, for example. Just as GitHub's support team is unable to assist with issues in someone's repository, HashiCorp's support team is unable to assist with third-party published boxes.

命名空间不保证规范框！ 一个常见的误解是，像“ ubuntu”这样的名称空间代表了Ubuntu盒子的规范空间。 这是不正确的。 例如，Vagrant Cloud上的命名空间的行为与GitHub上的命名空间非常相似。 就像GitHub的支持团队无法协助某人的存储库中的问题一样，HashiCorp的支持团队也无法协助第三方发布的盒子。

## [»](https://www.vagrantup.com/intro/getting-started/boxes#using-a-box)Using a Box

Now that the box has been added to Vagrant, we need to configure our project to use it as a base. Open the `Vagrantfile` and change the contents to the following:

现在，该框已添加到Vagrant中，我们需要配置项目以将其用作基础。 打开Vagrantfile并将内容更改为以下内容：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
end
```

The "hashicorp/bionic64" in this case must match the name you used to add the box above. This is how Vagrant knows what box to use. If the box was not added before, Vagrant will automatically download and add the box when it is run.

You may specify an explicit version of a box by specifying `config.vm.box_version` for example:

在这种情况下，“ hashicorp / bionic64”必须与您用来在上方添加框的名称匹配。 这就是流浪者知道使用哪个盒子的方式。 如果之前未添加该框，则Vagrant将在运行时自动下载并添加该框。



您可以通过指定config.vm.box_version来指定框的显式版本，例如：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_version = "1.1.0"
end
```

You may also specify the URL to a box directly using `config.vm.box_url`:

您也可以直接使用config.vm.box_url指定框的URL：

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_url = "https://vagrantcloud.com/hashicorp/bionic64"
end
```

In the next section, we will bring up the Vagrant environment and interact with it a little bit.

在下一节中，我们将介绍Vagrant环境并与其进行一些交互。

## [»](https://www.vagrantup.com/intro/getting-started/boxes#finding-more-boxes)Finding More Boxes

For the remainder of this getting started guide, we will only use the "hashicorp/bionic64" box we added previously. But soon after finishing this getting started guide, the first question you will probably have is "where do I find more boxes?"

The best place to find more boxes is [HashiCorp's Vagrant Cloud box catalog](https://vagrantcloud.com/boxes/search). HashiCorp's Vagrant Cloud has a public directory of freely available boxes that run various platforms and technologies. HashiCorp's Vagrant Cloud also has a great search feature to allow you to find the box you care about.

In addition to finding free boxes, HashiCorp's Vagrant Cloud lets you host your own boxes, as well as private boxes if you intend on creating boxes for your own organization.

在本入门指南的其余部分中，我们将仅使用之前添加的“ hashicorp / bionic64”框。 但是在完成本入门指南后不久，您可能会遇到的第一个问题是“我在哪里可以找到更多盒子？”



查找更多包装盒的最佳地点是HashiCorp的Vagrant Cloud包装盒目录。 HashiCorp的Vagrant Cloud具有运行各种平台和技术的免费可用盒子的公共目录。 HashiCorp的Vagrant Cloud还具有出色的搜索功能，可让您找到所需的盒子。



除了查找免费的包装盒之外，HashiCorp的Vagrant Cloud还允许您托管自己的包装盒，如果打算为自己的组织创建包装盒，还可以托管私人包装盒。