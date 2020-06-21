# Ansible从入门到实战（4）-Ansible常用模块A篇

书接上一节，本节依然对一些常用的模块进行介绍

## 二、cron模块
- 作用：在目标主机上设置定时任务
- 使用例子：

```bash
day # 天
hour # 小时
disabled # 禁用
job # 任务
minute #分钟
month #月
name #名字，描述信息
weekday #周
user #用户
state # 删除
ansible web -m cron -a "minute=19 job='touch /tmp/alex.txt' name=touchfile"
ansible web -m cron -a "minute=19 job='touch /tmp/alex.txt' name=touchfile disabled=yes" # 表示禁用crontab，禁用的时候不可以直接指定名称
ansible web -m cron -a "name=touchfile state=absent" # 删除crontab，可以直接指定名称进行删除
```
- 使用实例：

任务于每天1点5分执行，任务内容为输出test字符

`ansible apple -m cron -a "name='crontab test' minute=5 hour=1 job='echo test'"`

结果如下：

```
[root@localhost ~]# ansible apple -m cron -a "name='crontab test' minute=5 hour=1 job='echo test'"
apple | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "envs": [],
    "jobs": [
        "crontab test"
    ]
}
```

## 三、setup模块
- 作用：用于收集远程主机的一些基本信息
- 使用例子：

```bash
ansible_all_ipv4_addresses：仅显示ipv4的信息。
ansible_devices：仅显示磁盘设备信息。
ansible_distribution：显示是什么系统，例：centos,suse等。
ansible_distribution_major_version：显示是系统主版本。
ansible_distribution_version：仅显示系统版本。
ansible_machine：显示系统类型，例：32位，还是64位。
ansible_eth0：仅显示eth0的信息。
ansible_hostname：仅显示主机名。
ansible_kernel：仅显示内核版本。
ansible_lvm：显示lvm相关信息。
ansible_memtotal_mb：显示系统总内存。
ansible_memfree_mb：显示可用系统内存。
ansible_memory_mb：详细显示内存情况。
ansible_swaptotal_mb：显示总的swap内存。
ansible_swapfree_mb：显示swap内存的可用内存。
ansible_mounts：显示系统磁盘挂载情况。
ansible_processor：显示cpu个数(具体显示每个cpu的型号)。
ansible_processor_vcpus：显示cpu个数(只显示总的个数)。
```
- 使用实例：

`ansible apple -m setup -a 'filter=ansible_all_ipv4_addresses'`

结果如下：

```
[root@localhost ~]# ansible apple -m setup -a 'filter=ansible_all_ipv4_addresses'
apple | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "172.17.0.1",
            "192.168.153.129"
        ],
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false
}
```

## 四、synchronize模块
- 作用：把本地的目录和远程主机目录同步

- 参数：

```bash
archive: 归档，相当于同时开启recursive(递归)、links、perms、times、owner、group、-D选项都为yes ，默认该项为开启
checksum: 跳过检测sum值，默认关闭
compress:是否开启压缩
copy_links：复制链接文件，默认为no ，注意后面还有一个links参数
delete: 删除不存在的文件，默认no
dest：目录路径
dest_port：默认目录主机上的端口 ，默认是22，走的ssh协议
dirs：传速目录不进行递归，默认为no，即进行目录递归
rsync_opts：rsync参数部分
set_remote_user：主要用于/etc/ansible/hosts中定义或默认使用的用户与rsync使用的用户不同的情况
mode: push或pull 模块，push模的话，一般用于从本机向远程主机上传文件，pull 模式用于从远程主机上取文件

src=some/relative/path dest=/some/absolute/path rsync_path="sudo rsync"
src=some/relative/path dest=/some/absolute/path archive=no links=yes
src=some/relative/path dest=/some/absolute/path checksum=yes times=no
src=/tmp/helloworld dest=/var/www/helloword rsync_opts=--no-motd,--exclude=.git mode=pull

```
- 使用实例：

执行以下命令
`ansible apple -m synchronize -a "src=/tmp/ dest=/tpm/ archive=yes"`

结果如下：

