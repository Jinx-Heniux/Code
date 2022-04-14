# sed



```shell
# 06 02 sed命令 
# https://youtu.be/Oy6L3tv7nNk
# https://blog.51cto.com/zhubo/1828955
# https://www.cnblogs.com/itmeatball/p/7462797.html
# 
# 默认情况下，不修改原文件


#################### d命令 ####################
# 删除符合条件的行

sed '1,2d' /etc/passwd
# 删除第一和二行

sed '3,$d' /etc/fstab
# 删除从第三行到最后一行

sed '/oot/d' /etc/fstab
# 使用模式//, 所有包含oot的行都被删除

sed '1,+2d' /etc/fstab
# 删除第一行以及向下两行

sed '1d' /etc/fstab
# 只删除第一行

sed '/^\//d' /etc/fstab
# 删除已斜线/开头的行，\转义




#################### p命令 ####################
# p:显示符合条件的行

sed '/^\//p' /etc/fstab
# 默认显示模式空间的所有行，p命令显示匹配的行，所以，匹配的行被显示两次
sed -n '/^\//p' /etc/fstab
# 加-n选项，不再显示模式空间中的内容




#################### a命令 ####################
# a \string:在指定的行后面追加新行，内容为string

sed '/^\//a \# hello world' /etc/fstab
# 在以斜线\开头的行下面，追加新行
sed '/^\//a \# hello world\n# hello, linux' /etc/fstab
# \n表示换行




#################### r命令 ####################
# r  FILE:将指定的文件的内容添加到符合条件的行处

sed '2r /etc/issue' /etc/fstab
# 将issue文件的内容添加到，fstab文件中的第二行后

sed '$r /etc/issue' /etc/fstab
# 在最后一行添加，相当于合并两个文件的内容




#################### w命令 ####################
# w  FILE:将指定范围内的内容另存至指定的文件中
# 另存为

sed '/oot/w /tmp/oot.txt' /etc/fstab
# 将fstab文件中，所有包含oot的行，保存到oot.txt文件中
# 同时文件中的内容也被打印到屏幕上

sed -n '/oot/w /tmp/oot.txt' /etc/fstab
# 同上，但内容不打印到屏幕上




#################### s命令 ####################
# 查找并替换
# s/pattern/string/ 
# s#pattern#string# 
# s@pattern@string@
# g 全局替换 i 忽略字符大小写 
# \(\), 分组 \1, \2 后向引用
# &: 引用模式匹配整个串

sed 's/oot/OOT/' /etc/fstab
# 将文件的每一行中，第一个匹配到oot，换成OOT

sed 's/^\//#/' /etc/fstab
# 将行首的斜线/替换成井号#

sed 's/\//#/' /etc/fstab
# 替换第一个匹配
sed 's/\//#/g' /etc/fstab
# g修饰符，替换所有匹配


echo love | sed 's/l..e/&r/'
echo like | sed 's/l..e/&r/'
# &引用模式匹配到的整个字符串

echo "love like" | sed 's/\(l..e\)/\1r/g'
# 输入 love like 输出 lover liker

echo "love like" | sed 's@l\(..e\)@L\1@g'
# 输入 love like 输出 Love Like

sed 's#\(id:\)[0-9]\(:initdefault:\)#\15\2#g' /etc/inittab
# 在/etc/inittab文件中，id:数字:initdefault，将中间的数字换为5


history | sed 's#^[[:space:]]*##' | cut -d' ' -f1
history | sed -r 's#^[[:space:]]+##g' | cut -d' ' -f1
# 使用history命令将所有的历史命令号码提取出来

echo this is digit 7 in number | sed 's/digit \([0-9]\)/\1/'
# 1、首先查找digit \( [ 0-9]\),查找到为digit 7
# 2、又因为\1在此匹配子串[0-9],即匹配子串7
# 3、因此结果是查找到digit 7然后用7替换

echo aaa BBB | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
echo aaa BBB | sed 's/\([a-z]*\) \([A-Z]*\)/\2 \1/'
# 交换位置
```





