# Templating (Jinja2)

https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html

Ansible 使用 Jinja2 templating 语法来动态使用变量。Ansible 里有很多过滤器，大大拓展了变量使用的灵活性。

在task执行前，所有变量会模板化，再在远端host上执行。

### 获取当前时间

```
---
- hosts: localhost
  tasks:
  - name: get the current time
    dubug:
      msg: "{{ now() }}"
```

## 一、过滤器

过滤器用于将数据转换成模板表达式。需要注意的是模板化发生在Ansible控制节点，不是在远端的host，所以过滤器也可以像操作本地数据一样操作控制节点。

- ### 格式转换

转换为json或yaml格式

```
{{ some_variable | to_json }}
{{ some_variable | to_yaml }}
```

```
{{ some_variable | to_nice_json }}
{{ some_variable | to_nice_yaml }}
```

也可以修改缩进：

```
{{ some_variable | to_nice_json(indent=2) }}
{{ some_variable | to_nice_yaml(indent=8) }}
```

to_yaml and to_nice_yaml 默认每行80个字符，超过80个时会有难以预料的情况，可以通过width修改每行长度：

```
{{ some_variable | to_yaml(indent=8, width=1337) }}
{{ some_variable | to_nice_yaml(indent=8, width=1337) }}
```

如果需要读取json或yaml的数据，可以：

```
{{ some_variable | from_json }}
{{ some_variable | from_yaml }}
```

比如：

```
tasks:
  - shell: cat /some/path/to/file.json
    register: result

  - set_fact:
      myvar: "{{ result.stdout | from_json }}"
```

如果一个yaml文件保护多个子yaml文件，可以使用from_yaml_all：

```
tasks:
  - shell: cat /some/path/to/multidoc-file.yaml
    register: result
  - debug:
      msg: '{{ item }}'
    loop: '{{ result.stdout | from_yaml_all | list }}'
```

from_yaml_all 会返回一个迭达器，迭达各个yaml文件。

- ### 变量是否定义

默认， ansible.cfg配置的是，Ansible执行时，如果遇到没有定义的变量，会报错中断。如果关闭了这个配置，那么可以在变量的使用时强制要求定义：

```
{{ variable | mandatory }}
```

变量值将按原样使用，但是如果未定义，模板评估将引发错误。

- ### 设置变量默认值

如果需要给变量设置一个默认值：

```
{{ some_variable | default(5) }}
```

如果some_variable没有定义，将会使用some_variable=5.

如果要在变量评估为false或为空字符串时使用默认值，则必须将第二个参数设置为true：

```
{{ lookup('env', 'MY_USER') | default('admin', true) }}
```

- ### 省略参数

通过default(omit) 省略模块需要的参数：

```
- name: touch files with an optional mode
  file:
    dest: "{{ item.path }}"
    state: touch
    mode: "{{ item.mode | default(omit) }}"
  loop:
    - path: /tmp/foo
    - path: /tmp/bar
    - path: /tmp/baz
      mode: "0444"
```

对于列表中的前两个文件，默认模式将由系统的umask决定，因为不会将mode =参数发送到文件模块，而最终文件将收到mode = 0444选项。

- ### 列表过滤器

对于列表变量，可以有：

取最小值：

```
{{ list1 | min }}
```

取最大值：

```
{{ [3, 4, 2] | max }}
```

展开列表：

```
{{ [3, [4, 2] ] | flatten }}
```

展开列表的第一层：

```
{{ [3, [4, [2]] ] | flatten(levels=1) }}
```

- ### 集合操作

去掉重复值：

```
{{ list1 | unique }}
```

两个列表联合：

```
{{ list1 | union(list2) }}
```

取列表的交集：

```
{{ list1 | intersect(list2) }}
```

在列表1，不在列表2：

```
{{ list1 | difference(list2) }}
```

不同时在列表1和列表2（只在列表1或列表2）的：

```
{{ list1 | symmetric_difference(list2) }}
```

- 字典过滤器

把字典转为列表：

```
{{ dict | dict2items }}
```

比如下面的dict：

```
tags:
  Application: payment
  Environment: dev
```

会被转为：

```
- key: Application
  value: payment
- key: Environment
  value: dev
```

dict2items接受2个关键字参数key_name和value_name，这些参数允许配置要用于转换的键名：

```
{{ files | dict2items(key_name='file', value_name='path') }}
```

上面把

```
files:
  users: /etc/passwd
  groups: /etc/group
```

转为：

```
- file: users
  path: /etc/passwd
- file: groups
  path: /etc/group
```

- ### items2dict 过滤器

此过滤器将具有2个键的字典列表转换为dict，将这些键的值映射为key：value对：

```
{{ tags | items2dict }}
```

如：

```
tags:
  - key: Application
    value: payment
  - key: Environment
    value: dev
```

转为：

```
Application: payment
Environment: dev
```

这是dict2items过滤器的逆向。

items2dict接受2个关键字参数key_name和value_name，这些参数允许配置要用于转换的键名：

```
{{ tags | items2dict(key_name='key', value_name='value') }}
```

- ## zip and zip_longest filters

要获得结合其他列表元素的列表，请使用zip：

```
- name: give me list combo of two lists
  debug:
   msg: "{{ [1,2,3,4,5] | zip(['a','b','c','d','e','f']) | list }}"

- name: give me shortest combo of two lists
  debug:
    msg: "{{ [1,2,3] | zip(['a','b','c','d','e','f']) | list }}"
```

要始终用尽所有列表，请使用zip_longest：

```
- name: give me longest combo of three lists , fill with X
  debug:
    msg: "{{ [1,2,3] | zip_longest(['a','b','c','d','e','f'], [21, 22, 23], fillvalue='X') | list }}"
```

与上面提到的items2dict过滤器的输出类似，这些过滤器可用于构建dict：

把：

```
keys_list:
  - one
  - two
values_list:
  - apple
  - orange
```

转为：

```
one: apple
two: orange
```