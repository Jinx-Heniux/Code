# Commands

## 常用

### man

### whatis

### which



## 用户 用户组



### w

### who

### whoami



权限管理

umask

suid

## 密码

### md5sum

### passwd



## 文件 文件夹

### basename

### file



文本编辑

vim, grep, sed&#x20;

磁盘管理

df, free, fdisk, mount, mke2fs, e2fsck&#x20;

文件系统&#x20;

压缩及归档

gzip(.gz), bzip2(.bz2), xz, zip,cpio, tar,&#x20;

Bash脚本

if, for, while, case&#x20;

## 配置文件 配置文件夹

### /etc/passwd

### /etc/shadow

### /etc/group

### /etc/gshadow

### /etc/shells

### /sbin/nologin

### /etc/default/useradd

### /usr/local/bin

### /etc/sudoers&#x20;



## 小命令

last, lastb, lastlog, hostname,&#x20;

```shell

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







```



## 用户管理







## 权限管理

chown, chgrp, chmod, setfacl, getfacl, umask

```shell



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



