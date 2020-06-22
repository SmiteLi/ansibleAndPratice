# Working With Modules

- [Introduction to modules](https://docs.ansible.com/ansible/latest/user_guide/modules_intro.html)
- [Return Values](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html)
- [Module Maintenance & Support](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html)
- [Module Index](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html)

Ansible ships with a number of modules (called the ‘module library’) that can be executed directly on remote hosts or through [Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html#working-with-playbooks).

Users can also write their own modules. These modules can control system resources, like services, packages, or files (anything really), or handle executing system commands.

# Introduction to modules

Modules (also referred to as “task plugins” or “library plugins”) are discrete units of code that can be used from the command line or in a playbook task. Ansible executes each module, usually on the remote target node, and collects return values.

You can execute modules from the command line:

```
ansible webservers -m service -a "name=httpd state=started"
ansible webservers -m ping
ansible webservers -m command -a "/sbin/reboot -t now"
```

Each module supports taking arguments. Nearly all modules take `key=value` arguments, space delimited. Some modules take no arguments, and the command/shell modules simply take the string of the command you want to run.

From playbooks, Ansible modules are executed in a very similar way:

```
- name: reboot the servers
  action: command /sbin/reboot -t now
```

Which can be abbreviated to:

```
- name: reboot the servers
  command: /sbin/reboot -t now
```

Another way to pass arguments to a module is using YAML syntax also called ‘complex args’

```
- name: restart webserver
  service:
    name: httpd
    state: restarted
```

All modules return JSON format data. This means modules can be written in any programming language. Modules should be idempotent, and should avoid making any changes if they detect that the current state matches the desired final state. When used in an Ansible playbook, modules can trigger ‘change events’ in the form of notifying ‘handlers’ to run additional tasks.

Documentation for each module can be accessed from the command line with the ansible-doc tool:

```
ansible-doc yum
```

For a list of all available modules, see the [Module Docs](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html#modules-by-category), or run the following at a command prompt:

```
ansible-doc -l
```

# [Return Values](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id1)

Topics

- Return Values
  - Common
    - [backup_file](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#backup-file)
    - [changed](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#changed)
    - [failed](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#failed)
    - [invocation](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#invocation)
    - [msg](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#msg)
    - [rc](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#rc)
    - [results](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#results)
    - [skipped](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#skipped)
    - [stderr](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#stderr)
    - [stderr_lines](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#stderr-lines)
    - [stdout](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#stdout)
    - [stdout_lines](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#stdout-lines)
  - Internal use
    - [ansible_facts](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#ansible-facts)
    - [exception](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#exception)
    - [warnings](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#warnings)
    - [deprecations](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#deprecations)

Ansible modules normally return a data structure that can be registered into a variable, or seen directly when output by the ansible program. Each module can optionally document its own unique return values (visible through ansible-doc and on the [main docsite](https://docs.ansible.com/ansible/latest/index.html#ansible-documentation)).

This document covers return values common to all modules.

Note

Some of these keys might be set by Ansible itself once it processes the module’s return information.

## [Common](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id2)

### [backup_file](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id3)

For those modules that implement backup=no|yes when manipulating files, a path to the backup file created.

### [changed](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id4)

A boolean indicating if the task had to make changes.

### [failed](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id5)

A boolean that indicates if the task was failed or not.

### [invocation](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id6)

Information on how the module was invoked.

### [msg](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id7)

A string with a generic message relayed to the user.

### [rc](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id8)

Some modules execute command line utilities or are geared for executing commands directly (raw, shell, command, etc), this field contains ‘return code’ of these utilities.

### [results](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id9)

If this key exists, it indicates that a loop was present for the task and that it contains a list of the normal module ‘result’ per item.

### [skipped](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id10)

A boolean that indicates if the task was skipped or not

### [stderr](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id11)

Some modules execute command line utilities or are geared for executing commands directly (raw, shell, command, etc), this field contains the error output of these utilities.

### [stderr_lines](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id12)

When stderr is returned we also always provide this field which is a list of strings, one item per line from the original.

### [stdout](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id13)

Some modules execute command line utilities or are geared for executing commands directly (raw, shell, command, etc). This field contains the normal output of these utilities.

### [stdout_lines](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id14)

When stdout is returned, Ansible always provides a list of strings, each containing one item per line from the original output.



## [Internal use](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id15)

These keys can be added by modules but will be removed from registered variables; they are ‘consumed’ by Ansible itself.

### [ansible_facts](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id16)

This key should contain a dictionary which will be appended to the facts assigned to the host. These will be directly accessible and don’t require using a registered variable.

### [exception](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id17)

This key can contain traceback information caused by an exception in a module. It will only be displayed on high verbosity (-vvv).

### [warnings](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id18)

This key contains a list of strings that will be presented to the user.

### [deprecations](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id19)

This key contains a list of dictionaries that will be presented to the user. Keys of the dictionaries are msg and version, values are string, value for the version key can be an empty string.

# Module Maintenance & Support

- Maintenance
  - [Core](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#core)
  - [Network](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#network)
  - [Certified](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#certified)
  - [Community](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#community)
- [Issue Reporting](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#issue-reporting)
- [Support](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#support)

## [Maintenance](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id3)

To clarify who maintains each included module, adding features and fixing bugs, each included module now has associated metadata that provides information about maintenance.

### [Core](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id4)

[Core Maintained](https://docs.ansible.com/ansible/latest/modules/core_maintained.html#core-supported) modules are maintained by the Ansible Engineering Team. These modules are integral to the basic foundations of the Ansible distribution.

### [Network](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id5)

[Network Maintained](https://docs.ansible.com/ansible/latest/modules/network_maintained.html#network-supported) modules are are maintained by the Ansible Network Team. Please note there are additional networking modules that are categorized as Certified or Community not maintained by Ansible.

### [Certified](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id6)

[Certified](https://access.redhat.com/articles/3642632) modules are maintained by Ansible Partners.

### [Community](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id7)

[Community Maintained](https://docs.ansible.com/ansible/latest/modules/community_maintained.html#community-supported) modules are submitted and maintained by the Ansible community. These modules are not maintained by Ansible, and are included as a convenience.

## [Issue Reporting](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id8)

If you believe you have found a bug in a module and are already running the latest stable or development version of Ansible, first look at the [issue tracker in the Ansible repo](https://github.com/ansible/ansible/issues) to see if an issue has already been filed. If not, please file one.

Should you have a question rather than a bug report, inquiries are welcome on the [ansible-project Google group](https://groups.google.com/forum/#!forum/ansible-project) or on Ansible’s “#ansible” channel, located on irc.freenode.net.

For development-oriented topics, use the [ansible-devel Google group](https://groups.google.com/forum/#!forum/ansible-devel) or Ansible’s #ansible and #ansible-devel channels, located on irc.freenode.net. You should also read the [Community Guide](https://docs.ansible.com/ansible/latest/community/index.html#ansible-community-guide), [Testing Ansible](https://docs.ansible.com/ansible/latest/dev_guide/testing.html#developing-testing), and the [Developer Guide](https://docs.ansible.com/ansible/latest/dev_guide/index.html#developer-guide).

The modules are hosted on GitHub in a subdirectory of the [Ansible](https://github.com/ansible/ansible/tree/devel/lib/ansible/modules) repo.

NOTE: If you have a Red Hat Ansible Automation product subscription, please follow the standard issue reporting process via the [Red Hat Customer Portal](https://access.redhat.com/).

## [Support](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#id9)

For more information on how included Ansible modules are supported by Red Hat, please refer to the following [knowledge base article](https://access.redhat.com/articles/3166901) as well as other resources on the [Red Hat Customer Portal.](https://access.redhat.com/)