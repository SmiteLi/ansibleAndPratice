# 参考资料 
https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html#playbooks-delegation

## 本地执行：
To run an entire playbook locally, just set the hosts: line to hosts: 127.0.0.1 and then run the playbook like so:

ansible-playbook playbook.yml --connection=local

Alternatively, a local connection can be used in a single playbook play, even if other plays in the playbook use the default remote connection type:

---
- hosts: 127.0.0.1
  connection: local