```
[root@localhost ~]# ansible apple -m synchronize -a "src=/tmp/ dest=/tpm/ archive=yes"
apple | CHANGED => {
    "changed": true,
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /tmp/ 192.168.153.129:/tpm/",
    "msg": "cd+++++++++ ./\n<f+++++++++ docker-compose\n<f+++++++++ docker-compose.tgz\n<f+++++++++ yum_save_tx.2019-10-07.18-59.j7TBTQ.yumtx\n<f+++++++++ yum_save_tx.2019-10-07.19-04.jedVau.yumtx\n<f+++++++++ yum_save_tx.2019-10-07.19-17.htCrzX.yumtx\ncd+++++++++ .ICE-unix/\ncd+++++++++ .Test-unix/\ncd+++++++++ .X11-unix/\ncd+++++++++ .XIM-unix/\ncd+++++++++ .font-unix/\ncd+++++++++ ansible_synchronize_payload_TrcwGz/\n<f+++++++++ ansible_synchronize_payload_TrcwGz/__main__.py\n<f+++++++++ ansible_synchronize_payload_TrcwGz/__main__.pyc\n<f+++++++++ ansible_synchronize_payload_TrcwGz/ansible_synchronize_payload.zip\ncd+++++++++ apple/\ncd+++++++++ apple/var/\ncd+++++++++ apple/var/log/\n<f+++++++++ apple/var/log/cron\ncd+++++++++ systemd-private-4cfb5ddcbdbc47ee90ff96584630ca76-accessgate.service-MIH4hh/\ncd+++++++++ systemd-private-4cfb5ddcbdbc47ee90ff96584630ca76-accessgate.service-MIH4hh/tmp/\ncd+++++++++ vmware-root_735-4257003928/\ncd+++++++++ vmware-root_740-2999460834/\ncd+++++++++ vmware-root_742-2991137376/\n",
    "rc": 0,
    "stdout_lines": [
        "cd+++++++++ ./",
        "<f+++++++++ docker-compose",
        "<f+++++++++ docker-compose.tgz",
        "<f+++++++++ yum_save_tx.2019-10-07.18-59.j7TBTQ.yumtx",
        "<f+++++++++ yum_save_tx.2019-10-07.19-04.jedVau.yumtx",
        "<f+++++++++ yum_save_tx.2019-10-07.19-17.htCrzX.yumtx",
        "cd+++++++++ .ICE-unix/",
        "cd+++++++++ .Test-unix/",
        "cd+++++++++ .X11-unix/",
        "cd+++++++++ .XIM-unix/",
        "cd+++++++++ .font-unix/",
        "cd+++++++++ ansible_synchronize_payload_TrcwGz/",
        "<f+++++++++ ansible_synchronize_payload_TrcwGz/__main__.py",
        "<f+++++++++ ansible_synchronize_payload_TrcwGz/__main__.pyc",
        "<f+++++++++ ansible_synchronize_payload_TrcwGz/ansible_synchronize_payload.zip",
        "cd+++++++++ apple/",
        "cd+++++++++ apple/var/",
        "cd+++++++++ apple/var/log/",
        "<f+++++++++ apple/var/log/cron",
        "cd+++++++++ systemd-private-4cfb5ddcbdbc47ee90ff96584630ca76-accessgate.service-MIH4hh/",
        "cd+++++++++ systemd-private-4cfb5ddcbdbc47ee90ff96584630ca76-accessgate.service-MIH4hh/tmp/",
        "cd+++++++++ vmware-root_735-4257003928/",
        "cd+++++++++ vmware-root_740-2999460834/",
        "cd+++++++++ vmware-root_742-2991137376/"
    ]
}

```

## 五、mount模块
- 作用：管理挂载

- 使用例子：

```bash
dump ：默认为0
fstype：必选项，挂载文件的类型
name：必选项，挂载点
opts：传递给mount命令的参数
src：必选项，要挂载的文件
state：必选项 present：只处理fstab中的配置  absent：删除挂载点 mounted：自动创建挂载点并挂载之 umounted：卸载

name=/mnt/dvd src=/dev/sr0 fstype=iso9660 opts=ro state=present
name=/srv/disk src='LABEL=SOME_LABEL' state=present
name=/home src='UUID=b3e48f45-f933-4c8e-a700-22a159ec9077' opts=noatime state=present
ansible apple -a 'dd if=/dev/zero of=/disk.img bs=4k count=1024'
ansible apple -a 'losetup /dev/loop0 /disk.img'
ansible apple -m filesystem 'fstype=ext4 force=yes opts=-F dev=/dev/loop0'
ansible apple -m mount 'name=/mnt src=/dev/loop0 fstype=ext4 state=mounted opts=rw'
```
- 使用实例：

`ansible apple -m copy -a "src=/etc/init.d dest=/tmp"`

结果如下：

```

```

get_url模块
apt模块
template文件模块
mysql_db、redis数据库模块
url 网络模块
pip 模块
7、raw
　　用法和shell 模块一样 ，其也可以执行任意命令，就像在本机执行一样。

