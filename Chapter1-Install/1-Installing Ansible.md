# Installing Ansible

This page describes how to install Ansible on different platforms. Ansible is an agentless automation tool that by default manages machines over the SSH protocol. Once installed, Ansible does not add a database, and there will be no daemons to start or keep running. You only need to install it on one machine (which could easily be a laptop) and it can manage an entire fleet of remote machines from that central point. When Ansible manages remote machines, it does not leave software installed or running on them, so there’s no real question about how to upgrade Ansible when moving to a new version.

本页介绍如何在不同平台上安装Ansible。 Ansible是一种无代理程序自动化工具，默认情况下会通过SSH协议管理计算机。 安装后，Ansible不会添加数据库，并且将没有启动或继续运行的守护程序。 您只需要将其安装在一台计算机上（可以很容易地是一台笔记本电脑），它就可以从该中心管理整个远程计算机。 当Ansible管理远程计算机时，它不会在其上安装或运行软件，因此，在迁移到新版本时，如何升级Ansible并没有真正的问题。

##  [Prerequisites](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id9)

You install Ansible on a control node, which then uses SSH (by default) to communicate with your managed nodes (those end devices you want to automate).

您在控制节点上安装Ansible，然后使用SSH（默认情况下）与您的受管节点（您要自动化的那些终端设备）进行通信。

## [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id10)

Currently Ansible can be run from any machine with Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) installed. This includes Red Hat, Debian, CentOS, macOS, any of the BSDs, and so on. Windows is not supported for the control node.

When choosing a control node, bear in mind that any management system benefits from being run near the machines being managed. If you are running Ansible in a cloud, consider running it from a machine inside that cloud. In most cases this will work better than on the open Internet.

当前，Ansible可以从装有Python 2（2.7版）或Python 3（3.5版及更高版本）的任何计算机上运行。 这包括Red Hat，Debian，CentOS，macOS，任何BSD等。 控制节点不支持Windows。



选择控制节点时，请记住，任何管理系统都可从在被管理机器附近运行而受益。 如果您正在云中运行Ansible，请考虑从该云中的计算机上运行它。 在大多数情况下，这将比在开放Internet上更好。

macOS by default is configured for a small number of file handles, so if you want to use 15 or more forks you’ll need to raise the ulimit with `sudo launchctl limit maxfiles unlimited`. This command can also fix any “Too many open files” error.

默认情况下，macOS配置了少量文件句柄，因此，如果要使用15个或更多的派生，则需要使用sudo launchctl limit maxfiles unlimited来提高ulimit。 此命令还可以修复任何“打开文件过多”错误。

**Warning**:

> Please note that some modules and plugins have additional requirements. For modules these need to be satisfied on the ‘target’ machine (the managed node) and should be listed in the module specific docs.

请注意，某些模块和插件还有其他要求。 对于模块，需要在“目标”计算机（受管节点）上满足这些要求，并应在特定于模块的文档中列出。

### [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id11)

On the managed nodes, you need a way to communicate, which is normally SSH. By default this uses SFTP. If that’s not available, you can switch to SCP in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings). You also need Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later).

在受管节点上，您需要一种通信方式，通常是SSH。 默认情况下，它使用SFTP。 如果不可用，则可以在ansible.cfg中切换到SCP。 您还需要Python 2（2.6版或更高版本）或Python 3（3.5版或更高版本）。

Note

