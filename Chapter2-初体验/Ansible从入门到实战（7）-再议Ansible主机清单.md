# Ansible从入门到实战（7）-再议Ansible主机清单

## 一、基础
Ansible默认的主机清单文件是/etc/ansible/hosts，当然我们也可以使用 -i <path> 来指定主
机清单文件

## 二、清单文件基础：主机与主机组

先看一个典型的 hosts 文件（INI格式）：

```ini
mail.example.com

[webservers]
foo.example.com
bar.example.com
one.example.com
two.example.com
three.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

中括号里的名字为主机组名，如webservers，dbservers。非中括号的每一行是主机名称，如
mail.example.com，bar.example.com。

hosts文件还可以写成YAML的格式：

```yml
---
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
        one.example.com:
        two.example.com:
        three.example.com:  
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```

- 一台主机多个角色

显然，一台服务器有时既是webserver，也是dbserver，也就是说一台主机可以同时在多个主机组里。
如果你注意看上面的例子，可以看到主机one.example.com既是webservers，也是dbservers。

- SSH端口不是默认的22怎么办？

如果是YAML格式的文件，直接标明SSH 端口

```yml
...
  hosts:
    jumper:
      ansible_port: 5555
      ansible_host: 192.0.2.50
```

如果是INI格式的文件，写法如下：

```ini
jumper ansible_port=5555 ansible_host=192.0.2.50
```

- 一些技巧

如果你有一些主机，名称是www[01:50].example.com，db-[a:f].example.com这样的，那么你可
以把它们写成下面的形式：

```
[targets]
localhost              ansible_connection=local
www[01:50].example.com     ansible_connection=ssh        ansible_user=mpdehaan
db-[a:f].example.com     ansible_connection=ssh        ansible_user=mdehaan
```

上面的例子是说 01到50这50台主机，或者是 a到f 这些主机。上面的ansible_connection指明了
连接方式。

## 三、在清单中分配变量

你会奇怪为什么要设置变量，其实在后面的playbook中会使用到，我们暂且在这里先学习。
一个例子：

```
[atlanta]
foo.example.com http_port=80 maxRequestsPerChild=808
bar.example.com

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```

我们可以为单台主机指定变量，如`foo.example.com http_port=80 maxRequestsPerChild=808`，
也可以为整个主机组设置共用的变量，如：
```
[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```
上面的例子是说ntp_server等变量是属于atlanta这个组的。

## 四、默认的主机组

前面我们运行 `ansible all -m ping` 等命令时，你是否奇怪 all 表示的意思？

看下官方解释：
>There are two default groups: all and ungrouped. all contains every host.
ungrouped contains all hosts that don’t have another group aside from all.

我们可以理解了：有两个默认组：all 和 ungrouped。all 包含每个主机。 ungrouped包含不在
任一组的所有主机

## 五、配置变量的另一种方式

尽管我们可以将变量存储在主清单文件中，但是存储单独的主机变量和组变量文件可以帮助我们更
轻松地跟踪变量值。那么应该如何配置呢？

一个例子：/etc/ansible/hosts 文件如下

```
[raleigh]
foosball

[webservers]
foosball
```

那么Ansible将使用以下位置的YAML文件中的变量：
```
/etc/ansible/group_vars/raleigh # can optionally end in '.yml', '.yaml', or '.json'
/etc/ansible/group_vars/webservers
/etc/ansible/host_vars/foosball
```

首先要知道一点，Ansible是使用YAML格式的变量文件的。所以，对于/etc/ansible/group_vars/raleigh 这
个文件，一个示例如下：
```yaml
---
ntp_server: acme.example.org
database_server: storage.example.org
```

当然上面的这种配置是可选的，所以不存在以上文件也是没有问题的。

我们还可以在主机或者主机组目录下创建子目录，Ansible将按字典顺序读取这些目录中的所有文件。
对 “ raleigh”组的示例如下：
```
/etc/ansible/group_vars/raleigh/db_settings
/etc/ansible/group_vars/raleigh/cluster_settings
```
变量文件放在上面的目录下

- 来自官方是三个TIPS：
>Tip1: The group_vars/ and host_vars/ directories can exist in the playbook directory OR the inventory directory. If both paths exist, variables in the playbook directory will override variables set in the inventory directory.

>Tip2: The ansible-playbook command looks for playbooks in the current working directory by default. Other Ansible commands (for example, ansible, ansible-console, etc.) will only look for group_vars/ and host_vars/ in the inventory directory unless you provide the --playbook-dir option on the command line.

>Tip3: Keeping your inventory file and variables in a git repo (or other version control) is an excellent way to track changes to your inventory and host variables.

## 六、变量的覆盖

我们可能分别在
host
groups
all
三个级别配置使用了apple这个变量。Ansible默认的会把最详细的变量值覆盖大范围的变量值。
也就是优先级为host > groups > all
