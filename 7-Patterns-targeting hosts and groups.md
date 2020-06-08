# Patterns: targeting hosts and groups

When you execute Ansible through an ad-hoc command or by running a playbook, you must choose which managed nodes or groups you want to execute against. Patterns let you run commands and playbooks against specific hosts and/or groups in your inventory. An Ansible pattern can refer to a single host, an IP address, an inventory group, a set of groups, or all hosts in your inventory. Patterns are highly flexible - you can exclude or require subsets of hosts, use wildcards or regular expressions, and more. Ansible executes on all inventory hosts included in the pattern.

- [Using patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#using-patterns)
- [Common patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#common-patterns)
- [Limitations of patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#limitations-of-patterns)
- Advanced pattern options
  - [Using variables in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#using-variables-in-patterns)
  - [Using group position in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#using-group-position-in-patterns)
  - [Using regexes in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#using-regexes-in-patterns)
- [Patterns and ansible-playbook flags](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#patterns-and-ansible-playbook-flags)

## [Using patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id2)

You use a pattern almost any time you execute an ad-hoc command or a playbook. The pattern is the only element of an [ad-hoc command](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#intro-adhoc) that has no flag. It is usually the second element:

```
ansible <pattern> -m <module_name> -a "<module options>""
```

For example:

```
ansible webservers -m service -a "name=httpd state=restarted"
```

In a playbook the pattern is the content of the `hosts:` line for each play:

```
- name: <play_name>
  hosts: <pattern>
```

For example:

```
- name: restart webservers
  hosts: webservers
```

Since you often want to run a command or playbook against multiple hosts at once, patterns often refer to inventory groups. Both the ad-hoc command and the playbook above will execute against all machines in the `webservers` group.



## [Common patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id3)

This table lists common patterns for targeting inventory hosts and groups.

| Description            | Pattern(s)                   | Targets                                             |
| ---------------------- | ---------------------------- | --------------------------------------------------- |
| All hosts              | all (or *)                   |                                                     |
| One host               | host1                        |                                                     |
| Multiple hosts         | host1:host2 (or host1,host2) |                                                     |
| One group              | webservers                   |                                                     |
| Multiple groups        | webservers:dbservers         | all hosts in webservers plus all hosts in dbservers |
| Excluding groups       | webservers:!atlanta          | all hosts in webservers except those in atlanta     |
| Intersection of groups | webservers:&staging          | any hosts in webservers that are also in staging    |

Note

You can use either a comma (`,`) or a colon (`:`) to separate a list of hosts. The comma is preferred when dealing with ranges and IPv6 addresses.

Once you know the basic patterns, you can combine them. This example:

```
webservers:dbservers:&staging:!phoenix
```

targets all machines in the groups ‘webservers’ and ‘dbservers’ that are also in the group ‘staging’, except any machines in the group ‘phoenix’.

You can use wildcard patterns with FQDNs or IP addresses, as long as the hosts are named in your inventory by FQDN or IP address:

```
192.0.\*
\*.example.com
\*.com
```

You can mix wildcard patterns and groups at the same time:

```
one*.com:dbservers
```

## [Limitations of patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id4)

Patterns depend on inventory. If a host or group is not listed in your inventory, you cannot use a pattern to target it. If your pattern includes an IP address or hostname that does not appear in your inventory, you will see an error like this:

```
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: Could not match supplied host pattern, ignoring: *.not_in_inventory.com
```

Your pattern must match your inventory syntax. If you define a host as an [alias](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory-aliases):

```
atlanta:
  host1:
    http_port: 80
    maxRequestsPerChild: 808
    host: 127.0.0.2
```

you must use the alias in your pattern. In the example above, your must use `host1` in your pattern. If you use the IP address, you will once again get the error:

```
[WARNING]: Could not match supplied host pattern, ignoring: 127.0.0.2
```

## [Advanced pattern options](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id5)

The common patterns described above will meet most of your needs, but Ansible offers several other ways to define the hosts and groups you want to target.

### [Using variables in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id6)

You can use variables to enable passing group specifiers via the `-e` argument to ansible-playbook:

```
webservers:!{{ excluded }}:&{{ required }}
```

### [Using group position in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id7)

You can define a host or subset of hosts by its position in a group. For example, given the following group:

```
[webservers]
cobweb
webbing
weber
```

you can use subscripts to select individual hosts or ranges within the webservers group:

```
webservers[0]       # == cobweb
webservers[-1]      # == weber
webservers[0:2]     # == webservers[0],webservers[1]
                    # == cobweb,webbing
webservers[1:]      # == webbing,weber
webservers[:3]      # == cobweb,webbing,weber
```

### [Using regexes in patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id8)

You can specify a pattern as a regular expression by starting the pattern with `~`:

```
~(web|db).*\.example\.com
```

## [Patterns and ansible-playbook flags](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#id9)

You can change the behavior of the patterns defined in playbooks using command-line options. For example, you can run a playbook that defines `hosts: all` on a single host by specifying `-i 127.0.0.2,`. This works even if the host you target is not defined in your inventory. You can also limit the hosts you target on a particular run with the `--limit` flag:

```
ansible-playbook site.yml --limit datacenter2
```

Finally, you can use `--limit` to read the list of hosts from a file by prefixing the file name with `@`:

```
ansible-playbook site.yml --limit @retry_hosts.txt
```

To apply your knowledge of patterns with Ansible commands and playbooks, read [Introduction to ad-hoc commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#intro-adhoc) and [About Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro).