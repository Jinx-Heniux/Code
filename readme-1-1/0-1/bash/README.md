# Bash



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



## 08 02 bash脚本编程之七 case语句及脚本选项进阶

[https://www.youtube.com/watch?v=jcKZFmvAXnE](https://www.youtube.com/watch?v=jcKZFmvAXnE)

### 求最大值和最小值

```bash
#!/bin/bash
#
declare -i MAX=0
declare -i MIN=0

for I in {1..10}; do
  MYRAND=$RANDOM
  [ $I -eq 1 ] && MIN=$MYRAND
  if [ $I -le 9 ]; then
    echo -n "$MYRAND,"
  else
    echo "$MYRAND"
  fi
  [ $MYRAND -gt $MAX ] && MAX=$MYRAND
  [ $MYRAND -lt $MIN ] && MIN=$MYRAND
done

echo $MAX, $MIN

# 
# 
```

### case

```bash
#!/bin/bash
#
case $1 in
[0-9])
  echo "digit";;
[a-z])
  echo "lower";;
[A-Z])
  echo "upper";;
*)
  echo "special character";;
esac

```



```bash
#!/bin/bash
#
case $1 in
'start')
  echo "start server...";;
'restart')
  echo "restart server...";;
'stop')
  echo "stop server...";;
*)
 echo "`basename $0` <start|restart|stop>";;
esacb
```



```bash
#!/bin/bash
#
DEBUG=0
case $1 in
-v|--verbose)
  DEBUG=1;;
*)
  echo "Unknown Options"
  exit 7
  ;;
esac

[ $DEBUG -eq 1 ] && echo "hello"
```

### 添加用户 删除用户 脚本接收多个选项和参数

```bash
#!/bin/bash
#
DEBUG=0
ADD=0
DEL=0

for I in `seq 1 $#`; do
case $1 in
-v|--verbose)
  DEBUG=1
  shift
  ;;
-h|--help)
  echo "Usage: `basename $0` --add USER_LIST --del USER_LIST -v|--verbose -h|--help"
  exit 0
  ;;
--add)
  ADD=1
  ADDUSERS=$2
  shift 2
  ;;
--del)
  DEL=1
  DELUSERS=$2
  shift 2
  ;;
#*)
#  echo "Usage: `basename $0` --add USER_LIST --del USER_LIST -v|--verbose -h|--help"
#  exit 7
#  ;;
esac
done

echo $DEBUG $ADD $DEL

if [ $ADD -eq 1 ]; then
  for USER in `echo $ADDUSERS | sed 's@,@ @g'`; do
    if id $USER &> /dev/null; then
      [ $DEBUG -eq 1 ] && echo "$USER exists."
    else
      useradd $USER
      [ $DEBUG -eq 1 ] && echo "Add user $USER finished."
    fi
  done
fi

if [ $DEL -eq 1 ]; then
  for USER in `echo $DELUSERS | sed 's@,@ @g'`; do
    if id $USER &> /dev/null; then
      userdel -r $USER
      [ $DEBUG -eq 1 ] && echo "Delete $USER finished."
    else
      [ $DEBUG -eq 1 ] && echo "$USER does not exist."
    fi
  done
fi

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

