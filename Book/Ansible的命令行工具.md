# Working with command line tools

ansible和ansible-playbook是我们接触最多命令，但这只是Ansible部分的功能而已。Ansible还有众多命令，下面我们一一介绍，并附上一些实践的例子，体会Ansible的强大。

- [ansible](https://docs.ansible.com/ansible/latest/cli/ansible.html)
- [ansible-config](https://docs.ansible.com/ansible/latest/cli/ansible-config.html)
- [ansible-console](https://docs.ansible.com/ansible/latest/cli/ansible-console.html)
- [ansible-doc](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html)
- [ansible-galaxy](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html)
- [ansible-inventory](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html)
- [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)
- [ansible-pull](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html)
- [ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)



## ansible 命令

ansible 命令用于对一组特定的主机运行单个任务，是用于执行“远程操作”的最简单命令。它有以下参数：

```shell
usage: ansible [-h] [--version] [-v] [-b] [--become-method BECOME_METHOD]
            [--become-user BECOME_USER] [-K] [-i INVENTORY] [--list-hosts]
            [-l SUBSET] [-P POLL_INTERVAL] [-B SECONDS] [-o] [-t TREE] [-k]
            [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
            [-c CONNECTION] [-T TIMEOUT]
            [--ssh-common-args SSH_COMMON_ARGS]
            [--sftp-extra-args SFTP_EXTRA_ARGS]
            [--scp-extra-args SCP_EXTRA_ARGS]
            [--ssh-extra-args SSH_EXTRA_ARGS] [-C] [--syntax-check] [-D]
            [-e EXTRA_VARS] [--vault-id VAULT_IDS]
            [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
            [-f FORKS] [-M MODULE_PATH] [--playbook-dir BASEDIR]
            [-a MODULE_ARGS] [-m MODULE_NAME]
            pattern
```

下面详细介绍每个参数的含义：

- `--ask-vault-pass`

  ask for vault password

  执行命令时要求提供vault密码

- `--become-method <BECOME_METHOD>`

  privilege escalation method to use (default=%(default)s), use ansible-doc -t become -l to list valid choices.

  指定使用become时使用哪种提升权限的方法(default =％(default)s)，使用`ansible-doc -t become -l`可以查看由哪些方法：

  | 选项       | 英文解释                                           | 中文解释 |
  | ---------- | -------------------------------------------------- | -------- |
  | doas       | Do As user                                         |          |
  | dzdo       | Centrify's Direct Authorize                        |          |
  | enable     | Switch to elevated permissions on a network device |          |
  | ksu        | Kerberos substitute user                           |          |
  | machinectl | Systemd's machinectl privilege escalation          |          |
  | pbrun      | PowerBroker run                                    |          |
  | pfexec     | profile based execution                            |          |
  | pmrun      | Privilege Manager run                              |          |
  | runas      | Run As user                                        |          |
  | sesu       | CA Privileged Access Manager                       |          |
  | su         | Substitute User                                    |          |
  | sudo       | Substitute User DO                                 |          |

- `--become-user <BECOME_USER>`

  run operations as this user (default=root)

  指定运行命令的用户(默认= root)

- `--list-hosts`

  outputs a list of matching hosts; does not execute anything else

  输出匹配的主机，不执行其他任何操作

- `--playbook-dir <BASEDIR>`

  Since this tool does not use playbooks, use this as a substitute playbook directory.This sets the relative path for many features including roles/ group_vars/ etc.

  由于此工具不使用剧本，因此可以将其用作替代剧本目录，从而为许多功能(包括role / group_vars /etc)设置相对路径。

- `--private-key <PRIVATE_KEY_FILE>, --key-file <PRIVATE_KEY_FILE>`

  use this file to authenticate the connection

  使用私钥文件来验证连接

- `--scp-extra-args <SCP_EXTRA_ARGS>`

  specify extra arguments to pass to scp only (e.g. -l)

  执行scp命令时，指定额外的参数，例如(-l)

- `--sftp-extra-args <SFTP_EXTRA_ARGS>`

  specify extra arguments to pass to sftp only (e.g. -f, -l)

  执行sftp命令时，指定额外的参数(例如-f，-l)

- `--ssh-common-args <SSH_COMMON_ARGS>`

  specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

  执行 sftp/scp/ssh命令时，指定要传递给sftp / scp / ssh的参数(例如ProxyCommand)

- `--ssh-extra-args <SSH_EXTRA_ARGS>`

  specify extra arguments to pass to ssh only (e.g. -R)

  执行ssh命令时，指定额外的参数(例如-R)

- `--syntax-check`

  perform a syntax check on the playbook, but do not execute it

  仅对playbook进行语法检查，不是真实执行

- `--vault-id`

  the vault identity to use

  指定 vault的id

- `--vault-password-file`

  vault password file

  指定 vault的密码文件

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

  显示程序的版本号，配置文件位置，配置的模块搜索路径，模块位置，可执行文件位置，然后退出

- `-B <SECONDS>, --background <SECONDS>`

  run asynchronously, failing after X seconds (default=N/A)

  异步运行，在X秒后失败(默认值= N / A)，默认失败，立即返回

- `-C, --check`

  don’t make any changes; instead, try to predict some of the changes that may occur

  不进行任何更改； 相反，尝试预测可能发生的某些变化

- `-D, --diff`

  when changing (small) files and templates, show the differences in those files; works great with –check

  更改(小的)文件和模板时，显示这些文件的差异； 与`–check`命令一起使用效果更好

- `-K, --ask-become-pass`

  ask for privilege escalation password

  使用become提升权限时要求输入的密码

- `-M, --module-path`

  prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)

  指定模块的路径，多个路径使用冒号分隔。默认模块库(默认=〜/ .ansible / plugins / modules：/ usr / share / ansible / plugins / modules)

- `-P <POLL_INTERVAL>, --poll <POLL_INTERVAL>`

  set the poll interval if using -B (default=15)

  如果使用-B，则设置轮询间隔(默认值= 15)

- `-T <TIMEOUT>, --timeout <TIMEOUT>`

  override the connection timeout in seconds (default=10)

  设置连接超时时间(以秒为单位)(默认为10)

- `-a <MODULE_ARGS>, --args <MODULE_ARGS>`

  module arguments

  使用模块时，设置模块的参数

- `-b, --become`

  run operations with become (does not imply password prompting)

  使用become提升权限(不提示输入密码)

- `-c <CONNECTION>, --connection <CONNECTION>`

  connection type to use (default=smart)

  要使用的连接类型(默认=智能)

  --connection=type:指定远程连接主机的方式，默认是ssh，设置为local时，则只在本地执行playbook

- `-e, --extra-vars`

  set additional variables as key=value or YAML/JSON, if filename prepend with @

  通过key=value 或者 YAML/JSON语法设置变量，如果使用文件，文件名以@开头

- `-f <FORKS>, --forks <FORKS>`

  specify number of parallel processes to use (default=5)

  指定并发进程数(默认= 5)

- `-h, --help`

  show this help message and exit

  显示帮助消息

- `-i, --inventory, --inventory-file`

  specify inventory host path or comma separated host list. –inventory-file is deprecated

  指定主机清单，多个文件用逗号分隔

- `-k, --ask-pass`

  ask for connection password

  询问连接密码

- `-l <SUBSET>, --limit <SUBSET>`

  further limit selected hosts to an additional pattern

  将所选主机进一步限制为其他模式

- `-m <MODULE_NAME>, --module-name <MODULE_NAME>`

  module name to execute (default=command)

  要执行的模块名称(默认=command模块)

- `-o, --one-line`

  condense output

  冷凝输出

- `-t <TREE>, --tree <TREE>`

  log output to this directory

  指定日志输出目录

- `-u <REMOTE_USER>, --user <REMOTE_USER>`

  connect as this user (default=None)

  以该用户身份连接(默认=None)

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

  详细模式(-vvv用于更多，-vvvv用于启用连接调试)

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible.html#id5)

ansible.cfg中的大多数选项都可以使用更多功能

/etc/ansible/ansible.cfg –配置文件(如果存在)

〜/ .ansible.cfg –用户配置文件，覆盖默认配置(如果存在)

ANSIBLE_CONFIG –覆盖默认的ansible配置文件

## 命令使用实例：

- ping slave组

ansible slave -m ping

- 用bruce 用户以root 身份ping

ansible slave -m ping -u bruce --sudo

- 用bruce 用户sudo 到batman 用户ping

ansible slave -m ping -u bruce --sudo --sudo-user batman

- 给slave组安装ftp

ansible slave -m yum -a "name=vsftpd state=latest"
ansible slave -m yum -a 'name=vsftpd state=present'

- 启动ftp

ansible slave -m service -a 'name=vsftpd state=started enabled=yes'

- 查看ftp是否启动

ansible slave -m shell -a 'ss -tln | grep 21'

- 执行shell脚本文件

ansible slave  -m shell -a "/tmp/test.sh"

- 执行update命令

ansible slave -m command -a 'uptime'

- 创建用户hadoop

ansible slave -m user -a 'name=hadoop comment="ansible add user" password="123123"'

- 复制文件

ansible slave -m copy -a 'src=/root/.ssh/id_rsa.pub dest=/root'

- 追加文件

ansible slave -m shell -a 'cat /root/id_rsa.pub >>/root/.ssh/authorized_keys'

- 确保slave 组所有主机的httpd 是启动的

ansible slave -m service -a "name=httpd state=started"

- 重启slave 组所有主机的httpd 服务

ansible slave -m service -a "name=httpd state=restarted"

- 确保slave 组所有主机的httpd 是关闭的

ansible slave -m service -a "name=httpd state=stopped"



# ansible-config

**View ansible configuration.**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#common-options)
- Actions
  - [list](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#list)
  - [dump](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#dump)
  - [view](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#view)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id2)

```
usage: ansible-config [-h] [--version] [-v] {list,dump,view} ...
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id3)

Config command line class

一个配置命令行类

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id4)

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-h, --help`

  show this help message and exit

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Actions](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id5)



### [list](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id6)

list all current configs reading lib/constants.py and shows env and config file setting names

读取lib/constants.py文件，展示当前配置。另外也会展示环境变量和配置文件配置的名字。

- `-c <CONFIG_FILE>, --config <CONFIG_FILE>`

  path to configuration file, defaults to first file found in precedence.

指定配置文件的路径，默认是第一个文件

### [dump](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id7)

Shows the current settings, merges ansible.cfg if specified

- `--only-changed`

  Only show configurations that have changed from the default

- `-c <CONFIG_FILE>, --config <CONFIG_FILE>`

  path to configuration file, defaults to first file found in precedence.



### [view](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id8)

Displays the current config file

展示目前的配置文件。

- `-c <CONFIG_FILE>, --config <CONFIG_FILE>`

  path to configuration file, defaults to first file found in precedence.

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id9)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#id10)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-console

**REPL console for executing Ansible tasks.**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#common-options)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#id2)

```
usage: ansible-console [-h] [--version] [-v] [-b]
                    [--become-method BECOME_METHOD]
                    [--become-user BECOME_USER] [-K] [-i INVENTORY]
                    [--list-hosts] [-l SUBSET] [-k]
                    [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
                    [-c CONNECTION] [-T TIMEOUT]
                    [--ssh-common-args SSH_COMMON_ARGS]
                    [--sftp-extra-args SFTP_EXTRA_ARGS]
                    [--scp-extra-args SCP_EXTRA_ARGS]
                    [--ssh-extra-args SSH_EXTRA_ARGS] [-C] [--syntax-check]
                    [-D] [--vault-id VAULT_IDS]
                    [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
                    [-f FORKS] [-M MODULE_PATH] [--playbook-dir BASEDIR]
                    [--step]
                    [pattern]
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#id3)

a REPL that allows for running ad-hoc tasks against a chosen inventory (based on dominis’ ansible-shell).

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#id4)

- `--ask-vault-pass`

  ask for vault password

- `--become-method <BECOME_METHOD>`

  privilege escalation method to use (default=%(default)s), use ansible-doc -t become -l to list valid choices.

- `--become-user <BECOME_USER>`

  run operations as this user (default=root)

- `--list-hosts`

  outputs a list of matching hosts; does not execute anything else

- `--playbook-dir <BASEDIR>`

  Since this tool does not use playbooks, use this as a substitute playbook directory.This sets the relative path for many features including roles/ group_vars/ etc.

- `--private-key <PRIVATE_KEY_FILE>, --key-file <PRIVATE_KEY_FILE>`

  use this file to authenticate the connection

- `--scp-extra-args <SCP_EXTRA_ARGS>`

  specify extra arguments to pass to scp only (e.g. -l)

- `--sftp-extra-args <SFTP_EXTRA_ARGS>`

  specify extra arguments to pass to sftp only (e.g. -f, -l)

- `--ssh-common-args <SSH_COMMON_ARGS>`

  specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

- `--ssh-extra-args <SSH_EXTRA_ARGS>`

  specify extra arguments to pass to ssh only (e.g. -R)

- `--step`

  one-step-at-a-time: confirm each task before running

- `--syntax-check`

  perform a syntax check on the playbook, but do not execute it

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-C, --check`

  don’t make any changes; instead, try to predict some of the changes that may occur

- `-D, --diff`

  when changing (small) files and templates, show the differences in those files; works great with –check

- `-K, --ask-become-pass`

  ask for privilege escalation password

- `-M, --module-path`

  prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)

- `-T <TIMEOUT>, --timeout <TIMEOUT>`

  override the connection timeout in seconds (default=10)

- `-b, --become`

  run operations with become (does not imply password prompting)

- `-c <CONNECTION>, --connection <CONNECTION>`

  connection type to use (default=smart)

- `-f <FORKS>, --forks <FORKS>`

  specify number of parallel processes to use (default=5)

- `-h, --help`

  show this help message and exit

- `-i, --inventory, --inventory-file`

  specify inventory host path or comma separated host list. –inventory-file is deprecated

- `-k, --ask-pass`

  ask for connection password

- `-l <SUBSET>, --limit <SUBSET>`

  further limit selected hosts to an additional pattern

- `-u <REMOTE_USER>, --user <REMOTE_USER>`

  connect as this user (default=None)

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#id5)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-console.html#id6)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-doc

**plugin documentation tool**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#common-options)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#id2)

```
usage: ansible-doc [-h] [--version] [-v] [-M MODULE_PATH]
                [--playbook-dir BASEDIR]
                [-t {become,cache,callback,cliconf,connection,httpapi,inventory,lookup,netconf,shell,module,strategy,vars}]
                [-j] [-F | -l | -s | --metadata-dump]
                [plugin [plugin ...]]
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#id3)

displays information on modules installed in Ansible libraries. It displays a terse listing of plugins and their short descriptions, provides a printout of their DOCUMENTATION strings, and it can create a short “snippet” which can be pasted into a playbook.

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#id4)

- `--metadata-dump`

  **For internal testing only** Dump json metadata for all plugins.

- `--playbook-dir <BASEDIR>`

  Since this tool does not use playbooks, use this as a substitute playbook directory.This sets the relative path for many features including roles/ group_vars/ etc.

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-F, --list_files`

  Show plugin names and their source files without summaries (implies –list)

- `-M, --module-path`

  prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)

- `-h, --help`

  show this help message and exit

- `-j, --json`

  Change output into json format.

- `-l, --list`

  List available plugins

- `-s, --snippet`

  Show playbook snippet for specified plugin(s)

- `-t <TYPE>, --type <TYPE>`

  Choose which plugin type (defaults to “module”). Available plugin types are : (‘become’, ‘cache’, ‘callback’, ‘cliconf’, ‘connection’, ‘httpapi’, ‘inventory’, ‘lookup’, ‘netconf’, ‘shell’, ‘module’, ‘strategy’, ‘vars’)

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#id5)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html#id6)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-galaxy

**Perform various Role and Collection related operations.**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#common-options)
- Actions
  - collection
    - [collection init](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#collection-init)
    - [collection build](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#collection-build)
    - [collection publish](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#collection-publish)
    - [collection install](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#collection-install)
  - role
    - [role init](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-init)
    - [role remove](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-remove)
    - [role delete](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-delete)
    - [role list](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-list)
    - [role search](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-search)
    - [role import](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-import)
    - [role setup](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-setup)
    - [role login](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-login)
    - [role info](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-info)
    - [role install](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#role-install)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id2)

```
usage: ansible-galaxy [-h] [--version] [-v] TYPE ...
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id3)

command to manage Ansible roles in shared repositories, the default of which is Ansible Galaxy *https://galaxy.ansible.com*.

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id4)

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-h, --help`

  show this help message and exit

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Actions](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id5)



### [collection](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id6)

Perform the action on an Ansible Galaxy collection. Must be combined with a further action like init/install as listed below.



#### [collection init](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id7)

Creates the skeleton framework of a role or collection that complies with the Galaxy metadata format. Requires a role or collection name. The collection name must be in the format `<namespace>.<collection>`.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--collection-skeleton <COLLECTION_SKELETON>`

  The path to a collection skeleton that the new collection should be based upon.

- `--init-path <INIT_PATH>`

  The path in which the skeleton collection will be created. The default is the current working directory.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-f, --force`

  Force overwriting an existing role or collection

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [collection build](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id8)

Build an Ansible Galaxy collection artifact that can be stored in a central repository like Ansible Galaxy. By default, this command builds from the current working directory. You can optionally pass in the collection input path (where the `galaxy.yml` file is).

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--output-path <OUTPUT_PATH>`

  The path in which the collection is built to. The default is the current working directory.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-f, --force`

  Force overwriting an existing role or collection

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [collection publish](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id9)

Publish a collection into Ansible Galaxy. Requires the path to the collection tarball to publish.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--import-timeout <IMPORT_TIMEOUT>`

  The time to wait for the collection import process to finish.

- `--no-wait`

  Don’t wait for import validation results.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [collection install](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id10)

Install one or more roles(`ansible-galaxy role install`), or one or more collections(`ansible-galaxy collection install`). You can pass in a list (roles or collections) or use the file option listed below (these are mutually exclusive). If you pass in a list, it can be a name (which will be downloaded via the galaxy API and github), or it can be a local tar archive file.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--force-with-deps`

  Force overwriting an existing collection and its dependencies.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-f, --force`

  Force overwriting an existing role or collection

- `-i, --ignore-errors`

  Ignore errors during installation and continue with the next specified collection. This will not ignore dependency conflict errors.

- `-n, --no-deps`

  Don’t download collections listed as dependencies.

- `-p <COLLECTIONS_PATH>, --collections-path <COLLECTIONS_PATH>`

  The path to the directory containing your collections.

- `-r <REQUIREMENTS>, --requirements-file <REQUIREMENTS>`

  A file containing a list of collections to be installed.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



### [role](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id11)

Perform the action on an Ansible Galaxy role. Must be combined with a further action like delete/install/init as listed below.



#### [role init](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id12)

Creates the skeleton framework of a role or collection that complies with the Galaxy metadata format. Requires a role or collection name. The collection name must be in the format `<namespace>.<collection>`.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--init-path <INIT_PATH>`

  The path in which the skeleton role will be created. The default is the current working directory.

- `--offline`

  Don’t query the galaxy API when creating roles

- `--role-skeleton <ROLE_SKELETON>`

  The path to a role skeleton that the new role should be based upon.

- `--type <ROLE_TYPE>`

  Initialize using an alternate role type. Valid types include: ‘container’, ‘apb’ and ‘network’.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-f, --force`

  Force overwriting an existing role or collection

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role remove](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id13)

removes the list of roles passed as arguments from the local system.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-p, --roles-path`

  The path to the directory containing your roles. The default is the first writable one configured via DEFAULT_ROLES_PATH: ~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role delete](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id14)

Delete a role from Ansible Galaxy.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role list](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id15)

lists the roles installed on the local system or matches a single role passed as an argument.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-p, --roles-path`

  The path to the directory containing your roles. The default is the first writable one configured via DEFAULT_ROLES_PATH: ~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role search](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id16)

searches for roles on the Ansible Galaxy server

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--author <AUTHOR>`

  GitHub username

- `--galaxy-tags <GALAXY_TAGS>`

  list of galaxy tags to filter by

- `--platforms <PLATFORMS>`

  list of OS platforms to filter by

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role import](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id17)

used to import a role into Ansible Galaxy

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--branch <REFERENCE>`

  The name of a branch to import. Defaults to the repository’s default branch (usually master)

- `--no-wait`

  Don’t wait for import results.

- `--role-name <ROLE_NAME>`

  The name the role should have, if different than the repo name

- `--status`

  Check the status of the most recent import request for given github_user/github_repo.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role setup](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id18)

Setup an integration from Github or Travis for Ansible Galaxy roles

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--list`

  List all of your integrations.

- `--remove <REMOVE_ID>`

  Remove the integration matching the provided ID value. Use –list to see ID values.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-p, --roles-path`

  The path to the directory containing your roles. The default is the first writable one configured via DEFAULT_ROLES_PATH: ~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role login](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id19)

verify user’s identify via Github and retrieve an auth token from Ansible Galaxy.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--github-token <TOKEN>`

  Identify with github token rather than username and password.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role info](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id20)

prints out detailed information about an installed role as well as info available from the galaxy API.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--offline`

  Don’t query the galaxy API when creating roles

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-p, --roles-path`

  The path to the directory containing your roles. The default is the first writable one configured via DEFAULT_ROLES_PATH: ~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL



#### [role install](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id21)

Install one or more roles(`ansible-galaxy role install`), or one or more collections(`ansible-galaxy collection install`). You can pass in a list (roles or collections) or use the file option listed below (these are mutually exclusive). If you pass in a list, it can be a name (which will be downloaded via the galaxy API and github), or it can be a local tar archive file.

- `--api-key <API_KEY>`

  The Ansible Galaxy API key which can be found at https://galaxy.ansible.com/me/preferences. You can also use ansible-galaxy login to retrieve this key or set the token for the GALAXY_SERVER_LIST entry.

- `--force-with-deps`

  Force overwriting an existing role and its dependencies.

- `-c, --ignore-certs`

  Ignore SSL certificate validation errors.

- `-f, --force`

  Force overwriting an existing role or collection

- `-g, --keep-scm-meta`

  Use tar instead of the scm archive option when packaging the role.

- `-i, --ignore-errors`

  Ignore errors and continue with the next specified role.

- `-n, --no-deps`

  Don’t download roles listed as dependencies.

- `-p, --roles-path`

  The path to the directory containing your roles. The default is the first writable one configured via DEFAULT_ROLES_PATH: ~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles

- `-r <ROLE_FILE>, --role-file <ROLE_FILE>`

  A file containing a list of roles to be imported.

- `-s <API_SERVER>, --server <API_SERVER>`

  The Galaxy API server URL

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id22)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html#id23)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-inventory

