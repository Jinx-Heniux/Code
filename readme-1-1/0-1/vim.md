# vim

## 打开文件

```bash
vim /path/to/file
# 打开文件 光标在第1行

vim +10 /path/to/file
# 打开文件 光标在第10行

vim + /path/to/file
# 打开文件 光标在最后一行
```

## 输入模式

```bash
i
# 在当前光标所在字符的前面，转为输入模式，insert；
a
# 在当前光标所在字符的后面，转为输入模式，append；
o
# 在当前光标所在行的下方，新建一行，并转为输入模式；
I
# 在当前光标所在行的行首，转换为输入模式
A
# 在当前光标所在行的行尾，转换为输入模式
O
# 在当前光标所在行的上方，新建一行，并转为输入模式；

ESC
# 退出输入模式
```

## 末行模式

```bash
:
# 进入末行模式

:10d
# 删除第10行

:10,20d
# 删除第10到第20行

:set nu
# 显示行号

:! ls /etc
# 直接执行shell命令
# 再按一次回车键可以再次回到之前的界面。
```

## 关闭文件

```bash
########## 末行模式关闭文件
:q  
# 不保存退出
:wq 
# 保存并退出
:q! 
# 不保存并退出 强行退出
:w 
# 保存
:w! 
# 强行保存 只有管理员才可以这样做
:w cszhi.com   
# 保存至cszhi.com文件
:x
# 同:wq； 如果文件没有修改，直接退出；如果有修改，则保存后退出
:e!
# 放弃所有修改，并打开原来文件





########## 编辑模式下退出
ZZ
# 保存并退出
```

## 移动光标

```bash
##### 移动字符
h
# 向左移动一个字符
# 同删除键，Backspace
l
# 向右移动一个字符
# 同空格键，Space
j
# 向下移动一行
# 处于下一行的当前位置，如果为空白行，则行首
# 同回车键，Enter；区别：回车键用于移动到下一行非空白字符的行首，即第一个字符
k
# 向上移动一行
# 处于上一行的当前位置，如果为空白行，则行首
# 同减号键，-；区别：减号键用于移动到上一行非空白字符的行首，即第一个字符

# 可与数字连用
3h
# 向左三个字符
3j
# 向下移动三行




##### 移动单词
w
# 移动到下一个单词的词首； 会忽略标点符号
3w
# 跳3个单词 
b
# 跳至当前或前一个单词的词首
# 移动到所在单词的词首，如果此时光标不在当前单词词首先跳到当 前单词的词首，
# 然后在按e就移动到下一个单词的词首



e
# 跳至当前或下一个单词的词尾
# 移动到所在单词的词尾，如果此时光标不在当前单词词尾先跳到当 前单词的词尾，
# 然后在按e就移动到下一个单词的词尾
ge 
# 移动到前一个单词的词尾，或跳到前一个标点符号





0
# 跳到绝对行首
^
# 跳到第一个非空白字符
$
# 绝对行尾




G
# 跳到文本的最后一行

gg
# 跳到文本的第一行

10G
10gg
# 跳到文本的第十行




# 末行模式下，直接给出行号即可
:2
# go to the second line
:100
# go to 100 line





##### 页面
H 
# 跳到屏幕的上部
M
# 跳到屏幕的中间
L
# 跳到屏幕的下面

zt
# 当前行移到顶
zz
# 当前行移到中
zb
# 当前行移到底




##### 句子跳转 以英文句号.为准
(
# 跳到上一句的句首
)
# 跳到下一句的句首




##### 段落跳转 以空白行为准
{
# 跳到上一段
}
# 跳到下一段
```
