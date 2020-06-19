# [»](https://www.vagrantup.com/intro/getting-started/install#install-vagrant)Install Vagrant

JUMP TO SECTION

Vagrant must first be installed on the machine you want to run it on. To make installation easy, Vagrant is distributed as a [binary package](https://www.vagrantup.com/downloads) for all supported platforms and architectures. This page will not cover how to compile Vagrant from source, as that is covered in the [README](https://github.com/hashicorp/vagrant/blob/master/README.md) and is only recommended for advanced users.

## [»](https://www.vagrantup.com/intro/getting-started/install#installing-vagrant)Installing Vagrant

To install Vagrant, first find the [appropriate package](https://www.vagrantup.com/downloads) for your system and download it. Vagrant is packaged as an operating-specific package. Run the installer for your system. The installer will automatically add `vagrant` to your system path so that it is available in terminals. If it is not found, please try logging out and logging back in to your system (this is particularly necessary sometimes for Windows).

## [»](https://www.vagrantup.com/intro/getting-started/install#verifying-the-installation)Verifying the Installation

After installing Vagrant, verify the installation worked by opening a new command prompt or console, and checking that `vagrant` is available:

```shell-session
$ vagrant
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

# ...
```

## [»](https://www.vagrantup.com/intro/getting-started/install#caveats)Caveats

**Beware of system package managers!** Some operating system distributions include a vagrant package in their upstream package repos. Please do not install Vagrant in this manner. Typically these packages are missing dependencies or include very outdated versions of Vagrant. If you install via your system's package manager, it is very likely that you will experience issues. Please use the official installers on the downloads page.

## [»](https://www.vagrantup.com/intro/getting-started/install#next-steps)Next Steps

You have successfully downloaded and installed Vagrant! Read on to learn about [setting up your first Vagrant project](https://www.vagrantup.com/intro/getting-started/project_setup).