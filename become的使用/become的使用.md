

# 参考资料：https://docs.ansible.com/ansible/latest/user_guide/become.html#become
## 1. add a user and set nopassword for sudo:

```bash
su root
sed -i 's|# %wheel|%wheel|g' /etc/sudoers
useradd -d /home/apple -g wheel -m apple
echo "apple:apple" | chpasswd
su apple
```

## 2. install ansible

1. install pip

```
#wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum whatprovides pip
yum install -y python2-pip-8.1.2-14.el7.noarch
```

2. set the aliyun pypi

a. 找到下列文件

```
~/.pip/pip.conf
```

b. 在上述文件中添加或修改:

```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

3. install ansible

```
pip install -U pip
pip install ansible
```

# 3. using become
ansible-playbook -i inventory my.yml \
--extra-vars 'ansible_become_pass=YOUR-PASSWORD-HERE'

or 

ansible-playbook --ask-sudo-pass -i inventory my.yml
ansible-playbook --ask-become-pass -i inventory my.yml

- use ansible-vault
ansible-vault create passwd.yml

edit passwd.yml like vi/vim, obey the yaml syntax:
```
my_cluser_sudo_pass: your_sudo_password_for_remote_servers
```

ansible-playbook -i inventory --ask-vault-pass --extra-vars '@passwd.yml' my.yml
ansible-playbook -i hosts become.yml --ask-vault-pass --extra-vars '@group_vars/host2/passwd.yml'

edit again:
ansible-vault edit passwd.yml

change password for vault:
ansible-vault rekey passwd.yml