**None**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#common-options)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#id2)

```
usage: ansible-inventory [-h] [--version] [-v] [-i INVENTORY]
                      [--vault-id VAULT_IDS]
                      [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
                      [--playbook-dir BASEDIR] [--list] [--host HOST]
                      [--graph] [-y] [--toml] [--vars] [--export]
                      [--output OUTPUT_FILE]
                      [host|group]
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#id3)

used to display or dump the configured inventory as Ansible sees it

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#id4)

- `--ask-vault-pass`

  ask for vault password

- `--export`

  When doing an –list, represent in a way that is optimized for export,not as an accurate representation of how Ansible has processed it

- `--graph`

  create inventory graph, if supplying pattern it must be a valid group name

- `--host <HOST>`

  Output specific host info, works as inventory script

- `--list`

  Output all hosts info, works as inventory script

- `--list-hosts`

  ==SUPPRESS==

- `--output <OUTPUT_FILE>`

  When doing –list, send the inventory to a file instead of to the screen

- `--playbook-dir <BASEDIR>`

  Since this tool does not use playbooks, use this as a substitute playbook directory.This sets the relative path for many features including roles/ group_vars/ etc.

- `--toml`

  Use TOML format instead of default JSON, ignored for –graph

- `--vars`

  Add vars to graph display, ignored unless used with –graph

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-h, --help`

  show this help message and exit

