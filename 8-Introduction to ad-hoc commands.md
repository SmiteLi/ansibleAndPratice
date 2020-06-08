# Introduction to ad-hoc commands

An Ansible ad-hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes. Ad-hoc commands are quick and easy, but they are not reusable. So why learn about ad-hoc commands first? Ad-hoc commands demonstrate the simplicity and power of Ansible. The concepts you learn here will port over directly to the playbook language. Before reading and executing these examples, please read [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory).

- [Why use ad-hoc commands?](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#why-use-ad-hoc-commands)
- Use cases for ad-hoc tasks
  - [Rebooting servers](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#rebooting-servers)
  - [Managing files](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#managing-files)
  - [Managing packages](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#managing-packages)
  - [Managing users and groups](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#managing-users-and-groups)
  - [Managing services](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#managing-services)
  - [Gathering facts](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#gathering-facts)

## [Why use ad-hoc commands?](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id4)

Ad-hoc commands are great for tasks you repeat rarely. For example, if you want to power off all the machines in your lab for Christmas vacation, you could execute a quick one-liner in Ansible without writing a playbook. An ad-hoc command looks like this:

```
$ ansible [pattern] -m [module] -a "[module options]"
```

You can learn more about [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) and [modules](https://docs.ansible.com/ansible/latest/user_guide/modules.html#working-with-modules) on other pages.

## [Use cases for ad-hoc tasks](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id5)

Ad-hoc tasks can be used to reboot servers, copy files, manage packages and users, and much more. You can use any Ansible module in an ad-hoc task. Ad-hoc tasks, like playbooks, use a declarative model, calculating and executing the actions required to reach a specified final state. They achieve a form of idempotence by checking the current state before they begin and doing nothing unless the current state is different from the specified final state.

### [Rebooting servers](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id6)

The default module for the `ansible` command-line utility is the [command module](https://docs.ansible.com/ansible/latest/modules/command_module.html#command-module). You can use an ad-hoc task to call the command module and reboot all web servers in Atlanta, 10 at a time. Before Ansible can do this, you must have all servers in Atlanta listed in a a group called [atlanta] in your inventory, and you must have working SSH credentials for each machine in that group. To reboot all the servers in the [atlanta] group:

```
$ ansible atlanta -a "/sbin/reboot"
```

By default Ansible uses only 5 simultaneous processes. If you have more hosts than the value set for the fork count, Ansible will talk to them, but it will take a little longer. To reboot the [atlanta] servers with 10 parallel forks:

```
$ ansible atlanta -a "/sbin/reboot" -f 10
```

/usr/bin/ansible will default to running from your user account. To connect as a different user:

```
$ ansible atlanta -a "/sbin/reboot" -f 10 -u username
```

Rebooting probably requires privilege escalation. You can connect to the server as `username` and run the command as the `root` user by using the [become](https://docs.ansible.com/ansible/latest/user_guide/become.html#become) keyword:

```
$ ansible atlanta -a "/sbin/reboot" -f 10 -u username --become [--ask-become-pass]
```

If you add `--ask-become-pass` or `-K`, Ansible prompts you for the password to use for privilege escalation (sudo/su/pfexec/doas/etc).

Note

The [command module](https://docs.ansible.com/ansible/latest/modules/command_module.html#command-module) does not support extended shell syntax like piping and redirects (although shell variables will always work). If your command requires shell-specific syntax, use the shell module instead. Read more about the differences on the [Working With Modules](https://docs.ansible.com/ansible/latest/user_guide/modules.html#working-with-modules) page.

So far all our examples have used the default ‘command’ module. To use a different module, pass `-m` for module name. For example, to use the [shell module](https://docs.ansible.com/ansible/latest/modules/shell_module.html#shell-module):

```
$ ansible raleigh -m shell -a 'echo $TERM'
```

When running any command with the Ansible *ad hoc* CLI (as opposed to [Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html#working-with-playbooks)), pay particular attention to shell quoting rules, so the local shell retains the variable and passes it to Ansible. For example, using double rather than single quotes in the above example would evaluate the variable on the box you were on.



### [Managing files](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id7)

An ad-hoc task can harness the power of Ansible and SCP to transfer many files to multiple machines in parallel. To transfer a file directly to all servers in the [atlanta] group:

```
$ ansible atlanta -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```

If you plan to repeat a task like this, use the [template](https://docs.ansible.com/ansible/latest/modules/template_module.html#template-module) module in a playbook.

The [file](https://docs.ansible.com/ansible/latest/modules/file_module.html#file-module) module allows changing ownership and permissions on files. These same options can be passed directly to the `copy` module as well:

```
$ ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"
$ ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```

The `file` module can also create directories, similar to `mkdir -p`:

```
$ ansible webservers -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
```

As well as delete directories (recursively) and delete files:

```
$ ansible webservers -m file -a "dest=/path/to/c state=absent"
```



### [Managing packages](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id8)

You might also use an ad-hoc task to install, update, or remove packages on managed nodes using a package management module like yum. To ensure a package is installed without updating it:

```
$ ansible webservers -m yum -a "name=acme state=present"
```

To ensure a specific version of a package is installed:

```
$ ansible webservers -m yum -a "name=acme-1.5 state=present"
```

To ensure a package is at the latest version:

```
$ ansible webservers -m yum -a "name=acme state=latest"
```

To ensure a package is not installed:

```
$ ansible webservers -m yum -a "name=acme state=absent"
```

Ansible has modules for managing packages under many platforms. If there is no module for your package manager, you can install packages using the command module or create a module for your package manager.



### [Managing users and groups](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id9)

You can create, manage, and remove user accounts on your managed nodes with ad-hoc tasks:

```
$ ansible all -m user -a "name=foo password=<crypted password here>"

$ ansible all -m user -a "name=foo state=absent"
```

See the [user](https://docs.ansible.com/ansible/latest/modules/user_module.html#user-module) module documentation for details on all of the available options, including how to manipulate groups and group membership.



### [Managing services](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id10)

Ensure a service is started on all webservers:

```
$ ansible webservers -m service -a "name=httpd state=started"
```

Alternatively, restart a service on all webservers:

```
$ ansible webservers -m service -a "name=httpd state=restarted"
```

Ensure a service is stopped:

```
$ ansible webservers -m service -a "name=httpd state=stopped"
```



### [Gathering facts](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#id11)

Facts represent discovered variables about a system. You can use facts to implement conditional execution of tasks but also just to get ad-hoc information about your systems. To see all facts:

```
$ ansible all -m setup
```

You can also filter this output to display only certain facts, see the [setup](https://docs.ansible.com/ansible/latest/modules/setup_module.html#setup-module) module documentation for details.

Now that you understand the basic elements of Ansible execution, you are ready to learn to automate repetitive tasks using [Ansible Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro).