# Installing Ansible

This page describes how to install Ansible on different platforms. Ansible is an agentless automation tool that by default manages machines over the SSH protocol. Once installed, Ansible does not add a database, and there will be no daemons to start or keep running. You only need to install it on one machine (which could easily be a laptop) and it can manage an entire fleet of remote machines from that central point. When Ansible manages remote machines, it does not leave software installed or running on them, so there’s no real question about how to upgrade Ansible when moving to a new version.

##  [Prerequisites](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id9)

You install Ansible on a control node, which then uses SSH (by default) to communicate with your managed nodes (those end devices you want to automate).



## [Control node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id10)

Currently Ansible can be run from any machine with Python 2 (version 2.7) or Python 3 (versions 3.5 and higher) installed. This includes Red Hat, Debian, CentOS, macOS, any of the BSDs, and so on. Windows is not supported for the control node.

When choosing a control node, bear in mind that any management system benefits from being run near the machines being managed. If you are running Ansible in a cloud, consider running it from a machine inside that cloud. In most cases this will work better than on the open Internet.

**Warning**:

> Please note that some modules and plugins have additional requirements. For modules these need to be satisfied on the ‘target’ machine (the managed node) and should be listed in the module specific docs.



### [Managed node requirements](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id11)

On the managed nodes, you need a way to communicate, which is normally SSH. By default this uses SFTP. If that’s not available, you can switch to SCP in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings). You also need Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later).

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



## [Selecting an Ansible version to install](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id12)

Which Ansible version to install is based on your particular needs. You can choose any of the following ways to install Ansible:

- Install the latest release with your OS package manager (for Red Hat Enterprise Linux (TM), CentOS, Fedora, Debian, or Ubuntu).
- Install with `pip` (the Python package manager).
- Install from source to access the development (`devel`) version to develop or test the latest features.

Note

You should only run Ansible from `devel` if you are actively developing content for Ansible. This is a rapidly changing source of code and can become unstable at any point.

Ansible creates new releases two to three times a year. Due to this short release cycle, minor bugs will generally be fixed in the next release versus maintaining backports on the stable branch. Major bugs will still have maintenance releases when needed, though these are infrequent.

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

RPMs for currently supported versions of RHEL, CentOS, and Fedora are available from [EPEL](https://fedoraproject.org/wiki/EPEL) as well as [releases.ansible.com](https://releases.ansible.com/ansible/rpm).

Ansible version 2.4 and later can manage earlier operating systems that contain Python 2.6 or higher.

You can also build an RPM yourself. From the root of a checkout or tarball, use the `make rpm` command to build an RPM you can distribute and install.

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

Debian/Ubuntu packages can also be built from the source checkout, run:

```
$ make deb
```

You may also wish to run from source to get the development branch, which is covered below.

## [Installing Ansible with `pip`](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id23)

Ansible can be installed with `pip`, the Python package manager. If `pip` isn’t already available on your system of Python, run the following commands to install it:

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py --user
```

Then install Ansible [[1\]](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id7):

```
$ pip install --user ansible
```

Or if you are looking for the development version:

```
$ pip install --user git+https://github.com/ansible/ansible.git@devel
```

If you are installing on macOS Mavericks (10.9), you may encounter some noise from your compiler. A workaround is to do the following:

```
$ CFLAGS=-Qunused-arguments CPPFLAGS=-Qunused-arguments pip install --user ansible
```

In order to use the `paramiko` connection plugin or modules that require `paramiko`, install the required module [[2\]](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id8):

```
$ pip install --user paramiko
```

Ansible can also be installed inside a new or existing `virtualenv`:

```
$ python -m virtualenv ansible  # Create a virtualenv if one does not already exist
$ source ansible/bin/activate   # Activate the virtual environment
$ pip install ansible
```

If you wish to install Ansible globally, run the following commands:

```
$ sudo python get-pip.py
$ sudo pip install ansible
```

Note

Running `pip` with `sudo` will make global changes to the system. Since `pip` does not coordinate with system package managers, it could make changes to your system that leaves it in an inconsistent or non-functioning state. This is particularly true for macOS. Installing with `--user` is recommended unless you understand fully the implications of modifying global files on the system.

Note

Older versions of `pip` default to http://pypi.python.org/simple, which no longer works. Please make sure you have the latest version of `pip` before installing Ansible. If you have an older version of `pip` installed, you can upgrade by following [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installing/#upgrading-pip) .



## [Running Ansible from source (devel)](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id24)

Note

You should only run Ansible from `devel` if you are actively developing content for Ansible. This is a rapidly changing source of code and can become unstable at any point.

Ansible is easy to run from source. You do not need `root` permissions to use it and there is no software to actually install. No daemons or database setup are required.

Note

If you want to use Ansible Tower as the control node, do not use a source installation of Ansible. Please use an OS package manager (like `apt` or `yum`) or `pip` to install a stable version.

To install from source, clone the Ansible git repository:

```
$ git clone https://github.com/ansible/ansible.git
$ cd ./ansible
```

Once `git` has cloned the Ansible repository, setup the Ansible environment:

Using Bash:

```
$ source ./hacking/env-setup
```

Using Fish:

```
$ source ./hacking/env-setup.fish
```

If you want to suppress spurious warnings/errors, use:

```
$ source ./hacking/env-setup -q
```

If you don’t have `pip` installed in your version of Python, install it:

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py --user
```

Ansible also uses the following Python modules that need to be installed [[1\]](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id7):

```
$ pip install --user -r ./requirements.txt
```

To update Ansible checkouts, use pull-with-rebase so any local changes are replayed.

```
$ git pull --rebase
$ git pull --rebase #same as above
$ git submodule update --init --recursive
```

Once running the env-setup script you’ll be running from checkout and the default inventory file will be `/etc/ansible/hosts`. You can optionally specify an inventory file (see [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory)) other than `/etc/ansible/hosts`:

```
$ echo "127.0.0.1" > ~/ansible_hosts
$ export ANSIBLE_INVENTORY=~/ansible_hosts
```

You can read more about the inventory file at [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory).

Now let’s test things with a ping command:

```
$ ansible all -m ping --ask-pass
```

You can also use “sudo make install”.



## [Finding tarballs of tagged releases](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id25)

Packaging Ansible or wanting to build a local package yourself, but don’t want to do a git checkout? Tarballs of releases are available on the [Ansible downloads](https://releases.ansible.com/ansible) page.

These releases are also tagged in the [git repository](https://github.com/ansible/ansible/releases) with the release version.

## [Ansible command shell completion](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id26)

As of Ansible 2.9, shell completion of the Ansible command line utilities is available and provided through an optional dependency called `argcomplete`. `argcomplete` supports bash, and has limited support for zsh and tcsh.

You can install `python-argcomplete` from EPEL on Red Hat Enterprise based distributions, and or from the standard OS repositories for many other distributions.

For more information about installing and configuration see the [argcomplete documentation](https://argcomplete.readthedocs.io/en/latest/).

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

#### [Globally](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id31)

Global completion requires bash 4.2.

```
$ sudo activate-global-python-argcomplete
```

This will write a bash completion file to a global location. Use `--dest` to change the location.

#### [Per command](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id32)

If you do not have bash 4.2, you must register each script independently.

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

### [`argcomplete` with zsh or tcsh](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id33)

See the [argcomplete documentation](https://argcomplete.readthedocs.io/en/latest/).