- `-i, --inventory, --inventory-file`

  specify inventory host path or comma separated host list. –inventory-file is deprecated

- `-l, --limit`

  ==SUPPRESS==

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

- `-y, --yaml`

  Use YAML format instead of default JSON, ignored for –graph

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#id5)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html#id6)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-playbook

**Runs Ansible playbooks, executing the defined tasks on the targeted hosts.**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#common-options)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id2)

```
usage: ansible-playbook [-h] [--version] [-v] [-k]
                     [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
                     [-c CONNECTION] [-T TIMEOUT]
                     [--ssh-common-args SSH_COMMON_ARGS]
                     [--sftp-extra-args SFTP_EXTRA_ARGS]
                     [--scp-extra-args SCP_EXTRA_ARGS]
                     [--ssh-extra-args SSH_EXTRA_ARGS] [--force-handlers]
                     [--flush-cache] [-b] [--become-method BECOME_METHOD]
                     [--become-user BECOME_USER] [-K] [-t TAGS]
                     [--skip-tags SKIP_TAGS] [-C] [--syntax-check] [-D]
                     [-i INVENTORY] [--list-hosts] [-l SUBSET]
                     [-e EXTRA_VARS] [--vault-id VAULT_IDS]
                     [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
                     [-f FORKS] [-M MODULE_PATH] [--list-tasks]
                     [--list-tags] [--step] [--start-at-task START_AT_TASK]
                     playbook [playbook ...]
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id3)

the tool to run *Ansible playbooks*, which are a configuration and multinode deployment system. See the project home page ([https://docs.ansible.com](https://docs.ansible.com/)) for more information.

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id4)

- `--ask-vault-pass`

  ask for vault password

- `--become-method <BECOME_METHOD>`

  privilege escalation method to use (default=%(default)s), use ansible-doc -t become -l to list valid choices.

- `--become-user <BECOME_USER>`

  run operations as this user (default=root)

- `--flush-cache`

  clear the fact cache for every host in inventory

- `--force-handlers`

  run handlers even if a task fails

- `--list-hosts`

  outputs a list of matching hosts; does not execute anything else

- `--list-tags`

  list all available tags

- `--list-tasks`

  list all tasks that would be executed

- `--private-key <PRIVATE_KEY_FILE>, --key-file <PRIVATE_KEY_FILE>`

  use this file to authenticate the connection

- `--scp-extra-args <SCP_EXTRA_ARGS>`

  specify extra arguments to pass to scp only (e.g. -l)

- `--sftp-extra-args <SFTP_EXTRA_ARGS>`

  specify extra arguments to pass to sftp only (e.g. -f, -l)

- `--skip-tags`

  only run plays and tasks whose tags do not match these values

- `--ssh-common-args <SSH_COMMON_ARGS>`

  specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

- `--ssh-extra-args <SSH_EXTRA_ARGS>`

  specify extra arguments to pass to ssh only (e.g. -R)

- `--start-at-task <START_AT_TASK>`

  start the playbook at the task matching this name

- `--step`

  one-step-at-a-time: confirm each task before running

- `--syntax-check`

  perform a syntax check on the playbook, but do not execute it

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-C, --check`

  don’t make any changes; instead, try to predict some of the changes that may occur

