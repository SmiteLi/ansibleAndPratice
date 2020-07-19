# Ansible从入门到实战（4）-Ansible常用模块A篇

Ansible已经为我们管理服务器带来了便利，那么playbook又是什么？为什么需要它？

设想一个场景：你需要部署三台nginx服务器。当你使用Ansible来部署时，你可能需要敲定不少命
令，比如把安装包复制到指定的服务器，然后解压，运行，修改配置文件等，这其中必定会花费不少
心神和时间。然而有了playbook，你只需要把各个步骤编写好，最后运行这个playbook即可。最大
的优势是，下一次还需要部署的话，只需要修改一下IP等少量信息就可以再次运行了。

看到这，是不是已经迫不及待想学习playbook了呢？


## 一、playbook是什么？

playbook是Ansible的配置，部署和编排语言。它们可以描述您希望远程服务器执行的各种操作，
策略，或一般IT流程中的一组步骤。

从根本上讲，剧本可用于管理远程计算机的配置和部署。 在更高的级别上，他们可以对涉及滚动更新的多层部署进行排序，并且可以将操作委派给其他主机，并与监视服务器和负载平衡器进行交互。

本被设计为易于阅读，并以基本文字语言开发。有多种方法来组织剧本及其包含的文件，我们将就此提供一些建议，并充分利用Ansible。

## 二、一个简单的playbook实例

playbook文件使用的是YMAL格式的描述语言，示例如下：

verify-apache.yml：

```yaml
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running
    service:
      name: httpd
      state: started
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```

让我们来解读一下上面的例子：
- hosts，指明本次playbook要操作的主机，是主机清单里的一个组或一台主机
- vars，配置本次playbook要使用的变量
- remote_user，指明远程主机执行任务时的用户
- tasks，指明本次playbook要执行的操作。可以看到，tasts之下是各个task，每一个task都指
定了一个模块，完成一个操作。如果你对之前的模块使用已经熟悉，是不是发现其实就是那些命令
换了一个写法而已。是的，实际上就是。
- handlers，



## 三、详细谈下主机和用户

- 也可以为每个任务定义远程用户：

```yaml
---
- hosts: webservers
  remote_user: root
  tasks:
    - name: test connection
      ping:
      remote_user: yourname
```

- 还支持以其他用户身份运行:

```yaml
---
- hosts: webservers
  remote_user: yourname
  become: yes
```

- 您还可以在特定的task中使用关键字behome，而不是整个任务

```yaml
---
- hosts: webservers
  remote_user: yourname
  tasks:
    - service:
        name: nginx
        state: started
      become: yes
      become_method: sudo
```

- 还可以以您的身份登录，然后成为不同于root用户的用户：

```yaml
---
- hosts: webservers
  remote_user: yourname
  become: yes
  become_user: postgres
```

- 您还可以使用其他特权升级方法，例如su：

```yaml
---
- hosts: webservers
  remote_user: yourname
  become: yes
  become_method: su
```

>If you need to specify a password for sudo, run ansible-playbook with --ask-become-pass or -K. If you run a playbook utilizing become and the playbook seems to hang, it’s probably stuck at the privilege escalation prompt and can be stopped using Control-C, allowing you to re-execute the playbook adding the appropriate password.

## 四、详细谈下tasks

每个playbook包含一个任务列表。在继续执行下一个task之前，针对与主机模式匹配的所有机器，一次执行一个任务。

重要的是要了解，在playbook中，所有主机都将获得相同的任务指令。playbook的目的是将主机的选择映射到任务。

从上到下运行的剧本时，任务失败的主机将从整个剧本的轮换中删除。如果失败，只需更正剧本文件并重新运行即可。每个任务的目标是执行一个带有非常具体参数的模块。变量可以在模块的参数中使用。模块应该是幂等的，也就是说，按顺序运行模块多次应与仅运行一次模块具有相同的效果。

一种实现幂等的方法是让模块检查是否已达到其所需的最终状态，如果已经达到该状态，则不执行任何操作即可退出。如果剧本使用的所有模块都是幂等的，则剧本本身很可能是幂等的，因此重新运行剧本应该是安全的。

每个任务都应有一个name，该名称包含在运行剧本的输出中。 这是人类易理解的输出，因此提供每个任务步骤的良好描述很有用。 如果未提供名称，则执行task后的信息反馈字符串将用于输出。

一个task如下：

```yaml
tasks:
  - name: Copy ansible inventory file to client
    copy: src=/etc/ansible/hosts dest=/etc/ansible/hosts
            owner=root group=root mode=0644
```

对于task的写法，如上，建议写了模块名，之后跟操作，也就是上面的copy模块。

- 已经定义了的变量可以用在task中。如定义了变量 vhost 在 vars指示符哪里，那么我们可以：

```yaml
tasks:
  - name: create a virtual host file for {{ vhost }}
    template:
      src: somefile.j2
      dest: /etc/httpd/conf.d/{{ vhost }}
```

## 五、详细谈下Handlers

在实际使用中，假如我们修改了apache服务的配置文件，那么我们就需要重启apache服务，让配置生效。
那么playbook有没有这样的配置呢？

先看个例子：

```
- name: template configuration file
  template:
    src: template.j2
    dest: /etc/foo.conf
  notify:
     - restart memcached
     - restart apache

handlers:   
  - name: restart memcached
   service:
     name: memcached
     state: restarted
  - name: restart apache
   service:
     name: apache
     state: restarted
```

上面的例子，notify表示 template 模块的操作完成后，会去通知 "restart memcached"
和 "restart apache"这两个handlers。由此我们可以看出，handlers就是用于在一个task执行
后，去通知另一个需要紧接着执行的任务。

当然，handlers也可以监听task，如下

```yml
handlers:
    - name: restart memcached
      service:
        name: memcached
        state: restarted
      listen: "restart web services"
    - name: restart apache
      service:
        name: apache
        state: restarted
      listen: "restart web services"

tasks:
    - name: restart everything
      command: echo "this task will restart the web services"
      notify: "restart web services"
```

## 六、如果执行一个playbook？

很简单：`ansible-playbook playbook.yml -f 10`
检查语法：`ansible-playbook playbook.yml --syntax-check -f 10`
查看对哪些主机执行：`ansible-playbook playbook.yml --list-hosts`
查看详细输出：`ansible-playbook playbook.yml --verbose -f 10`

检查playbook具体执行（不是真正执行）：`ansible-lint veryify-apache.yml`

## 十一、下一节是？

下一节继续介绍常用的Ansible模块
