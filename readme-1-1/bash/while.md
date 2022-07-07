# while

## [09 04 Linux压缩及归档 - YouTube](https://www.youtube.com/watch?v=3KiFY42wKB0)

### 练习：计算100以内所有正整数的和

```bash
#!/bin/bash

declare -i I=1
declare -i SUM=0

while [ $I -le 100 ]; do
  let SUM+=$I
  let I++
done

echo $SUM
```

### 练习：转换用户输入的字符为大写，除了quit:

```bash
#!/bin/bash
#

read -p "input something ...> " STRING

while [ $STRING != 'quit' ]; do
  echo $STRING | tr 'a-z' 'A-Z'
  read -p "input something ...> " STRING
done
```

### 练习：每隔5秒查看hadoop用户是否登录，如果登录，显示其登录并退出；否则，显示当前时间，并说明hadoop尚未登录

```bash
#!/bin/bash
#

who | grep "hadoop" &> /dev/null
RETVAL=$?

while [ $RETVAL -ne 0 ]; do
  echo "`date`, hadoop is not log."
  sleep 5
  who | grep "hadoop" &> /dev/null
  RETVAL=$?
done
```