- `-D, --diff`

  when changing (small) files and templates, show the differences in those files; works great with –check

- `-K, --ask-become-pass`

  ask for privilege escalation password

- `-M, --module-path`

  prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)

- `-T <TIMEOUT>, --timeout <TIMEOUT>`

  override the connection timeout in seconds (default=10)

- `-b, --become`

  run operations with become (does not imply password prompting)

- `-c <CONNECTION>, --connection <CONNECTION>`

  connection type to use (default=smart)

- `-e, --extra-vars`

  set additional variables as key=value or YAML/JSON, if filename prepend with @

- `-f <FORKS>, --forks <FORKS>`

  specify number of parallel processes to use (default=5)

- `-h, --help`

  show this help message and exit

- `-i, --inventory, --inventory-file`

  specify inventory host path or comma separated host list. –inventory-file is deprecated

- `-k, --ask-pass`

  ask for connection password

- `-l <SUBSET>, --limit <SUBSET>`

  further limit selected hosts to an additional pattern

- `-t, --tags`

  only run plays and tasks tagged with these values

- `-u <REMOTE_USER>, --user <REMOTE_USER>`

  connect as this user (default=None)

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id5)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id6)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



# ansible-pull

**pulls playbooks from a VCS repo and executes them for the local host**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#common-options)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#id2)

