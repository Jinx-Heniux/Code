# Commands

常用命令

用户管理

权限管理

umask

suid

文件管理

文本编辑

vim, grep, sed&#x20;

磁盘管理

df, free, fdisk, mount, mke2fs, e2fsck&#x20;

文件系统&#x20;

压缩及归档

gzip(.gz), bzip2(.bz2), xz, zip,cpio, tar,&#x20;

Bash脚本

if, for, while, case&#x20;

配置文件 | /usr/local/bin | /etc/sudoers | /etc/shadow



## 小命令

whomai, who, w, last, lastb, lastlog, basename, hostname, whatis

```shell
basename /etc/passwd
# 直接取得路径基名(文件名)





# 如果当前主机的主机名不是 www.magedu.com，就把它改成www.magedu.com
[ `hostname` != "www.magedu.com" ] && hostname www.magedu.com

# 如果当前主机的主机名为空或者为(none)或者为localhost，就把它改为 www.magedu.com
[ -z `hostname` ] || [ `hostname` == '(none)' -o `hostname` == 'localhost' ] && hostname www.magedu.com





last -n 3
# 登录成功信息，显示最近三次的信息

lastb -n 3
# 登录失败信息

lastlog -u hadoop
# 用户hadoop最近一次登录信息






w
# Show who is logged on and what they are doing.

whatis
# display one-line manual page descriptions

who
# show who is logged on

whoami
# print effective userid
```



## 用户管理

useradd, id, finger, usermod, userdel, chsh, chfn, passwd, pwck, groupadd, groupmod, groupdel, gpasswd, newgrp, chage

```shell
finger a-user-name
# user information lookup program
# The finger displays information about the system users.






############################## passwd ##############################
echo "redhat" | passwd --stdin user3
# 从标准输入读取密码，传递密码过去。不是手动输入
# 从管道接受字符 修改密码

passwd -d user3 
# 清空user3的密码 空密码 无法登陆






############################## useradd ##############################
useradd -u 1000 user1
# 添加用户，设置属主ID为1000

useradd -g mygroup user2
# 添加用户user2给设置基本组为mygroup，如果组不存在报错

useradd -G mygroup user3
useradd -G mygroup,mygroup2 user3
tail /etc/group
# 添加user3，并设置附加组为mygroup和mygroup2

useradd -c "Tony Blare" -d /home/blare user4
# 设置注释信息 和 家目录

useradd -s /sbin/nologin user5 
# 不让使用shell 用户不能登录

useradd -r apache
# 添加系统用户apache，即使指定家目录，也不会被创建




############################## usermod ##############################
# As root, run this command to add your user "sammy" to the sudo group
usermod -G sudo sammy
usermod -aG sudo sammy
# -a 表示追加










############################## 练习 ##############################
# 创建一个用户mandriva，其ID号为2002，基本组为distro（组ID为3003），附加组为linux；
groupadd -g 3003 distro
groupadd linux
useradd -u 2002 -g distro -G linux mandriva

# 创建一个用户fedora，其全名为Fedora Community，默认shell为tcsh；
useradd -c "Fedora Community" -s /bin/tcsh fedora

# 修改mandriva的ID号为4004，基本组为linux，附加组为distro和fedora；
usermod -u 4004 -g linux -G distro,fedora mandriva

# 给fedora加密码，并设定其密码最短使用期限为2天，最长为50天
passwd -n 2 -x 50 fedora

# 将mandriva的默认shell改为/bin/bash;
usermod -s /bin/bash mandirva

# 添加系统用户hbase，且不允许其登录系统；
useradd -r -s /sbin/nologin hbase
```





## 权限管理

chown, chgrp, chmod, setfacl, getfacl, umask

