# bash



```bash
bash -x script.sh
# -x: Print commands and their arguments as they are executed.
```





```bash
#!/bin/bash
#
if [ $1 == '-s' ]; then
  ! grep "${2}$" /etc/shells &> /dev/null && echo "Invalid shell." && exit 7
elif [ $1 == '--help' ]; then
  echo "Usage: `basename $0` -s SHELL | --help"
  exit 0
else
  echo "Unknown Options."
  exit 8
fi

NumOfUser=`grep "${2}$" /etc/passwd | wc -l`
ShellUsers=`grep "${2}$" /etc/passwd | cut -d: -f1`
ShellUsers=`echo $ShellUsers | sed 's@[[:space:]]@,@g'`

echo -e "$2, $NumOfUser users, they are: \n$ShellUsers"

# 08 01 facl及用户及Linux终端
# https://www.youtube.com/watch?v=diGtbcD5U1g
```







## 08 03 磁盘及文件系统管理详解之一

[https://www.youtube.com/watch?v=LBPDEeiPqTw](https://www.youtube.com/watch?v=LBPDEeiPqTw)

### 脚本showlogged.sh

```bash
#!/bin/bash
#
declare -i SHOWNUMS=0
declare -i SHOWUSERS=0

echo $#
echo `seq 1 $#`

if [ ! $# -gt 0 ]; then
  echo "Usage: `basename $0` -h|--help -c|--count -v|--verbose"
  exit 1
fi

for I in `seq 1 $#`; do
  if [ $# -gt 0 ]; then
    case $1 in
    -h|--help)
      echo "Usage: `basename $0` -h|--help -c|--count -v|--verbose"
      exit 0;;
    -v|--verbose)
      let SHOWUSERS=1
      shift;;
    -c|--count)
      let SHOWNUMS=1
      shift;;
    *)
      echo "Usage: `basename $0` -h|--help -c|--count -v|--verbose"
      exit 8;;
    esac
  fi
done

if [ $SHOWNUMS -eq 1 ]; then
  echo "Logged-in users: `who | wc -l`"
  if [ $SHOWUSERS -eq 1 ]; then
    echo "They are: "
    who
  fi
fi

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



