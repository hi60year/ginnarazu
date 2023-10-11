---
title: 重庆大学python课程常见问题汇总
date: 2023-10-08 20:50:39
tags:
comment: false
---

## 写在前面

本文是为了避免python课程群内多次相同问题的重复提问，提高问答效率而撰写。请你在课程群提问之前先在本文查找自己的问题是否已经出现过。

对于本文内无法理解的内容，希望你能够自己通过翻阅课本、复习网课等方式优先自行解决。

本文将常见问题分为三类，运行时错误问题、错误答案问题、“如何做”问题。其中，运行时错误问题是当你的程序**直接被解释器终止无法运行**时碰到的问题；错误答案问题是你的程序**可以正常运行、可以通过OJ所给定的样例，但无法通过全部通过**时碰到的问题；“如何做”问题是你**清楚的明白自己的需求，但不知道如何用代码去实现**时所碰到的问题。

## 运行时错误问题

### can't multiply sequence by non-int

#### 问题原因

python 是一个拥有强类型检查的语言，这意味着字符串类型(str)没有办法直接用于算数运算，即使你想用该字符串代表一个数字。看看如下的例子，你觉得结果是什么：

```python
    "111" * 3
```

{% folding blue::答案%}
    答案是"111111111"，而不是333或"333"。在python中，用一个整数n乘以一个序列，得到的结果是这个序列重复n次并连接起来的新序列。
{% endfolding %}

顺带一提，`"111" * "111"`这样的写法就会导致标题的问题。

请务必时刻记住，`input()`函数的结果是`str`类型。即类似于

```python
    v = input()
    a = v*v
```

这样的代码也会导致上述问题。

#### 解决方案

通过将字符串转换成整数/浮点数来解决这个问题，即，使用`int()`, `float()`等。也可以选择使用`eval()`。
如上例，解决方案为

```python
    v = float(input())
    a = v*v
```

### no module named 'xxx' 问题

遇到该问题时有两个主要原因。
- 模块的名字打错
- 未安装该模块

若该模块没有被安装，请在你的终端（cmd, bash, powershell, ...）中输入以下命令：

{% note yellow %}
若你正在使用vscode，可以使用 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>`<kbd> 调出终端
{% endnote %}

```bash
pip install xxx
```

其中，"xxx"是你要安装的模块名字。

{% notel yellow 安装过慢的解决方案%}
由于pip不会直接连接国内源，所以在安装时若没有科学上网可能会出现速度过慢的问题。可以通过提供`-i`参数指定要用的源，以清华源为例：

```bash
pip install xxx -i https://pypi.tuna.tsinghua.edu.cn/simple
```

若你嫌每次都要提供这个参数过于麻烦，请参考：[pip换源](https://zhuanlan.zhihu.com/p/551940762)
{% endnotel %}




## 错误答案问题

当你遇到答案错误问题时，首先重新读题，然后确认自己的输出是否每一个字母都和答案一致，没有多输出也没有少输出任何字符。常见的错误情况有：多打/少打空格、少打句号、拼写错误等。

### 如何避免拼写错误、多打少打空格、少打句号、大小写错等问题？

首先，出现这种情况的时候一般是该题目需要一个字符串的输出，比如：

```plain
The acceleration of C900 is <> M / s, the take-off speed is <> M / s, and the shortest runway length is <> M.
```

在这一题上许多同学的标点符号、拼写、空格数量都出现问题。

比较好的解决方案是**直接复制样例的答案**，然后在最后输出的时候粘贴后对整个样例答案的模板字符串进行字符串格式化，例如，我们假定上述三空需要填写的变量名字分别为`v1`, `v2`, `s`，此处输出保留两位小数：

{% tabs first tab %}
<!-- tab format函数法 -->
```python
print("The acceleration of C900 is {:.2f} M / s, the take-off speed is {:.2f} M / s, and the shortest runway length is {:.2f} M.".format(v1, v2, s))
```
<!-- endtab -->

<!-- tab f-string 法-->
```python
print(f"The acceleration of C900 is {v1:.2f} M / s, the take-off speed is {v2:.2f} M / s, and the shortest runway length is {s:.2f} M.")
```
<!-- endtab -->

<!-- tab %号法 -->
```python
print("The acceleration of C900 is %.2f M / s, the take-off speed is %.2f M / s, and the shortest runway length is %.2f M." % (v1, v2, s))
```
<!-- endtab -->

{% endtabs %}

如果采用这样的方案，很难想象还能出现任何形式的拼写错误。

### 未通过`input()`获取输入

OJ里面的样例只是为了展示给你在特定的输入下应该有怎样的输出。实际情况的输入是和样例的输入不同的，因此需要通过`input()`函数获取输入，请认真读题。

譬如，题目的要求是：输入一个名字`name`，输出 我是`name`，并给定以下样例：

{% notel grey 输入样例 %}
李华
{% endnotel %}

{% notel blue 输出样例 %}
我是李华
{% endnotel %}

则如下的代码是错误的：

{% note danger fa-circle-exclamation  %}
注意：如下代码是错误示范
{% endnote %}

```python
name = "李华"
print(f"我是{name}")
```

上述程序根本没有获取输入，是完全没有意义的程序，无论怎样运行也只能输出“我是李华”这一种结果，显然这不是我们所需要的。
正确代码如下：

```python
name = input()
print(f"我是{name}")
```

### 不要填写`input()`函数的`prompt`参数

`input()`函数的位置参数叫做`prompt`，即输入提示。这部分内容会被输出到输出流被OJ读取从而判断错误。例如，OJ让你输出 输入的两个数字的和。

{% note danger fa-circle-exclamation  %}
注意：如下代码是错误示范
{% endnote %}

```python
a = input("please input a")
b = input("please input b")
print(a+b)
```

这样的代码会多输出两行"please input a", "please input b"从而被OJ判断为错误答案。

### 循环删除问题

这是一个相当高频的问题。看如下例子：

```python
a = list(range(10))
for i in a:
    if i > 5: a.remove(i)