```
usage: ansible-pull [-h] [--version] [-v] [-k]
                 [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
                 [-c CONNECTION] [-T TIMEOUT]
                 [--ssh-common-args SSH_COMMON_ARGS]
                 [--sftp-extra-args SFTP_EXTRA_ARGS]
                 [--scp-extra-args SCP_EXTRA_ARGS]
                 [--ssh-extra-args SSH_EXTRA_ARGS] [--vault-id VAULT_IDS]
                 [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
                 [-e EXTRA_VARS] [-t TAGS] [--skip-tags SKIP_TAGS]
                 [-i INVENTORY] [--list-hosts] [-l SUBSET] [-M MODULE_PATH]
                 [-K] [--purge] [-o] [-s SLEEP] [-f] [-d DEST] [-U URL]
                 [--full] [-C CHECKOUT] [--accept-host-key]
                 [-m MODULE_NAME] [--verify-commit] [--clean]
                 [--track-subs] [--check] [--diff]
                 [playbook.yml [playbook.yml ...]]
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#id3)

is used to up a remote copy of ansible on each managed node, each set to run via cron and update playbook source via a source repository. This inverts the default *push* architecture of ansible into a *pull* architecture, which has near-limitless scaling potential.

The setup playbook can be tuned to change the cron frequency, logging locations, and parameters to ansible-pull. This is useful both for extreme scale-out as well as periodic remediation. Usage of the ‘fetch’ module to retrieve logs from ansible-pull runs would be an excellent way to gather and analyze remote logs from ansible-pull.

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#id4)

- `--accept-host-key`

  adds the hostkey for the repo url if not already added

- `--ask-vault-pass`

  ask for vault password

- `--check`

  don’t make any changes; instead, try to predict some of the changes that may occur

- `--clean`

  modified files in the working repository will be discarded

- `--diff`

  when changing (small) files and templates, show the differences in those files; works great with –check

- `--full`

  Do a full clone, instead of a shallow one.

- `--list-hosts`

  outputs a list of matching hosts; does not execute anything else

- `--private-key <PRIVATE_KEY_FILE>, --key-file <PRIVATE_KEY_FILE>`

  use this file to authenticate the connection

- `--purge`

  purge checkout after playbook run

- `--scp-extra-args <SCP_EXTRA_ARGS>`

  specify extra arguments to pass to scp only (e.g. -l)

- `--sftp-extra-args <SFTP_EXTRA_ARGS>`

  specify extra arguments to pass to sftp only (e.g. -f, -l)

- `--skip-tags`

  only run plays and tasks whose tags do not match these values

- `--ssh-common-args <SSH_COMMON_ARGS>`

  specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

- `--ssh-extra-args <SSH_EXTRA_ARGS>`

  specify extra arguments to pass to ssh only (e.g. -R)

- `--track-subs`

  submodules will track the latest changes. This is equivalent to specifying the –remote flag to git submodule update

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

- `--verify-commit`

  verify GPG signature of checked out commit, if it fails abort running the playbook. This needs the corresponding VCS module to support such an operation

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-C <CHECKOUT>, --checkout <CHECKOUT>`

  branch/tag/commit to checkout. Defaults to behavior of repository module.

