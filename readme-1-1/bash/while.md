# while

## 整数求和

练习：计算100以内所有正整数的和

[09 04 Linux压缩及归档 - YouTube](https://www.youtube.com/watch?v=3KiFY42wKB0)

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



