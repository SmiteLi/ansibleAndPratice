# Ansible快速入门指导

## 一、基础概念

1. 如何快速学习一门技术？

对于计算机科学与技术，虽然只有八个字，却是无数前人累积的知识构建的庞大体系。对于现代的我们，不可能从二进制慢慢到汇编到高级编程语言这样学习，那得花上数年甚至数十年的时间。学习任何一门IT技术，最好的思路是：

- 这门技术是什么？
- 手把手实践这么技术
- 这么技术为什么会这样子设计？

现在，我们准备学习ansible，让我们遵循What，practice，why（WPW）的学习思路，先把ansible的概念过一遍，实践一遍，然后从入门到实践系统学习这门技术。

2. Ansible是什么？

Ansible是一款无代理的自动化工具，通俗来说，就是使用SSH协议连接到各个服务器，然后执行各种预先编排的任务，以此达到从一个点（服务器）出发，三百六十度无死角般管理数十上百甚至更多的服务器。本教程简化介绍Ansible的基础知识，以及一个系统管理员如何使用Ansible工具更高效地管理系统。

在开始之前，我们知道Ansible中一些常用的术语：

**Control node:** the host on which you use Ansible to execute tasks on the managed nodes

控制节点：安装了Ansible的主机，用来管理其他服务器（被管理节点）

**Managed node:** a host that is configured by the control node

被管理节点：一台主机，收到控制节点的管理

**Host inventory:** a list of managed nodes

：定义了被管理的主机是哪些，通常是填写hostname或者使用IP

**Ad-hoc command:** 一次执行一条命令

**Playbook:** 用于复杂管理的可重复执行任务

**Module:** 模块，用于实现各种功能，例如user模块，用于对Linux系统的用户管理。

**Idempotency:** 幂等性，不管执行多少次，最后的结果达到我们的预期。

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

## 二、实验环境

搭建实验环境：一个控制节点，两个被管理节点。

控制节点的/etc/hosts如下：

```
192.168.102.211 vm1 vm1.redhat.lab
192.168.102.212 vm2 vm2.redhat.lab
192.168.102.213 vm3 vm3.redhat.lab
```

为了简化使用，在服务器设置执行sudo命令不需要密码：

`visudo`然后设置如下：

```
%wheel ALL=(ALL) NOPASSWD: ALL
```

也可以直接修改/etc/sudoers文件，效果一样的。

接着配置该用户ssh免密登录管理节点：

1. 在控制节点执行：`ssh-keygen -b 2048 -t rsa -q -N "" -f ~/.ssh/id_rsa`
2. 把控制节点公钥发送到被管理节点去：

` ssh-copy-id 192.168.1.1`

## 三、安装ansible