- `-K, --ask-become-pass`

  ask for privilege escalation password

- `-M, --module-path`

  prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)

- `-T <TIMEOUT>, --timeout <TIMEOUT>`

  override the connection timeout in seconds (default=10)

- `-U <URL>, --url <URL>`

  URL of the playbook repository

- `-c <CONNECTION>, --connection <CONNECTION>`

  connection type to use (default=smart)

- `-d <DEST>, --directory <DEST>`

  directory to checkout repository to

- `-e, --extra-vars`

  set additional variables as key=value or YAML/JSON, if filename prepend with @

- `-f, --force`

  run the playbook even if the repository could not be updated

- `-h, --help`

  show this help message and exit

- `-i, --inventory, --inventory-file`

  specify inventory host path or comma separated host list. –inventory-file is deprecated

- `-k, --ask-pass`

  ask for connection password

- `-l <SUBSET>, --limit <SUBSET>`

  further limit selected hosts to an additional pattern

- `-m <MODULE_NAME>, --module-name <MODULE_NAME>`

  Repository module name, which ansible will use to check out the repo. Choices are (‘git’, ‘subversion’, ‘hg’, ‘bzr’). Default is git.

- `-o, --only-if-changed`

  only run the playbook if the repository has been updated

- `-s <SLEEP>, --sleep <SLEEP>`

  sleep for random interval (between 0 and n number of seconds) before starting. This is a useful way to disperse git requests

