# for

## [10 05 脚本编程之八 脚本完成磁盘分区格式化](https://www.youtube.com/watch?v=kiUp9i7ewEI)

11:40

### 从1加到100之和 求和

```bash
#!/bin/bash
#
declare -i SUM=0

for I in {1..100}; do
  let SUM+=$I
done

echo $SUM

```



```bash
#!/bin/bash
#
declare -i SUM=0

for ((I=1;I<=100;I++ )); do
  let SUM+=$I
done

echo $SUM

```



### 一百以内所有偶数之和

```bash
#!/bin/bash
#
declare -i SUM=0

for ((I=2;I<=100;I+=2 )); do
  let SUM+=$I
done

echo $SUM

```