- If you have SELinux enabled on remote nodes, you will also want to install libselinux-python on them before using any copy/file/template related functions in Ansible. You can use the [yum module](https://docs.ansible.com/ansible/latest/modules/yum_module.html#yum-module) or [dnf module](https://docs.ansible.com/ansible/latest/modules/dnf_module.html#dnf-module) in Ansible to install this package on remote systems that do not have it.

- By default, Ansible uses the Python interpreter located at `/usr/bin/python` to run its modules. However, some Linux distributions may only have a Python 3 interpreter installed to `/usr/bin/python3` by default. On those systems, you may see an error like:

  ```
  "module_stdout": "/bin/sh: /usr/bin/python: No such file or directory\r\n"
  ```

  you can either set the [ansible_python_interpreter](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#ansible-python-interpreter) inventory variable (see [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory)) to point at your interpreter or you can install a Python 2 interpreter for modules to use. You will still need to set [ansible_python_interpreter](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#ansible-python-interpreter) if the Python 2 interpreter is not installed to **/usr/bin/python**.

- Ansible’s [raw module](https://docs.ansible.com/ansible/latest/modules/raw_module.html#raw-module), and the [script module](https://docs.ansible.com/ansible/latest/modules/script_module.html#script-module), do not depend on a client side install of Python to run. Technically, you can use Ansible to install a compatible version of Python using the [raw module](https://docs.ansible.com/ansible/latest/modules/raw_module.html#raw-module), which then allows you to use everything else. For example, if you need to bootstrap Python 2 onto a RHEL-based system, you can install it as follows:

  ```
  $ ansible myhost --become -m raw -a "yum install -y python2"
  ```

如果在远程节点上启用了SELinux，则还需要在Ansible中使用任何与复制/文件/模板相关的功能之前，在它们上安装libselinux-python。 您可以使用Ansible中的yum模块或dnf模块在没有此软件包的远程系统上安装此软件包。



默认情况下，Ansible使用位于/ usr / bin / python的Python解释器来运行其模块。 但是，默认情况下，某些Linux发行版可能仅将Python 3解释器安装到/ usr / bin / python3。 在这些系统上，您可能会看到类似以下的错误：



“ module_stdout”：“ / bin / sh：/ usr / bin / python：没有这样的文件或目录\ r \ n”



您可以将ansible_python_interpreter清单变量（请参阅如何构建清单）设置为指向您的解释器，或者可以安装Python 2解释器以供模块使用。 如果未将Python 2解释器安装到/ usr / bin / python，则仍然需要设置ansible_python_interpreter。



Ansible的原始模块和脚本模块不依赖于客户端安装的Python来运行。 从技术上讲，您可以使用Ansible使用raw模块安装兼容版本的Python，然后使用该模块使用其他所有模块。 例如，如果需要将Python 2引导到基于RHEL的系统上，则可以按以下方式安装它：



$ ansible myhost --become -m raw -a“ yum install -y python2”

## [Selecting an Ansible version to install](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id12)

Which Ansible version to install is based on your particular needs. You can choose any of the following ways to install Ansible:

- Install the latest release with your OS package manager (for Red Hat Enterprise Linux (TM), CentOS, Fedora, Debian, or Ubuntu).

- Install with `pip` (the Python package manager).

- Install from source to access the development (`devel`) version to develop or test the latest features.

  选择要安装的Ansible版本

  

  根据您的特定需要安装哪个Ansible版本。 您可以选择以下任何一种方式来安装Ansible：

  

  使用OS软件包管理器安装最新版本（用于Red Hat Enterprise Linux（TM），CentOS，Fedora，Debian或Ubuntu）。

  

  使用pip（Python软件包管理器）进行安装。

  

  从源代码安装以访问开发（开发）版本以开发或测试最新功能。

Note

You should only run Ansible from `devel` if you are actively developing content for Ansible. This is a rapidly changing source of code and can become unstable at any point.

如果您正在积极地为Ansible开发内容，则只能从开发运行Ansible。 这是一个快速变化的代码源，并且随时可能变得不稳定。

Ansible creates new releases two to three times a year. Due to this short release cycle, minor bugs will generally be fixed in the next release versus maintaining backports on the stable branch. Major bugs will still have maintenance releases when needed, though these are infrequent.

Ansible一年两次创建新版本两次。 由于发行周期短，与在稳定分支上保持反向移植相比，通常会在下一发行版中修复一些小错误。 重大错误仍会在需要时发布维护版本，尽管这种情况很少见

## [Installing Ansible on RHEL, CentOS, or Fedora](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id13)

On Fedora:

```
$ sudo dnf install ansible
```

On RHEL and CentOS:

```
$ sudo yum install ansible
```

RPMs for RHEL 7 and RHEL 8 are available from the [Ansible Engine repository](https://access.redhat.com/articles/3174981).

To enable the Ansible Engine repository for RHEL 8, run the following command:

```
$ sudo subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms
```

To enable the Ansible Engine repository for RHEL 7, run the following command:

```
$ sudo subscription-manager repos --enable rhel-7-server-ansible-2.9-rpms
```

RPMs for currently supported versions of RHEL, CentOS, and Fedora are available from [EPEL](https://fedoraproject.org/wiki/EPEL) (Extra Packages for Enterprise Linux)as well as [releases.ansible.com](https://releases.ansible.com/ansible/rpm).可以下载rpm包

Ansible version 2.4 and later can manage earlier operating systems that contain Python 2.6 or higher.

Ansible 2.4和更高版本可以管理包含Python 2.6或更高版本的早期操作系统。

You can also build an RPM yourself. From the root of a checkout or tarball, use the `make rpm` command to build an RPM you can distribute and install.

您也可以自己构建RPM。 从结帐或tarball的根开始，使用make rpm命令来构建可以分发和安装的RPM。

```
$ git clone https://github.com/ansible/ansible.git
$ cd ./ansible
$ make rpm
$ sudo rpm -Uvh ./rpm-build/ansible-*.noarch.rpm
```



## [Installing Ansible on Ubuntu](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id14)

Ubuntu builds are available [in a PPA here](https://launchpad.net/~ansible/+archive/ubuntu/ansible).

To configure the PPA on your machine and install Ansible run these commands:

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

Note

On older Ubuntu distributions, “software-properties-common” is called “python-software-properties”. You may want to use `apt-get` instead of `apt` in older versions. Also, be aware that only newer distributions (i.e. 18.04, 18.10, etc.) have a `-u` or `--update` flag, so adjust your script accordingly.

在较旧的Ubuntu发行版中，“ software-properties-common”被称为“ python-software-properties”。 您可能要使用apt-get而不是旧版本中的apt。 另外，请注意，只有较新的发行版（即18.04、18.10等）才带有-u或--update标志，因此请相应地调整脚本。

Debian/Ubuntu packages can also be built from the source checkout, run:

```
$ make deb
```

You may also wish to run from source to get the development branch, which is covered below.

## [Installing Ansible with `pip`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id23)

Ansible can be installed with `pip`, the Python package manager. If `pip` isn’t already available on your system of Python, run the following commands to install it:

Ansible可以与Python软件包管理器pip一起安装。 如果您的Python系统上尚未提供pip，请运行以下命令进行安装：

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py --user
```

pip安装教程：https://pip.pypa.io/en/stable/installing/#upgrading-pip

Then install Ansible [[1\]](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id7):

安装ansible

```
$ pip install --user ansible
```

Or if you are looking for the development version:

安装开发者版本的ansible

```
$ pip install --user git+https://github.com/ansible/ansible.git@devel
```

If you are installing on macOS Mavericks (10.9), you may encounter some noise from your compiler. A workaround is to do the following:

如果在macOS上安装，有可能遇到编译错误。可以使用下面命令解决。

```
$ CFLAGS=-Qunused-arguments CPPFLAGS=-Qunused-arguments pip install --user ansible
```

In order to use the `paramiko` connection plugin or modules that require `paramiko`, install the required module [[2\]](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id8):

paramiko是用于建立SSH2连接（客户端或服务器）的库。 重点是使用SSH2作为SSL的替代方法，以在python脚本之间建立安全连接。 支持所有主要密码和哈希方法。 也支持SFTP客户端和服务器模式。

为了能够使用paramiko提供的安全功能，安装依赖包：

```
$ pip install --user paramiko
```

Ansible can also be installed inside a new or existing `virtualenv`:

ansible也可以在虚拟环境中安装

```
$ python -m virtualenv ansible  # Create a virtualenv if one does not already exist
$ source ansible/bin/activate   # Activate the virtual environment
$ pip install ansible
```

If you wish to install Ansible globally, run the following commands:

如果想系统使用ansible，运行下面命令：

```
$ sudo python get-pip.py
$ sudo pip install ansible
```

Note

Running `pip` with `sudo` will make global changes to the system. Since `pip` does not coordinate with system package managers, it could make changes to your system that leaves it in an inconsistent or non-functioning state. This is particularly true for macOS. Installing with `--user` is recommended unless you understand fully the implications of modifying global files on the system.

使用sudo运行pip将对系统进行全局更改。 由于pip无法与系统程序包管理器协调，因此可能会对系统进行更改，使系统处于不一致或无法运行的状态。 对于macOS尤其如此。 除非您完全了解在系统上修改全局文件的含义，否则建议使用--user进行安装。

Note

Older versions of `pip` default to http://pypi.python.org/simple, which no longer works. Please make sure you have the latest version of `pip` before installing Ansible. If you have an older version of `pip` installed, you can upgrade by following [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installing/#upgrading-pip) .

升级pip到最新版本

```
pip install -U pip
或者
python -m pip install -U pip
```

## [Finding tarballs of tagged releases](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id25)

Packaging Ansible or wanting to build a local package yourself, but don’t want to do a git checkout? Tarballs of releases are available on the [Ansible downloads](https://releases.ansible.com/ansible) page.

These releases are also tagged in the [git repository](https://github.com/ansible/ansible/releases) with the release version.

打包Ansible还是想自己构建本地包，但不想进行git checkout？ 可在Ansible下载页面上找到发行包。



这些发行版还在git存储库中用发行版标记



最新tarball：https://releases.ansible.com/ansible/ansible-latest.tar.gz



为了ansible正常运行，至少需要以下三个包：

jinja2
PyYAML
cryptography



cd ansible-$version

pip install -r requirements.txt

编译安装：

python setup.py build

python setup.py install

## [Ansible command shell completion](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id26)

As of Ansible 2.9, shell completion of the Ansible command line utilities is available and provided through an optional dependency called `argcomplete`. `argcomplete` supports bash, and has limited support for zsh and tcsh.

You can install `python-argcomplete` from EPEL on Red Hat Enterprise based distributions, and or from the standard OS repositories for many other distributions.

For more information about installing and configuration see the [argcomplete documentation](https://argcomplete.readthedocs.io/en/latest/).

从Ansible 2.9开始，Ansible命令行实用程序的Shell补全可用，并通过称为argcomplete的可选依赖项提供。 argcomplete支持bash，并且对zsh和tcsh的支持有限。



您可以在基于Red Hat Enterprise的发行版上从EPEL安装python-argcomplete，或者从许多其他发行版的标准OS存储库中安装python-argcomplete。



有关安装和配置的更多信息，请参见argcomplete文档。

### [Installing `argcomplete` on RHEL, CentOS, or Fedora](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id27)

On Fedora:

```
$ sudo dnf install python-argcomplete
```

On RHEL and CentOS:

```
$ sudo yum install epel-release
$ sudo yum install python-argcomplete
```

### [Installing `argcomplete` with `apt`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id28)

```
$ sudo apt install python-argcomplete
```

### [Installing `argcomplete` with `pip`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id29)

```
$ pip install argcomplete
```

### [Configuring `argcomplete`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id30)

There are 2 ways to configure `argcomplete` to allow shell completion of the Ansible command line utilities: globally or per command.

有两种方法可以配置argcomplete以允许完成Ansible命令行实用程序的shell：全局或每个命令。

#### [Globally](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id31)

Global completion requires bash 4.2.

```
$ sudo activate-global-python-argcomplete
bash --version
```

This will write a bash completion file to a global location. Use `--dest` to change the location.

```
[root@do1harbor ansible-2.9.9]# sudo activate-global-python-argcomplete
Installing bash completion script /etc/bash_completion.d/python-argcomplete
```

#### [Per command](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id32)

If you do not have bash 4.2, you must register each script independently.

如果bash版本小于4.2，需要为每一个ansible命令设置自动补全。

```
$ eval $(register-python-argcomplete ansible)
$ eval $(register-python-argcomplete ansible-config)
$ eval $(register-python-argcomplete ansible-console)
$ eval $(register-python-argcomplete ansible-doc)
$ eval $(register-python-argcomplete ansible-galaxy)
$ eval $(register-python-argcomplete ansible-inventory)
$ eval $(register-python-argcomplete ansible-playbook)
$ eval $(register-python-argcomplete ansible-pull)
$ eval $(register-python-argcomplete ansible-vault)
```

You should place the above commands into your shells profile file such as `~/.profile` or `~/.bash_profile`.