- `-t, --tags`

  only run plays and tasks tagged with these values

- `-u <REMOTE_USER>, --user <REMOTE_USER>`

  connect as this user (default=None)

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#id5)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#id6)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present

# ansible-vault

**encryption/decryption utility for Ansible data files**

- [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#synopsis)
- [Description](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#description)
- [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#common-options)
- Actions
  - [create](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#create)
  - [decrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#decrypt)
  - [edit](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#edit)
  - [view](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#view)
  - [encrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#encrypt)
  - [encrypt_string](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#encrypt-string)
  - [rekey](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#rekey)
- [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#environment)
- [Files](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#files)
- [Author](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#author)
- [License](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#license)
- [See also](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#see-also)

## [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id2)

```
usage: ansible-vault [-h] [--version] [-v]
                  {create,decrypt,edit,view,encrypt,encrypt_string,rekey}
                  ...
```

## [Description](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id3)

can encrypt any structured data file used by Ansible. This can include *group_vars/* or *host_vars/* inventory variables, variables loaded by *include_vars* or *vars_files*, or variable files passed on the ansible-playbook command line with *-e @file.yml* or *-e @file.json*. Role variables and defaults are also included!

Because Ansible tasks, handlers, and other objects are data, these can also be encrypted with vault. If you’d like to not expose what variables you are using, you can keep an individual task file entirely encrypted.

## [Common Options](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id4)

- `--version`

  show program’s version number, config file location, configured module search path, module location, executable location and exit

- `-h, --help`

  show this help message and exit

- `-v, --verbose`

  verbose mode (-vvv for more, -vvvv to enable connection debugging)

## [Actions](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id5)



### [create](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id6)

create and open a file in an editor that will be encrypted with the provided vault secret when closed

- `--ask-vault-pass`

  ask for vault password

- `--encrypt-vault-id <ENCRYPT_VAULT_ID>`

  the vault id used to encrypt (required if more than vault-id is provided)

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file



### [decrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id7)

decrypt the supplied file using the provided vault secret

- `--ask-vault-pass`

  ask for vault password

- `--output <OUTPUT_FILE>`

  output file name for encrypt or decrypt; use - for stdout

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file



### [edit](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id8)

open and decrypt an existing vaulted file in an editor, that will be encrypted again when closed

- `--ask-vault-pass`

  ask for vault password

- `--encrypt-vault-id <ENCRYPT_VAULT_ID>`

  the vault id used to encrypt (required if more than vault-id is provided)

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file



### [view](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id9)

open, decrypt and view an existing vaulted file using a pager using the supplied vault secret

- `--ask-vault-pass`

  ask for vault password

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file



### [encrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id10)

encrypt the supplied file using the provided vault secret

- `--ask-vault-pass`

  ask for vault password

- `--encrypt-vault-id <ENCRYPT_VAULT_ID>`

  the vault id used to encrypt (required if more than vault-id is provided)

- `--output <OUTPUT_FILE>`

  output file name for encrypt or decrypt; use - for stdout

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file



### [encrypt_string](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id11)

encrypt the supplied string using the provided vault secret

- `--ask-vault-pass`

  ask for vault password

- `--encrypt-vault-id <ENCRYPT_VAULT_ID>`

  the vault id used to encrypt (required if more than vault-id is provided)

- `--output <OUTPUT_FILE>`

  output file name for encrypt or decrypt; use - for stdout

- `--stdin-name <ENCRYPT_STRING_STDIN_NAME>`

  Specify the variable name for stdin

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

- `-n, --name`

  Specify the variable name

- `-p, --prompt`

  Prompt for the string to encrypt



### [rekey](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id12)

re-encrypt a vaulted file with a new secret, the previous secret is required

- `--ask-vault-pass`

  ask for vault password

- `--encrypt-vault-id <ENCRYPT_VAULT_ID>`

  the vault id used to encrypt (required if more than vault-id is provided)

- `--new-vault-id <NEW_VAULT_ID>`

  the new vault identity to use for rekey

- `--new-vault-password-file <NEW_VAULT_PASSWORD_FILE>`

  new vault password file for rekey

- `--vault-id`

  the vault identity to use

- `--vault-password-file`

  vault password file

## [Environment](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id13)

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## [Files](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#id14)

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present

