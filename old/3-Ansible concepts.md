# Ansible concepts

These concepts are common to all uses of Ansible. You need to understand them to use Ansible for any kind of automation. This basic introduction provides the background you need to follow the rest of the User Guide.

- [Control node](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#control-node)
- [Managed nodes](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#managed-nodes)
- [Inventory](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#inventory)
- [Modules](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#modules)
- [Tasks](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#tasks)
- [Playbooks](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#playbooks)

## [Control node](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id1)

Any machine with Ansible installed. You can run commands and playbooks, invoking `/usr/bin/ansible` or `/usr/bin/ansible-playbook`, from any control node. You can use any computer that has Python installed on it as a control node - laptops, shared desktops, and servers can all run Ansible. However, you cannot use a Windows machine as a control node. You can have multiple control nodes.

任何装有Ansible的机器。 您可以从任何控制节点运行命令和剧本，并调用/ usr / bin / ansible或/ usr / bin / ansible-playbook。 您可以将任何安装了Python的计算机用作控制节点-笔记本电脑，共享台式机和服务器都可以运行Ansible。 但是，不能将Windows计算机用作控制节点。 您可以有多个控制节点

## [Managed nodes](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id2)

The network devices (and/or servers) you manage with Ansible. Managed nodes are also sometimes called “hosts”. Ansible is not installed on managed nodes.

您使用Ansible管理的网络设备（和/或服务器）。 受管节点有时也称为“主机”。 未在受管节点上安装Ansible

## [Inventory](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id3)

A list of managed nodes. An inventory file is also sometimes called a “hostfile”. Your inventory can specify information like IP address for each managed node. An inventory can also organize managed nodes, creating and nesting groups for easier scaling. To learn more about inventory, see [the Working with Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory) section.

受管节点的列表。 清单文件有时也称为“主机文件”。 您的清单可以为每个受管节点指定诸如IP地址之类的信息。 库存还可以组织受管节点，创建和嵌套组以便于扩展。 要了解有关库存的更多信息，请参见使用库存部分。

## [Modules](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id4)

The units of code Ansible executes. Each module has a particular use, from administering users on a specific type of database to managing VLAN interfaces on a specific type of network device. You can invoke a single module with a task, or invoke several different modules in a playbook. For an idea of how many modules Ansible includes, take a look at the [list of all modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html#modules-by-category).

将执行代码单元Ansible。 从管理特定类型的数据库上的用户到管理特定类型的网络设备上的VLAN接口，每个模块都有特定的用途。 您可以通过任务调用单个模块，也可以在剧本中调用多个不同的模块。 要了解Ansible包含多少个模块，请查看所有模块的列表

## [Tasks](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id5)

The units of action in Ansible. You can execute a single task once with an ad-hoc command.

Ansible中的行动单位。 您可以使用临时命令一次执行一个任务

## [Playbooks](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#id6)

Ordered lists of tasks, saved so you can run those tasks in that order repeatedly. Playbooks can include variables as well as tasks. Playbooks are written in YAML and are easy to read, write, share and understand. To learn more about playbooks, see [About Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#about-playbooks).

已保存任务的有序列表，以便您可以按该顺序重复运行那些任务。 剧本可以包括变量以及任务。 剧本采用YAML编写，易于阅读，编写，共享和理解。 要了解有关剧本的更多信息，请参阅关于剧本