Ansible for Red Hat Enterprise Linux 7 is located in the [Extras channel](https://access.redhat.com/solutions/912213). If you’re using Red Hat Enterprise Linux 6, enable the [EPEL](http://fedoraproject.org/wiki/EPEL) repository. For Extra Packages for Enterprise Linux (EPEL), this [solution](https://access.redhat.com/solutions/3358) in the customer portal may also be helpful. On Fedora systems you will find Ansible in the base repository.

Once the appropriate repository has been configured, it’s a quick and simple install:

```
[curtis@vm1 ~]$ sudo yum install -y ansible
```

Let’s check the version:

```
[curtis@vm1 ~]$ ansible --version
ansible 2.4.1.0
 config file = /etc/ansible/ansible.cfg
 configured module search path = [u'/home/curtis/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
 ansible python module location = /usr/lib/python2.7/site-packages/ansible
 executable location = /bin/ansible
 python version = 2.7.5 (default, May 3 2017, 07:55:04) [GCC 4.8.5 20150623 (Red Hat 4.8.5-14)]
```

Note the default configuration file, and that python is required and present in our minimal Red Hat Enterprise Linux 7.4 installation.

## 四、配置ansible

### 决定ansible行为的ansible.cfg配置文件

默认配置文件是/etc/ansible/ansible.cfg，全局配置，对服务器上所有使用ansible的用户生效。ansible使用ansible.cfg的顺序如下，一旦发现了配置，直接使用，就不再搜索下层的文件。

优先级如下：

- ```
  ANSIBLE_CONFIG (environment variable)
  ```

- ```
  ansible.cfg (per directory)
  ```

- ```
  ~/.ansible.cfg (home directory)
  ```

- ```
  /etc/ansible/ansible.cfg (global)
  ```

本次我们的ansible.cfg配置如下：指定了hosts文件。

```
[curtis@vm1 ~]$ cat ansible.cfg
[defaults]
inventory = $HOME/hosts
```

### Host inventory：主机清单

默认的主机清单文件是/etc/ansible/hosts。不过本次我们通过ansible.cfg配置了主机清单文件为$HOME/hosts。我们的hosts文件如下：

```
[webservers]
vm2
vm3

[dbservers]
vm4

[logservers]
vm5

[lamp:children]
webservers
dbservers
```

这里我们定义了四个组: vm2和vm3属于webservers组；vm4属于dbservers组,，vm5属于logservers组，webservers组和dbservers 组构成了更大的组：lamp组。

查看配置的所有主机:

```
[curtis@vm1 ~]$ ansible all --list-hosts
 hosts (4):
   vm5
   vm2
   vm3
   vm4
```

查看webservers组包含的主机:

```
[curtis@vm1 ~]$ ansible webservers --list-hosts
 hosts (2):
   vm2
   vm3
```

使用ansible ping所有主机:

```
[curtis@vm1 ~]$ ansible all -m ping
vm4 | SUCCESS => {
   "changed": false, 
   "failed": false, 
   "ping": "pong"
}
vm5 | SUCCESS => {
   "changed": false, 
   "failed": false, 
   "ping": "pong"
}
vm3 | SUCCESS => {
   "changed": false, 
   "failed": false, 
   "ping": "pong"
}
vm2 | SUCCESS => {
   "changed": false, 
   "failed": false, 
   "ping": "pong"
}
```

从截图可以看到SUCCESS了, 每一台主机都返回了"pong"的结果。.

查看可用的module:

```
[curtis@vm1 ~]$ ansible-doc -l
```

查看有多少module:

```
[curtis@vm1 ~]$ ansible-doc -l | wc -l
1378
```

## Ansible初体验

设置好ansible后，下面用ansible来进行一下实际的工作：

```
[curtis@vm1 ~]$ ansible all -m copy -a 'src=dvd.repo dest=/etc/yum.repos.d owner=root group=root mode=0644' -b
vm5 | SUCCESS => {
   "changed": true, 
   "checksum": "c15fdb5c1183f360ce29a1274c5f69e4e43060f5", 
   "dest": "/etc/yum.repos.d/dvd.repo", 
   "failed": false, 
   "gid": 0, 
   "group": "root", 
   "md5sum": "db5a5da08d1c4be953cd0ae6625d8358", 
   "mode": "0644", 
   "owner": "root", 
   "secontext": "system_u:object_r:system_conf_t:s0", 
   "size": 135, 
   "src": "/home/curtis/.ansible/tmp/ansible-tmp-1516898124.58-210025572567032/source", 
   "state": "file", 
   "uid": 0
}

[...]
```

- -m指定使用的module
- -a 指定该模块的参数

上面例子，把hello文件copy到vm1中，设置属于root用户，root用户组，文件权限是0644。

-b选项表明使用权限提升，如使用sudo，这样我们就不会遇到因为权限不足导致执行失败。

查看copy模块的用法：

```
[curtis@vm1 ~]$ ansible-doc copy
```

## Playbooks

上面的命令，一次只能完成一项task（任务）。但需要多次操作时，就需要多次执行。当把这些task编排到一个文件里时，就成了ansible中的playbook（剧本）。

ansible在运行时可以采集被管理主机的基本信息:

```
[curtis@vm1 ~]$ ansible vm2 -m setup
```
部署nginx的playbook nginx.yml内容如下：
```
---
- hosts: webservers
 become: yes
 tasks:
   - name: install Apache server
     yum:
       name: httpd
       state: latest

   - name: enable and start Apache server
     service:
       name: httpd
       enabled: yes
       state: started

   - name: open firewall port
     firewalld:
       service: http
       immediate: true
       permanent: true
       state: enabled

   - name: create web admin group
     group:
       name: web
       state: present

   - name: create web admin user
     user:
       name: webadm
       comment: "Web Admin"
       password: $6$rounds=656000$bp7zTIl.nar2WQPS$U5CBB15GHnzBqnhY0r7UX65FrBI6w/w9YcAL2kN9PpDaYQIDY6Bi.CAEL6PRRKUqe2bJYgsayyh9NOP1kUy4w.
       groups: web
       append: yes

   - name: set content directory group/permissions 
     file:
       path: /var/www/html
       owner: root
       group: web
       state: directory
       mode: u=rwx,g=rwx,o=rx,g+s

   - name: create default page content
     copy:
       content: "Welcome to {{ ansible_fqdn}} on {{ ansible_default_ipv4.address }}"
       dest: /var/www/html/index.html
       owner: webadm
       group: web
       mode: u=rw,g=rw,o=r

- hosts: dbservers
 become: yes
 tasks:
   - name: install MariaDB server
     yum:
       name: mariadb-server
       state: latest

   - name: enable and start MariaDB server
     service:
       name: mariadb
       enabled: yes
       state: started

- hosts: logservers
 become: yes
 tasks:
   - name: configure rsyslog remote log reception over udp
     lineinfile:
       path: /etc/rsyslog.conf
       line: "{{ item }}"
       state: present
     with_items:
       - '$ModLoad imudp'
       - '$UDPServerRun 514'
     notify:
       - restart rsyslogd

   - name: open firewall port
     firewalld:
       port: 514/udp
       immediate: true
       permanent: true
       state: enabled

 handlers:
   - name: restart rsyslogd
     service:
       name: rsyslog
       state: restarted

- hosts: lamp
 become: yes
 tasks:
   - name: configure rsyslog
     lineinfile:
       path: /etc/rsyslog.conf
       line: '*.* @192.168.102.215:514'
       state: present
     notify:
       - restart rsyslogd

 handlers:
   - name: restart rsyslogd
     service:
       name: rsyslog
       state: restarted
```

### 运行playbook

```
[curtis@vm1 ~]$ ansible-playbook myplaybook.yml
```

可以看到输出的结果如下：

```
PLAY [webservers] *********************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [vm2]
ok: [vm3]

TASK [install Apache server] **********************************************************
changed: [vm3]
changed: [vm2]

TASK [enable and start Apache server] *************************************************
changed: [vm2]
changed: [vm3]

TASK [open firewall port] *************************************************************
changed: [vm2]
changed: [vm3]

TASK [create web admin group] *********************************************************
changed: [vm3]
changed: [vm2]

TASK [create web admin user] **********************************************************
changed: [vm3]
changed: [vm2]

TASK [set content directory group/permissions] ****************************************
changed: [vm3]
changed: [vm2]

TASK [create default page content] ****************************************************
changed: [vm3]
changed: [vm2]

PLAY [dbservers] **********************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [vm4]

TASK [install MariaDB server] *********************************************************
changed: [vm4]

TASK [enable and start MariaDB server] ************************************************
changed: [vm4]

PLAY [logservers] *********************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [vm5]

TASK [configure rsyslog remote log reception over udp] ********************************
changed: [vm5] => (item=$ModLoad imudp)
changed: [vm5] => (item=$UDPServerRun 514)

TASK [open firewall port] *************************************************************
changed: [vm5]

RUNNING HANDLER [restart rsyslogd] ****************************************************
changed: [vm5]

PLAY [lamp] ***************************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [vm3]
ok: [vm2]
ok: [vm4]

TASK [configure rsyslog] **************************************************************
changed: [vm2]
changed: [vm3]
changed: [vm4]

RUNNING HANDLER [restart rsyslogd] ****************************************************
changed: [vm3]
changed: [vm2]
changed: [vm4]

PLAY RECAP ****************************************************************************
vm2                        : ok=11 changed=9 unreachable=0    failed=0 
vm3                        : ok=11 changed=9 unreachable=0    failed=0 
vm4                        : ok=6 changed=4 unreachable=0    failed=0 
vm5                        : ok=4 changed=3 unreachable=0    failed=0 
```

验证部署是否成功：

```
[curtis@vm1 ~]$ curl http://vm2
Welcome to vm2 on 192.168.102.212
[curtis@vm1 ~]$ curl http://vm3
Welcome to vm3 on 192.168.102.213 
```

查看nginx的access日志：



运行playbook前，检查playbook的yaml语法是否正确:

```
$ ansible-playbook --syntax-check myplaybook.yml
```

在~/.vimrc文件加入下面的配置，对yaml语法进行高亮和检查：

```
autocmd Filetype yaml setlocal tabstop=2 ai colorcolumn=1,3,5,7,9,80
```

测试对被管理节点产生哪些修改：

```
$ ansible-playbook --check myplaybook.yml
```

单步执行playbook

```
$ ansible-playbook --step myplaybook.yml
```

类似shell脚本，可在playbook第一行加入如下配置：

```
#!/bin/ansible-playbook
```
