# until

## 10 05 脚本编程之八 脚本完成磁盘分区格式化

4:33 小写转大写

```bash
#!/bin/bash
#
read -p "Input something: " STRING

until [ $STRING == 'quit' ]; do
  echo $STRING | tr 'a-z' 'A-Z'
  read -p "Agian, Input something: " STRING
done

```