```shell
############################## chown ##############################

chown hadoop /tmp/hi
# 将hi目录的属主改成hadoop，注意，只有该目录的属主被改变，其中的内容没有改变

chown -R hadoop /tmp/hi
# 目录和其中内容的属主都改变为hadoop

chown --reference=/tmp/abc /tmp/test
# 参考/tmp/abc，也就是和它一样

chown root:root /tmp/abc
# 同时改变属主和属组

chown :hadoop /tmp/abc
# 只改变属组





############################## chmod ##############################

chmod 750 /tmp/abc
# 修改文件abc权限为750

chmod 75 /tmp/abc
# 75=075

chmod 5 /tmp/abc
# 5=005

chmod --reference=/tmp/test /tmp/abc
# 将文件abc权限改成和test一样

chmod u=rwx /tmp/abc
# 修改属主权限

chmod g=rw /tmp/abc
# 修改属组权限

chmod o=rx /tmp/abc
# 修改其他用户权限

chmod g=r,o=r /tmp/abc
chmod go=rw /tmp/abc

chmod g=rx,o= /tmp/abc
# 不给权限就是没有权限

chmod u-x /tmp/abc
# 去掉属主的执行权限

chmod u+x,g-x /tmp/abc

chmod a+x /tmp/abc
# 对所有用户添加执行权限
# a可shengl
chmod -x /tmp/abc

chmod u-wx /tmp/abc
# 去掉属主的写权限和执行权限

chmod u+s /bin/cat
# 给文件/bin/cat添加SUID权限

chmod u-s /bin/cat
# 给文件/bin/cat取消SUID权限

chmod o+t /tmp/project/
# 对于文件夹project，给other设置sticky权限
chmod o-t /tmp/project/




############################## 练习 ##############################
# 手动创建一个新用户的完整过程
# 1，新建一个没有家目录的用户openstack
useradd -M openstack
# 2，复制/etc/skel到/home/openstack
cp -r /etc/skel /home/openstack
# 3，改变/home/openstack及其内部文件的属主属组均为openstack
chown -R openstack:openstack /home/openstack
# 4，/home/openstack及其内部文件的属组和其他用户没有任何访问权限
chmod -R go= /home/openstack
su - openstack








############################## FACL ##############################
# Filesystem Access Control List (FACL) 文件系统访问控制列表

# getfacl: get file access control lists

# 查看一个文件的facl
getfacl /etc/inittab

# setfacl: set file access control lists
# -m：设定
# -x：取消

# 设定用户的额外访问控制权限:setfacl -m u:username:perm filename
# 设置后，用户www，对文件inittab，有读写rw权限
setfacl -m u:www:rw /etc/inittab

# 设定组的额外访问控制权限:setfacl -m g:groupname:perm filename
# 设置后，属于www组的用户，对文件inittab，有读写rw权限
setfacl -m g:www:rw /etc/inittab

# 取消用户的额外访问控制权限:setfacl -x u:username
# 撤销一个用户对一个文件的facl权限
setfacl -x u:www /etc/inittab

# 取消组的额外访问控制权限:setfacl -x g:groupname
# 撤销一个用户组对一个文件的facl权限
setfacl -x g:www /etc/inittab






umask
# set file mode creation mask


```





## find&#x20;

```shell
# find 查找文件和目录
# 参考网页
# https://blog.51cto.com/zhubo/1830255
# https://blog.51cto.com/u_13788421/2147136
# https://www.cnblogs.com/client-server/p/5549223.html



#################### 组合 ####################

find /tmp -nouser -a -type d



#################### 根据时间查找 ####################

find ./ -amin -5
# ./: 当前目录
# -5: 五分钟之内访问过

find ./ -amin +5
# +5: 五分钟之前访问过

find /tmp -atime +30
# +30: 三十天之前访问过，也就是说，有三十天没有被访问过的文件 



#################### 根据权限查找 ####################

find ./ -perm 644
# 精确匹配 查找当前目录下权限为644的文件

find ./ -perm /644
# /斜线表示匹配644中的一个就可以

find ./ -perm -644
# -减号表示文件权限包含了644的都匹配，例如，666, 755



#################### 处理操作 查找后执行命令 ####################
# -ok COMMAND {} \; 反斜线合分号必须有。{} 表示引用find到的结果。  每次操作都需要用户确认
# -exec COMMAND {} \; 反斜线合分号必须有。{} 表示引用find到的结果。不需要确认

find ./ -prem -006 -exec chmod o-w {} \;

find ./ -type d -ok chmod +x {} \;

find ./ -perm -020 -exec mv {} {}.new \;

find ./ -name "*.sh" -a -perm -111 -exec chmod o-x {} \;



#################### 练习 ####################
# 查找/var下属主为root并且属组为mail的所有文件
find /var -user root -group mail 
find /var -user root -a -group mail
find /var -user root -a -group mail | xargs ls -ld
# -a可加可不加

# 查找/usr目录下不属于root、bin或student的文件
find /usr -not -user root -a -not -user bin | xargs ls -l | less
find /usr -not \(-user root -o  -user bin \)
# 两种解法

# 查找/etc目录下最近一周内内容修改过且不属于root及student用户的文件
find /etc -mtime -7 -not -group root | xargs ls -ld
find /etc -mtime -7 -a -not -group root | xargs ls -ld

# 查找当前系统上没有属主或属组且最近1天内曾被访问过的文件，并将其属主属组均修改为root；
find / -nouser -a -nogroup | xargs ls -ld
find / -nouser -o -nogroup | xargs ls -ld
find / -nouser -o -nogroup -a -atime -1
find / \( -nouser -o -nogroup \) -a -atime -1
find / \( -nouser -o -nogroup \) -a -atime -1 -exec chown root:root {} \;

# 查找/etc目录下所有用户都没有写权限的文件，显示出其详细信息
find /etc -not -perm /222 -ls

# 查找/etc目录下大于1M的文件，并将其文件名写入/tmp/etc.largefiles文件中
find /etc -size +1M -exec echo {} >> /tmp/etc.largefiles \;
find /etc -size +1M | xargs echo >> /tmp/etc.largefiles
find /etc -size +5k | wc -l
```





