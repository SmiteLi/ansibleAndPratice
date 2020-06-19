# Introduction to Vagrant

Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the "works on my machine" excuse a relic of the past.

If you are already familiar with the basics of Vagrant, the [documentation](https://www.vagrantup.com/docs) provides a better reference build for all available features and internals.

Vagrant是一款软件，用于构建和管理虚拟机环境。 它简单易用，专注于自动化，缩短了开发环境的配置时间，并使“在我的机器上工作”成为过去。

## [»](https://www.vagrantup.com/intro#why-vagrant)Why Vagrant?

为什么选择Vagrant？

Vagrant provides easy to configure, reproducible, and portable work environments built on top of industry-standard technology and controlled by a single consistent workflow to help maximize the productivity and flexibility of you and your team.

To achieve its magic, Vagrant stands on the shoulders of giants. Machines are provisioned on top of VirtualBox, VMware, AWS, or [any other provider](https://www.vagrantup.com/docs/providers/). Then, industry-standard [provisioning tools](https://www.vagrantup.com/docs/provisioning/) such as shell scripts, Chef, or Puppet, can automatically install and configure software on the virtual machine.

Vagrant提供了易于配置，可重现的便携式工作环境，这些环境建立在行业标准技术之上，并由一个一致的工作流程控制，有助于最大程度地提高您和您的团队的生产力和灵活性。

为了实现这些能力，Vagrant站在巨人的肩膀上。 在VirtualBox，VMware，AWS或任何其他提供商的基础上配置了虚拟机。 然后，行业标准的配置工具（例如Shell脚本，Chef或Puppet）可以在虚拟机上自动安装和配置软件。

### [»](https://www.vagrantup.com/intro#for-developers)For Developers

对于开发者

If you are a **developer**, Vagrant will isolate dependencies and their configuration within a single disposable, consistent environment, without sacrificing any of the tools you are used to working with (editors, browsers, debuggers, etc.). Once you or someone else creates a single [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/), you just need to `vagrant up` and everything is installed and configured for you to work. Other members of your team create their development environments from the same configuration, so whether you are working on Linux, Mac OS X, or Windows, all your team members are running code in the same environment, against the same dependencies, all configured the same way. Say goodbye to "works on my machine" bugs.

如果您是开发人员，Vagrant会在单个一次性的一致环境中隔离依赖关系及其配置，而不会牺牲您使用的任何工具（编辑器，浏览器，调试器等）。 一旦您或其他人创建了一个Vagrantfile，您只需流浪汉，一切都已安装并配置为您可以使用。 团队的其他成员使用相同的配置创建他们的开发环境，因此，无论您是在Linux，Mac OS X还是Windows上工作，所有团队成员都在相同的环境中针对相同的依赖项运行代码，并且所有配置都相同 道路。 与“在我的机器上工作”的错误说再见。

### [»](https://www.vagrantup.com/intro#for-operators)For Operators

If you are an **operations engineer** or **DevOps engineer**, Vagrant gives you a disposable environment and consistent workflow for developing and testing infrastructure management scripts. You can quickly test things like shell scripts, Chef cookbooks, Puppet modules, and more using local virtualization such as VirtualBox or VMware. Then, with the *same configuration*, you can test these scripts on remote clouds such as AWS or RackSpace with the *same workflow*. Ditch your custom scripts to recycle EC2 instances, stop juggling SSH prompts to various machines, and start using Vagrant to bring sanity to your life.

如果您是运营工程师或DevOps工程师，Vagrant会为您提供一个可用于开发和测试基础架构管理脚本的一次性环境和一致的工作流程。 您可以使用本地虚拟化（例如VirtualBox或VMware）快速测试诸如Shell脚本，Chef Cookbook，Puppet模块之类的内容，以及其他功能。 然后，使用相同的配置，您可以在具有相同工作流程的远程云（例如AWS或RackSpace）上测试这些脚本。 放弃您的自定义脚本以回收EC2实例，停止将SSH提示摆弄到各种计算机上，并开始使用Vagrant使您的生活变得井井有条。

### [»](https://www.vagrantup.com/intro#for-designers)For Designers

If you are a **designer**, Vagrant will automatically set everything up that is required for that web app in order for you to focus on doing what you do best: design. Once a developer configures Vagrant, you do not need to worry about how to get that app running ever again. No more bothering other developers to help you fix your environment so you can test designs. Just check out the code, `vagrant up`, and start designing.

如果您是设计师，Vagrant会自动设置该Web应用程序所需的所有内容，以便您专注于做自己擅长的事情：设计。 开发人员配置Vagrant后，您无需担心如何再次运行该应用程序。 无需再打扰其他开发人员来帮助您修复环境，从而可以测试设计。 只需签出代码，仔细检查，然后开始设计。

### [»](https://www.vagrantup.com/intro#for-everyone)For Everyone

Vagrant is designed for everyone as the easiest and fastest way to create a virtualized environment!

Vagrant为每个人而设计，是创建虚拟化环境的最简单，最快的方法！