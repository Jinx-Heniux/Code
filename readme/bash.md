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

# https://www.onlinegdb.com/
```