　　raw模块和comand、shell 模块不同的是其没有chdir、creates、removes参数

filesystem模块
git模块
unarchive模块


## 六、fetch模块
- 作用：从远程主机中拉取文件到 ansible 管理主机

- 使用例子：

```bash
dest #目标地址
src # 源地址
ansible db -m fetch -a "dest=/tmp src=/var/log/cron" # 复制远程被管控机器的文件道管控机器上，以被管控机的ip为目录，并保留原来的目录结构
```
- 使用实例：

执行以下命令，把文件fetch到本地主机

`ansible apple -m fetch -a "dest=/tmp src=/var/log/cron"`

结果如下：

```
[root@localhost ~]# ansible apple -m fetch -a "dest=/tmp src=/var/log/cron"
apple | CHANGED => {
    "changed": true,
    "checksum": "f64864c44ef11f161332f5bb2aa7cde186ecebb8",
    "dest": "/tmp/apple/var/log/cron",
    "md5sum": "f6ff5ee1d2700ef7b9f517c83fa48d22",
    "remote_checksum": "f64864c44ef11f161332f5bb2aa7cde186ecebb8",
    "remote_md5sum": null
}
```

## 七、file模块
- 作用：管理文件和文件属性

- 使用例子：

```bash
access_time # 创建时间
group # 属组
mode # 权限
owner # 属主
path # 文件的路径
src # 源地址，只有在软连接和硬链接的时候才会使用
state # directory 目录 touch 文件  link 软连接 hard 硬链接 absent 删除
ansible db -m file -a "path=/alex state=directory" # 创建一个目录
ansible db -m file -a "path=/root/alex.txt state=touch" # 创建一个文件
ansible db -m file -a "src=/root/q.txt path=/tmp/q state=link" # 创建软连接，源地址是本机上的文件地址
ansible db -m file -a "path=/tmp/q state=absent" # 删除文件或者文件夹
```
- 使用实例：

执行以下命令，把文件fetch到本地主机

``

结果如下：

```

```

## 八、yum模块
- 作用：管理包工具

- 使用例子：

```bash
disable_gpg_check # 禁止检查gpgcheck
disablerepo # 禁用repo源
enablerepo # 启用repo源
name #包的名称
state  remove 卸载 install 安装
ansible web -m yum -a 'name=python2-pip' # 安装python2-pip
ansible web -m yum -a 'name=python2-pip,redis' #用来安装多个包
ansible web -m yum -a 'name="@Development Tools"' # 用来安装包组
```
- 使用实例：

执行以下命令

`ansible apple -m yum -a 'name=httpd state=absent'`

结果如下：

```
[root@localhost ~]# ansible apple -m yum -a 'name=httpd state=absent'
apple | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "httpd is not installed"
    ]
}
```

从返回结果可以看出我的管理节点上并没有安装httpd

## 九、service模块
- 作用：管理服务

- 使用例子：

```bash
name # 包名
enabled # 开机自启动
state  started|stopped|reloaded|restarted
ansible web -m service -a 'name=redis state=started enabled=yes' #启动redis，并设置开机自启动
ansible web -m service -a 'name=redis state=stopped' #关闭redis
```
- 使用实例：

执行以下命令

`ansible apple -m service -a 'name=redis state=started enabled=yes'`

结果如下：

```
[root@localhost ~]# ansible apple -m service -a 'name=redis state=started enabled=yes'
apple | FAILED! => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "msg": "Could not find the requested service redis: host"
}
```

## 十、user模块
- 作用：管理用户和用户组

- 使用例子：

```bash
group #组名
groups # 附加组
home # 家目录位置
remove #删除用户的家目录
shell # 用户登录的shell
system # 系统用户
uid # 用户id
 ansible db -m user -a "uid=500 system=yes groups=root name=mysql" # 创建mysql用户，指定用户为系统用户，并指定uid为500，指定附加组为root
 ansible db -m user -a "uid=3000 groups=mysql name=canglaoshi home=/opt/canglaoshi shell=/sbin/nologin" # 创建普通用户，并制定用户的家目录和登录shell以及uid，附加组
 ansible db -m user -a "name=canglaoshi remove=yes state=absent" # 删除用户
```
- 使用实例：

执行以下命令

`ansible apple -m user -a "name=canglaoshi remove=yes state=absent"`

结果如下：

```
[root@localhost ~]# ansible apple -m user -a "name=canglaoshi remove=yes state=absent"
apple | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "name": "canglaoshi",
    "state": "absent"
}
```

## 十一、下一节是？

下一节继续介绍常用的Ansible模块