## grep

egrep = grep -E

fgrep

```shell
# grep: Global Research
# 使用格式：grep [options] pattern [file...]
# pattern要用单引号引起来
# pattern -> 正则表达式 Regular Expression->REGEXP
# 1. 基本的正则表达式->Basic REGEXP
# 2. 扩展的正则表达式->Extened REGEXP 
##### -> +加号
##### -> {}花括号 和 ()分组前，不用加反斜线\
##### -> | 竖线 表示或者
# https://blog.51cto.com/zhubo/1846555
# https://www.cnblogs.com/darwinli/p/9043330.html
# https://blog.51cto.com/zhubo/1827128

grep 'root' /etc/passwd
grep --color 'root' /etc/passwd
# 查找passwd文件中，包含root单词的行
alias grep='grep --color'

grep -v 'root' /etc/passwd
# 显示不包含root单词的行

grep -o 'root' /etc/passwd
# 只显示root单词，如果有

grep '^$' /etc/inittab | wc -l
# 显示inittab文件中有多少空白行

grep '[[:digit:]]$' /etc/inittab
# 显示以一个数字为结尾的所有行

grep '[[:space:]][[:digit:]]$' /etc/inittab
# 显示以一个空格和一个数字为结尾的所有行

grep 'root\>' test2.txt
# 出现在词尾
grep '\<root' test2.txt
# 出现在词首
grep '\<root\>' test2.txt
# 单独一个单词

grep '\([0-9]\).*\1' /etc/inittab
# [0-9]表示0到9任意一个数字作为一个分组
# .*表示任意个字符，可以是0个
# \1表示引用前面的分组，就是和分组的内容一样

grep -A 2 '^core id' /proc/cpuinfo
# 显示匹配的行和其后面2行 after
grep -B 2 '^core id' /proc/cpuinfo
# 显示匹配的行和其前面2行 before
grep -C 2 '^core id' /proc/cpuinfo
# 显示匹配的行和其前后各2行 context

grep --color -E 'C|cat' test6.txt
# 字符C或单词cat
grep --color -E '(C|c)at' test6.txt
# 字符C或c，Cat或cat



grep --color -E '^[[:space:]]+' /boot/grub/grub.conf
# 在行首，至少一个空白字符的行

ifconfig | egrep --color '\b([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\b'
ifconfig | egrep --color '\<([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>'
# 取出文件中1-255的整数

# 找出ifconfig中的IP地址 
ifconfig | egrep '\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>\.\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>\.\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>\.\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>'
ifconfig | egrep --color '(\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>\.){3}\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>'

# 找出ifconfig中的ABC 3类IP地址 
\<([1-9]|[1-9][0-9]|1[0-9]{2}|2[01][0-9]|22[0-3])\>(\.\<([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-4])\>){2}\.\<([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-4])\>
```



