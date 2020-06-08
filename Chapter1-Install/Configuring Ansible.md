# [Configuring Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#id3)

Topics

- Configuring Ansible
  - Configuration file
    - [Getting the latest configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#getting-the-latest-configuration)
  - [Environmental configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#environmental-configuration)
  - [Command line options](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#command-line-options)

This topic describes how to control Ansible settings.



## [Configuration file](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#id4)

Certain settings in Ansible are adjustable via a configuration file (ansible.cfg). The stock configuration should be sufficient for most users, but there may be reasons you would want to change them. Paths where configuration file is searched are listed in [reference documentation](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings-locations).

Ansible的某些设置可以通过配置文件(ansibl .cfg)进行调整。库存配置对于大多数用户来说应该足够了，但是可能有一些原因需要更改它们。在参考文档中列出了搜索配置文件的路径

### [Getting the latest configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#id5)

If installing Ansible from a package manager, the latest `ansible.cfg` file should be present in `/etc/ansible`, possibly as a `.rpmnew` file (or other) as appropriate in the case of updates.

If you installed Ansible from pip or from source, you may want to create this file in order to override default settings in Ansible.

An [example file is available on GitHub](https://github.com/ansible/ansible/blob/devel/examples/ansible.cfg).

For more details and a full listing of available configurations go to [configuration_settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings). Starting with Ansible version 2.4, you can use the [ansible-config](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#ansible-config) command line utility to list your available options and inspect the current values.

For in-depth details, see [Ansible Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).

如果从程序包管理器安装Ansible，则最新的ansible.cfg文件应存在于/ etc / ansible中，并可能在更新时作为.rpmnew文件（或其他）存在。



如果从pip或从源安装了Ansible，则可能要创建此文件以覆盖Ansible中的默认设置。



GitHub上有一个示例文件。



有关更多详细信息和可用配置的完整列表，请转到configuration_settings。 从Ansible 2.4版开始，您可以使用ansible-config命令行实用程序列出可用选项并检查当前值。



有关详细信息，请参阅Ansible配置设置。

## [Environmental configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#id6)

Ansible also allows configuration of settings using environment variables. If these environment variables are set, they will override any setting loaded from the configuration file.

You can get a full listing of available environment variables from [Ansible Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).

Ansible还允许使用环境变量配置设置。 如果设置了这些环境变量，它们将覆盖从配置文件加载的所有设置。



您可以从Ansible Configuration Settings中获得可用环境变量的完整列表。

## [Command line options](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#id7)

Not all configuration options are present in the command line, just the ones deemed most useful or common. Settings in the command line will override those passed through the configuration file and the environment.

The full list of options available is in [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#ansible-playbook) and [ansible](https://docs.ansible.com/ansible/latest/cli/ansible.html#ansible).

并非命令行中存在所有配置选项，只有最有用或最常用的配置选项。 命令行中的设置将覆盖通过配置文件和环境传递的设置。



可用选项的完整列表在ansible-playbook和ansible中。