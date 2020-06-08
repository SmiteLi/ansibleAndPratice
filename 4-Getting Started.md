# Getting Started

- Now that you have read the [installation guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installation-guide) and installed Ansible on a control node, you are ready to learn how Ansible works. A basic Ansible command or playbook:

  selects machines to execute against from inventoryconnects to those machines (or network devices, or other managed nodes), usually over SSHcopies one or more modules to the remote machines and starts execution there

Ansible can do much more, but you should understand the most common use case before exploring all the powerful configuration, deployment, and orchestration features of Ansible. This page illustrates the basic process with a simple inventory and an ad-hoc command. Once you understand how Ansible works, you can read more details about [ad-hoc commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#intro-adhoc), organize your infrastructure with [inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory), and harness the full power of Ansible with [playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro).

- Selecting machines from inventory
  - [Action: create a basic inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#action-create-a-basic-inventory)
  - [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#beyond-the-basics)
- Connecting to remote nodes
  - [Action: check your SSH connections](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#action-check-your-ssh-connections)
  - [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id1)
- Copying and executing modules
  - [Action: run your first Ansible commands](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#action-run-your-first-ansible-commands)
  - [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id2)
- [Next steps](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#next-steps)

## [Selecting machines from inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id3)

Ansible reads information about which machines you want to manage from your inventory. Although you can pass an IP address to an ad-hoc command, you need inventory to take advantage of the full flexibility and repeatability of Ansible.

### [Action: create a basic inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id4)

For this basic inventory, edit (or create) `/etc/ansible/hosts` and add a few remote systems to it. For this example, use either IP addresses or FQDNs:

```
192.0.2.50
aserver.example.org
bserver.example.org
```

### [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id5)

Your inventory can store much more than IPs and FQDNs. You can create [aliases](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory-aliases), set variable values for a single host with [host vars](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#host-variables), or set variable values for multiple hosts with [group vars](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#group-variables).



## [Connecting to remote nodes](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id6)

Ansible communicates with remote machines over the [SSH protocol](https://www.ssh.com/ssh/protocol/). By default, Ansible uses native OpenSSH and connects to remote machines using your current user name, just as SSH does.

### [Action: check your SSH connections](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id7)

Confirm that you can connect using SSH to all the nodes in your inventory using the same username. If necessary, add your public SSH key to the `authorized_keys` file on those systems.

### [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id8)

You can override the default remote user name in several ways, including: * passing the `-u` parameter at the command line * setting user information in your inventory file * setting user information in your configuration file * setting environment variables

See [Controlling how Ansible behaves: precedence rules](https://docs.ansible.com/ansible/latest/reference_appendices/general_precedence.html#general-precedence-rules) for details on the (sometimes unintuitive) precedence of each method of passing user information. You can read more about connections in [Connection methods and details](https://docs.ansible.com/ansible/latest/user_guide/connection_details.html#connections).

## [Copying and executing modules](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id9)

Once it has connected, Ansible transfers the modules required by your command or playbook to the remote machine(s) for execution.

### [Action: run your first Ansible commands](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id10)

Use the ping module to ping all the nodes in your inventory:

```
$ ansible all -m ping
```

Now run a live command on all of your nodes:

```
$ ansible all -a "/bin/echo hello"
```

You should see output for each host in your inventory, similar to this:

```
aserver.example.org | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

### [Beyond the basics](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id11)

By default Ansible uses SFTP to transfer files. If the machine or device you want to manage does not support SFTP, you can switch to SCP mode in [Configuring Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#intro-configuration). The files are placed in a temporary directory and executed from there.

If you need privilege escalation (sudo and similar) to run a command, pass the `become` flags:

```
# as bruce
$ ansible all -m ping -u bruce
# as bruce, sudoing to root (sudo is default method)
$ ansible all -m ping -u bruce --become
# as bruce, sudoing to batman
$ ansible all -m ping -u bruce --become --become-user batman
```

You can read more about privilege escalation in [Understanding privilege escalation: become](https://docs.ansible.com/ansible/latest/user_guide/become.html#become).

Congratulations! You have contacted your nodes using Ansible. You used a basic inventory file and an ad-hoc command to direct Ansible to connect to specific remote nodes, copy a module file there and execute it, and return output. You have a fully working infrastructure.

## [Next steps](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id12)

Next you can read about more real-world cases in [Introduction to ad-hoc commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#intro-adhoc), explore what you can do with different modules, or read about the Ansible [Working With Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html#working-with-playbooks) language. Ansible is not just about running commands, it also has powerful configuration management and deployment features.