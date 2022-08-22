# tar



```bash

tar -jcf /tmp/users.tar.bz2 inittab issue
tar jcf /tmp/users.tar.bz2 inittab issue
# -可省略
# 将文件inittab和issue打包并压缩到/tmp/，文件名为users.tar.bz2

tar xf /tmp/users.tar.bz2 -C ./
# 将/tmp/users.tar.bz2文件解压到当前目录下
```