print(a)
```

这个程序的输出是什么？


{% folding blue::答案%}
答案是[0, 1, 2, 3, 4, 5, 7, 9]。
{% endfolding %}

如果你做错了答案，请你往下看：

为什么会出现这样的答案？按直觉来说不应该输出的是`[0, 1, 2, 3, 4, 5]`吗？看如下情况，i引用a\[6\]的位置：
```plain
                          i
                          |
                          ↓
┌---┬---┬---┬---┬---┬---┬---┬---┬---┬---┐
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | a
└---┴---┴---┴---┴---┴---┴---┴---┴---┴---┘

% 如果不执行删除操作，下一次循环为：

                              i
                              |
                              ↓
┌---┬---┬---┬---┬---┬---┬---┬---┬---┬---┐
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | a
└---┴---┴---┴---┴---┴---┴---┴---┴---┴---┘

% 如果在这之前执行了一次删除操作，实际情况是：

                              i
                              |
                              ↓
┌---┬---┬---┬---┬---┬---┬---┬---┬---┐
| 0 | 1 | 2 | 3 | 4 | 5 | 7 | 8 | 9 | a
└---┴---┴---┴---┴---┴---┴---┴---┴---┘

```

你会神奇的发现，虽然`i`还是按往常一样向前移动了一个位置，但是删除之后，由于`i`的前面少了一个位置，就相当于`i`向前移动了两个位置！

这是初学者超常犯的错误，为了避免这样的情况，在这里提供三种方案：

{%tabs third tab%}
<!--tab 重新构造法 -->
这是我本人比较喜欢的方案。相比于删除的概念，重新构造体现了函数式编程范式（functional programming paradigm）中不可变性（immutablity）的思想。另外，该方法的效率较高。

```python
a = [i for i in a if i <= 5]
```
<!-- endtab -->
<!-- tab 副本删除法 -->
让`i`去遍历一个不会被更改的副本，这样删除元素后不影响`i`本身的遍历结果，删除也就自然正常。该方案的缺点是时间效率比较差。
```python
# 这里使用a的全体切片，其实就是a的副本
for i in a[:]:
    if i > 5: a.remove(i)
```
<!--endtab-->
<!-- tab 逆向下标遍历法 -->
逆向遍历下标，删除时就不会影响到下标的遍历顺序：
```python
for i in range(len(a) - 1, -1, -1):
    if a[i] > 5: a.pop(i)
```
<!-- endtab -->
{%endtabs%}

## “如何做”问题

这部分问题大部分其实都在课本或者课堂内有讲解，当你不知道如何去做某件事情的时候不妨翻阅一下课本或者有关资料。

### 如何保留n位小数

请注意，“保留小数”都是指在输出时以保留数位小数的形式输出，而不是去更改该小数的值，因此，保留小数只需要在输出时**通过特定方式将小数转换成字符串的一部分（即格式化字符串）**进行处理，切记避免画蛇添足。

[请先阅读：python字符串的格式化](https://www.geeksforgeeks.org/string-formatting-in-python/)

此处简单用例子进行说明，可以将例子中的1.666换成任何float类型表达式：

{% tabs second tab %}
<!-- tab format函数法 -->
```python
# 输出1.67
print("{:.2f}".format(1.666))
```
<!-- endtab -->

<!-- tab f-string 法-->
```python
# 输出1.67
print(f"{1.666:.2f}")
```
<!-- endtab -->

<!-- tab %号法 -->
```python
#输出1.67
print("%.2f" % 1.666)
```
<!-- endtab -->
{% endtabs %}


### 如何去除`print()`函数不同参数间的空格

平时利用`print()`输出多个参数时，python会默认会在不同参数间添加空格，有时不是想要的行为，例如：

```python
print("Hello", "World", 2.5)
```

会输出 `Hello World 2.5` 而非 `HelloWorld2.5`。要避免这个行为，可以更改`print()`的`sep`关键字参数来更改分隔不同参数的分隔符。当参数值为空字符串（`''`或`""`）时，不会在中间添加任何字符。

```python
# 输出 HelloWorld2.5
print("Hello", "World", 2.5, sep = '')
```

{% note  %}
同理，`end`关键字参数控制`print()`输出后添加的字符，默认是`'\n'`（换行符），更改此参数可以让print不再输出后换行，留作读者训练。
{% endnote %}

### 如何输入点的坐标x, y

如`2.4, 2.5`，这样的输入可以用两种方式解决。

```python
# 方案一：这种方案成立的原因是，这样的输入本质上是一个元组(tuple)
x, y = eval(input())

# 方案二：用split获取","旁边的两个字符串后利用map分别转换成float类型，有关map和split的内容请自行查询
x, y = map(float, input().split(','))
```
