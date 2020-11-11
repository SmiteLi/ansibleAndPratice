# playbook的条件控制

通常，一个play的结果可能取决于变量的值，facts（有关远程系统的知识）或先前的task结果。 在某些情况下，变量的值可能取决于其他变量。 可以基于主机是否符合其他条件来创建其他组来管理主机。 

下面介绍如何在playbook中使用条件控制。

## when语句

当我们想关闭Linux版本为Debian的系统时：

```yaml
tasks:
  - name: "shut down Debian flavored systems"
    command: /sbin/shutdown -t now
    when: ansible_facts['os_family'] == "Debian"
    # note that all variables can be used directly in conditionals without double curly braces
```

when语句表示当该条件的判断为True时会执行该task。

多个条件可用括号，如下：

```yaml
tasks:
  - name: "shut down CentOS 6 and Debian 7 systems"
    command: /sbin/shutdown -t now
    when: (ansible_facts['distribution'] == "CentOS" and ansible_facts['distribution_major_version'] == "6") or
          (ansible_facts['distribution'] == "Debian" and ansible_facts['distribution_major_version'] == "7")
```

多个条件都为True时可用列表的形式：

```yaml
tasks:
  - name: "shut down CentOS 6 systems"
    command: /sbin/shutdown -t now
    when:
      - ansible_facts['distribution'] == "CentOS"
      - ansible_facts['distribution_major_version'] == "6"
```

假设我们要忽略一个语句的错误，然后根据成功或失败决定有条件地执行某项操作：

```
tasks:
  - command: /bin/false
    register: result
    ignore_errors: True

  - command: /bin/something
    when: result is failed

  # In older versions of ansible use ``success``, now both are valid but succeeded uses the correct tense.
  - command: /bin/something_else
    when: result is succeeded

  - command: /bin/still/something_else
    when: result is skipped
```

fail/failed,success/succeeded都生效。 其他返回结果也可以参考来写。

要查看特定系统上可用的facts，可以在playbook中执行以下操作：

```
- debug: var=ansible_facts
```

对字符串进行数字比较时，可以转换字符串为数值类型，再比较：

```
tasks:
  - shell: echo "only on Red Hat 6, derivatives, and later"
    when: ansible_facts['os_family'] == "RedHat" and ansible_facts['lsb']['major_release']|int >= 6
```

playbook中或者inventory中定义的变量都可以使用。对于非布尔值的变量，需要使用bool过滤器，把类似‘yes’, ‘on’, ‘1’, ‘true’这样的字符串转为布尔值。下面是一个例子：

```
vars:
  epic: true
  monumental: "yes"
```

然后，条件执行可能看起来像：

```yaml
tasks:
    - shell: echo "This certainly is epic!"
      when: epic or monumental|bool
```

或者：

```yaml
tasks:
    - shell: echo "This certainly isn't epic!"
      when: not epic
```

如果变量没有配置，可以跳过或者设置一个默认值：

```yaml
tasks:
    - shell: echo "I've got '{{ foo }}' and am not afraid to use it!"
      when: foo is defined

    - fail: msg="Bailing out. this play requires 'bar'"
      when: bar is undefined
```

从上面的例子可以看到，在条件语句里，变量不需要使用{{}}的。

## 循环与条件

when和loop的联合使用：

```
tasks:
    - command: echo {{ item }}
      loop: [ 0, 2, 4, 6, 8, 10 ]
      when: item > 5
```

如果需要根据定义的循环变量跳过整个任务，可使用|default过滤器提供空的迭代器：

```
- command: echo {{ item }}
  loop: "{{ mylist|default([]) }}"
  when: item > 5
```

如果是字典，则可以：

```
- command: echo {{ item.key }}
  loop: "{{ query('dict', mydict|default({})) }}"
  when: item.value > 5
```

## roles、imports和includes使用when语句

当'reticulating splines'在output输出时，导入tasks/sometasks.yml：

```
- import_tasks: tasks/sometasks.yml
  when: "'reticulating splines' in output"
```

对role使用：

```
- hosts: webservers
  roles:
    - role: debian_stock_config
      when: ansible_facts['os_family'] == 'Debian'
```

## 条件导入

```yaml
---
- hosts: all
  remote_user: root
  vars_files:
    - "vars/common.yml"
    - [ "vars/{{ ansible_facts['os_family'] }}.yml", "vars/os_defaults.yml" ]
  tasks:
  - name: make sure apache is started
    service: name={{ apache }} state=started
```

导入vars/RedHat.yml，或者‘vars/Debian.yml或者vars/os_defaults.yml等。如果没有导入，则会抛出错误。

## 根据变量选择文件或者模板

```yaml
  template:
      src: "{{ item }}"
      dest: /etc/myapp/foo.conf
  loop: "{{ query('first_found', { 'files': myfiles, 'paths': mypaths}) }}"
  vars:
    myfiles:
      - "{{ansible_facts['distribution']}}.conf"
      -  default.conf
    mypaths: ['search_location_one/somedir/', '/opt/other_location/somedir/']
```

## [Register Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html#id12)

即使由于条件而跳过任务，也会发生注册。 这样，您可以查询变量“is skipped”，以了解是否尝试执行任务。

“ register”关键字决定要保存结果的变量。结果变量可以在模板，action lines或when语句中使用。 看起来像这样（在一个简单的例子中）：

```
- name: test play
  hosts: all

  tasks:
      - shell: cat /etc/motd
        register: motd_contents

      - shell: echo "motd contains the word hi"
        when: motd_contents.stdout.find('hi') != -1
```

如前所示，可以使用“ stdout”值访问已注册变量的字符串内容。 如果将注册结果转换为列表（或已经是列表），则可以在任务循环中使用它，如下所示。 “ stdout_lines”也已在对象上可用，但是如果您愿意的话，您也可以调用“ home_dirs.stdout.split（）”，并且可以按其他字段拆分：

```
- name: registered variable usage as a loop list
  hosts: all
  tasks:

    - name: retrieve the list of home directories
      command: ls /home
      register: home_dirs

    - name: add home dirs to the backup spooler
      file:
        path: /mnt/bkspool/{{ item }}
        src: /home/{{ item }}
        state: link
      loop: "{{ home_dirs.stdout_lines }}"
      # same as loop: "{{ home_dirs.stdout.split() }}"
```

如前所示，可以使用“ stdout”值访问已注册变量的字符串内容。 您可以检查注册变量的字符串内容是否为空：

```
- name: check registered variable for emptiness
  hosts: all

  tasks:

      - name: list contents of directory
        command: ls mydir
        register: contents

      - name: check contents for emptiness
        debug:
          msg: "Directory is empty"
        when: contents.stdout == ""
```

## 一些常用的Facts

### [ansible_facts\[‘distribution’\]

可能的值为 (不是完全的列表):

```
Alpine
Altlinux
Amazon
Archlinux
ClearLinux
Coreos
CentOS
Debian
Fedora
Gentoo
Mandriva
NA
OpenWrt
OracleLinux
RedHat
Slackware
SMGL
SUSE
Ubuntu
VMwareESX
```



### [ansible_facts\[‘distribution_major_version’\]

操作系统的主版本号，如Ubuntu16.04的系统，该值为16

This will be the major version of the operating system. For example, the value will be 16 for Ubuntu 16.04.

### [ansible_facts\[‘os_family’\]

可能的值为 (不是完全的列表):

```
AIX
Alpine
Altlinux
Archlinux
Darwin
Debian
FreeBSD
Gentoo
HP-UX
Mandrake
RedHat
SGML
Slackware
Solaris
Suse
Windows
```