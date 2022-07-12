# 磁盘及文件系统管理

## 磁盘及文件系统管理

```bash
# 磁盘及文件系统管理详解之一
# https://www.youtube.com/watch?v=LBPDEeiPqTw
# 磁盘及文件系统管理详解之二
# https://www.youtube.com/watch?v=3xlMMocCi1c




# ln - make links between files

ln abc test/abc2
# 对abc文件创建一个硬链接文件test/abc2

ln -sv /backup/abc /backup/test/abc2
# 对/backup/abc文件创建一个软链接文件/backup/test/abc2






# du - estimate file space usage

du /backup
# 显示/backup目录下，每个文件的大小；包括自身
du -s /backup
du -sh /backup
# 显示/backup目录自身的大小






# df - report file system disk space usage

df
df -h
df -i
df -P
# 不换行




# mknod - make block or character special files

mknod mydev c 66 0
# 在当前目录下，创建一个设备文件，文件名mydev，类型c是字符，主设备号66（表示类型），次设备号0
mknod -m 640 mydev2 c 66 1
# -m用来设置权限






# tty - print the file name of the terminal connected to standard input
tty
```







## [10 01 Raid及mdadm命令之一 - YouTube](https://www.youtube.com/watch?v=Oe4WQA-SGoU)



```bash
#!/bin/bash
#
cat << EOF
d|D) show disk usages.
m|M) show memory usages.
s|S) show swap usages.
*) quit.
EOF
echo -n -e "\033[1;31;41mYour choice: \033[0m"
read CHOICE
# read -p "Your choice: " CHOICE

while [ $CHOICE != 'quit' ];do
  case $CHOICE in
  d|D)
    echo "Disk usage: "
    df -Ph ;;
  m|M)
    echo "Memory usage: "
    free -m | grep "Mem" ;;
  s|S)
    echo "Swap usage: "
    free -m | grep "Swap" ;;
  *)
    echo "Unknown.." ;;
  esac
  
  read -p "Again, your choice: " CHOICE
done

```





