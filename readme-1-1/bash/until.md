# until

## 10 05 脚本编程之八 脚本完成磁盘分区格式化

4:33&#x20;

### 小写转大写

```bash
#!/bin/bash
#
read -p "Input something: " STRING

until [ $STRING == 'quit' ]; do
  echo $STRING | tr 'a-z' 'A-Z'
  read -p "Agian, Input something: " STRING
done

```



### 判断某用户是否登录系统

```bash
#!/bin/bash
#
who | grep 'azhs2si' &> /dev/null
RETVAL=$?

until [ $RETVAL -eq 0 ]; do
  echo "zhs2si is not logged in!"
  sleep 5
  who | grep 'zhs2si' &> /dev/null
  RETVAL=$?
done

echo "zhs2si is logged in!"

```

