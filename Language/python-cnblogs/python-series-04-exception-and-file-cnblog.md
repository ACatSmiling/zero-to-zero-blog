>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-27`*

## 异常

程序在运行过程当中，不可避免的会出现一些错误，比如：使用了没有赋值过的变量，使用了不存在的索引，除 0 等。这些错误在程序中，我们称其为**`异常`**。

-  程序运行过程中，一旦出现异常将会导致程序立即终止，异常以后的代码全部都不会执行！

### 处理异常

程序运行时出现异常，目的并不是让我们的程序直接终止！Python 是希望在出现异常时，我们可以编写代码来对异常进行处理！

**`try-except-finally 语句`**：

```python
try:
    代码块（可能出现错误的语句）
except 异常类型 as 异常名:
    代码块（出现错误以后的处理方式）
except 异常类型 as 异常名:
    代码块（出现错误以后的处理方式）
except 异常类型 as 异常名:
    代码块（出现错误以后的处理方式）
else:
    代码块（没出错时要执行的语句）
finally:
    代码块（该代码块总会执行）
```

- **try 是必须的，else 语句有没有都行，except 和 finally 至少有一个。**

- 可以将可能出错的代码放入到 try 语句，这样如果代码没有错误，则会正常执行，如果出现错误，则会执行 expect 子句中的代码，这样我们就可以通过代码来处理异常，避免因为一个异常导致整个程序的终止。

### 异常的传播

当在函数中出现异常时，如果在函数中对异常进行了处理，则异常不会再继续传播，如果函数中没有对异常进行处理，则异常会继续向函数调用处传播，如果函数调用处处理了异常，则不再传播，如果没有处理则继续向调用处传播，直到传递到全局作用域（主模块），如果依然没有处理，则程序终止，并且显示异常信息。

当程序运行过程中出现异常以后，所有的异常信息会被保存一个专门的异常对象中，而异常传播时，实际上就是异常对象抛给了调用处。

示例：

```python
def fn():
    print('Hello fn')
    print(a)
    print(10 / 0)


def fn2():
    print('Hello fn2')
    fn()


def fn3():
    print('Hello fn3')
    fn2()


fn3()
```

### 异常对象

Python 中常见的异常类型：

| 报错类型          | 描述                                           |
| ----------------- | ---------------------------------------------- |
| AssertionError    | 当 assert 断言条件为假的时候抛出的异常         |
| AttributeError    | 当访问的对象属性不存在的时候抛出的异常         |
| IndexError        | 超出对象索引的范围时抛出的异常                 |
| KeyError          | 在字典中查找一个不存在的 key 抛出的异常        |
| NameError         | 访问一个不存在的变量时抛出的异常               |
| OSError           | 操作系统产生的异常                             |
| SyntaxError       | 语法错误时会抛出此异常                         |
| TypeError         | 类型错误，通常是不同类型之间的操作会出现此异常 |
| ZeroDivisionError | 进行数学运算时除数为 0 时会出现此异常          |

示例：

```python
print('异常出现前')
l = []
try:
    # print(c)
    # l[10]
    # 1 + 'hello'
    print(10 / 0)
except NameError:
    # 如果 except 后不跟任何的内容，则此时它会捕获到所有的异常
    # 如果在 except 后跟着一个异常的类型，那么此时它只会捕获该类型的异常
    print('出现 NameError 异常')
except ZeroDivisionError:
    print('出现 ZeroDivisionError 异常')
except IndexError:
    print('出现 IndexError 异常')
# Exception 是所有异常类的父类，所以如果 except 后跟的是 Exception，它也会捕获到所有的异常
# 可以在异常类后边跟着一个 as xx 此时 xx 就是异常对象
except Exception as e: # 等同于 except:
    print('未知异常', e, type(e))
finally:
    print('无论是否出现异常，该子句都会执行')

print('异常出现后')
```

### 抛出异常

可以使用`raise 语句`来手动抛出异常，raise 语句后需要跟一个异常类或异常的实例。

示例：

```python
# 也可以自定义异常类，只需要创建一个类继承 Exception 即可
class MyError(Exception):
    pass


def add(a, b):
    # 如果 a 和 b 中有负数，就向调用处抛出异常
    if a < 0 or b < 0:
        # raise 用于向外部抛出异常，后边可以跟一个异常类，或异常类的实例
        # raise ExceptioUS20190040060A1-20190207.XMLn
        # 抛出异常的目的，告诉调用者这里调用时出现问题，希望你自己处理一下
        # raise Exception('两个参数中不能有负数！')
        raise MyError('自定义的异常')

        # 也可以通过 if else 来代替异常的处理，但无法考虑到所有可能出异常的条件，难以控制
    return a + b


print(add(-123, 456))
```

## 文件

文件是以计算机硬盘为载体存储在计算机上的信息集合，文件可以是文本文档、图片、程序等等。计算机文件基本上分为二种：二进制文件和纯文本文件。

- `二进制文件`：**没有统一的字符编码。**与文本文件的一个最主要的区别在于是否有统一的字符编码格式，二进制文件顾名思义是直接由 0 与 1 组成，无统一的字符编码，例如图片文件（jpg、png），视频文件（avi）等。
- `纯文本文件`：**有统一的编码，可以被看做存储在磁盘上的长字符串。**编码格式常见的有 ASCII、ISO-8859-1、GB2312、GBK、UTF-8、UTF-16 等。

通过 Python 程序，操作文件的步骤：

- 打开文件。
- 对文件进行各种操作（读、写），然后保存。
- 关闭文件。

### 打开文件

**`open()`**：打开一个文件。

语法：

```python
open(file, mode='r', buffering=-1, encoding_=None, errors=None, newline=None, closefd=True, opener=None)
```

- `file`：要打开的文件的名字（路径）。

- `mode`：打开文件的模式。

  | mode   | 解释                                                         |
  | ------ | ------------------------------------------------------------ |
  | **r**  | 只读，默认模式，文件的指针将会放在文件的开头。文件必须存在，不存在则抛出异常 |
  | **w**  | 只写，文件的指针将会放在文件的开头，写之前会清空文件的内容。如果文件不存在，会创建新文件 |
  | **a**  | 追加的方式，文件的指针将会放在文件的结尾，在原本内容中继续写。如果文件不存在，则会创建新文件 |
  | **r+** | 可读可写                                                     |
  | **w+** | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
  | **a+** | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写 |
  | **b**  | rb、wb、ab、rb+、wb+、ab+ 意义和上面一样，用于二进制文件操作，例如图片、音频、视频等 |

- 返回值：返回一个对象，这个对象就代表了当前打开的文件。

示例：

```python
# 创建一个变量，来保存文件的名字
# 如果目标文件和当前文件在同一级目录下，则直接使用文件名即可
file_name = 'demo.txt'

# 在 Windows 系统使用路径时，可以使用 / 来代替 \
# 或者可以使用 \\ 来代替 \
# 或者也可以使用原始字符串
# file_name = 'hello\\demo.txt'
# file_name = r'hello\demo.txt'

# 表示路径，可以使用 .. 来返回一级目录
# file_name = '../hello/demo.txt'

# 如果目标文件距离当前文件比较远，此时可以使用绝对路径
# 绝对路径应该从磁盘的根目录开始书写
# file_name = r'C:\Users\XiSun\Desktop\hello.txt'

file_obj = open(file_name)  # 打开 file_name 对应的文件
print(file_obj)  # <_io.TextIOWrapper name='demo.txt' mode='r' encoding='cp936'>
```

### 关闭文件

```python
# 打开文件
file_name = 'demo.txt'

# 调用 open() 来打开文件
# file_obj = open(file_name)

# 当我们获取了文件对象以后，所有的对文件的操作都应该通过对象来进行

# 读取文件中的内容：
#   read() 方法，用来读取文件中的内容，它会将内容全部保存为一个字符串返回
# content = file_obj.read()
# print(content)

# 关闭文件：
#   调用 close() 方法来关闭文件
# file_obj.close()

# 上面的 open() 和 close() 是常规写法，可以用下面的方式简化操作
# with ... as 语句：
with open(file_name) as file_obj:
    # 在 with 语句中可以直接使用 file_obj 来做文件操作
    # 此时这个文件只能在 with 中使用，一旦 with 结束则文件会自动 close()
    print(file_obj.read())

# 进一步完善，添加文件读取异常处理
# 文件处理的标准格式：
file_name = 'hello.txt'

try:
    with open(file_name) as file_obj:
        print(file_obj.read())
except FileNotFoundError:
    print(f'{file_name} 文件不存在~~')
```

### 读取文件

```python
file_name = 'demo.txt'

try:
    # 调用 open() 来打开一个文件，可以将文件分成两种类型
    # 一种，是纯文本文件（使用 utf-8 等编码编写的文本文件）
    # 一种，是二进制文件（图片、mp3、ppt 等这些文件）
    # open() 打开文件时，默认是以文本文件的形式打开的，但是 open() 默认的编码为 None
    #   所以处理文本文件时，必须要指定文件的编码
    with open(file_name, encoding='utf-8') as file_obj:
        # 首先，判断文件是否可读，增加容错
        print(file_obj.readable())  # True

        # 通过 read() 来读取文件中的内容
        # 如果直接调用 read() 它会将文本文件的所有内容全部都读取出来
        #   如果要读取的文件较大的话，会一次性将文件的内容加载到内存中，容易导致内存泄漏
        #   所以对于较大的文件，不要直接调用 read()
        # help(file_obj.read) 查看帮助信息
        # read() 可以接收一个 size 作为参数，该参数用来指定要读取的字符的数量
        #   默认值为 -1，它会读取文件中的所有字符
        #   可以为 size 指定一个值，这样 read() 会读取指定数量的字符，
        #       每一次读取都是从上次读取到位置开始读取的
        #       如果字符的数量小于 size，则会读取剩余所有的
        #       如果已经读取到了文件的最后了，则会返回''空串
        # content = file_obj.read(-1)
        content = file_obj.read(6)
        content = file_obj.read(6)
        content = file_obj.read(6)
        content = file_obj.read(6)
        # print(content)
        # print(len(content))
except FileNotFoundError:
    print(f'{file_name} 这个文件不存在！')

# 读取大文件的方式
file_name = 'demo.txt'

try:
    with open(file_name, encoding='utf-8') as file_obj:
        # 定义一个变量，来保存文件的内容
        file_content = ''
        # 定义一个变量，来指定每次读取的大小
        chunk = 100
        # 创建一个循环来读取文件内容
        while True:
            # 读取 chunk 大小的内容
            content = file_obj.read(chunk)

            # 检查是否读取到了内容
            if not content:
                # 内容读取完毕，退出循环
                break

            # 输出内容
            # print(content, end='')
            file_content += content

except FileNotFoundError:
    print(f'{file_name} 这个文件不存在！')

print(file_content)
```

```python
import pprint

file_name = 'demo.txt'

try:
    with open(file_name, encoding='utf-8') as file_obj:
        # readline()：
        #   该方法可以用来读取一行内容
        # print(file_obj.readline(), end='')  # print() 打印自带换行符

        # readlines()：
        #   该方法用于一行一行的读取内容，它会一次性将读取到的内容封装到一个列表中返回
        # r = file_obj.readlines()
        # pprint.pprint(r[0])  # 'aaa\n'
        # pprint.pprint(r[1])  # 'bbb\n'
        # pprint.pprint(r[2])  # 'ccc\n'

        # file_obj 是一个可迭代对象
        print(isinstance(file_obj, collections.abc.Iterator))  # True

        # 简化写法，遍历 file_obj，也可以遍历 readlines() 方法返回的行列表
        for t in file_obj:
            print(t, end='')

except FileNotFoundError:
    print(f'{file_name} 这个文件不存在！')
```

### 写入文件

```python
file_name = 'demo1.txt'

# 使用 open() 打开文件时必须要指定打开文件所要做的操作：读、写、追加
#   如果不指定操作类型，则默认是 `读取文件` ，而读取文件时是不能向文件中写入的
# r：表示只读的
# w：表示是可写的，使用 w 来写入文件时，如果文件不存在会创建文件，如果文件存在则会截断文件
#       截断文件指删除原来文件中的所有内容（覆盖原文件内容）
# a：表示追加内容，如果文件不存在会创建文件，如果文件存在则会向文件中追加内容
# x：用来新建文件，如果文件不存在则创建，存在则报错
# +：为操作符增加功能
#   r+：即可读又可写，文件不存在会报错
#   w+：即可写又可读
#   a+：即可追加又可读
# with open(file_name , 'w' , encoding='utf-8') as file_obj:
# with open(file_name , 'r+' , encoding='utf-8') as file_obj:
with open(file_name, 'x', encoding='utf-8') as file_obj:
    # 首先，判断文件是否可读，增加容错
    print(file_obj.writable())  # True

    # write() 来向文件中写入内容
    # 如果操作的是一个文本文件的话，则 write() 需要传递一个字符串作为参数
    # 该方法会可以分多次向文件中写入内容
    # 写入完成以后，该方法会返回写入的字符的个数
    file_obj.write('aaa\n')
    file_obj.write('bbb\n')
    file_obj.write('ccc\n')
    file_obj.write(str(123) + '123123\n')  # 要转换为字符串，且不会自动换行
    r = file_obj.write('今天天气真不错')
    print(r)  # 7
```

### 二进制文件操作

```python
file_name = 'F:/QQMusic/FLOW - Go!!!.flac'

# 读取模式：
#   t：读取文本文件（默认值）
#   b：读取二进制文件

with open(file_name, 'rb') as file_obj:
    # 读取文本文件时，size 是以字符为单位的
    # 读取二进制文件时，size 是以字节为单位
    # print(file_obj.read(100))

    # 将读取到的内容写出来
    # 定义一个新的文件
    new_name = 'aa.flac'

    with open(new_name, 'wb') as new_obj:

        # 定义每次读取的大小
        chunk = 1024 * 100  # 100 KB

        while True:
            # 从已有的对象中读取数据
            content = file_obj.read(chunk)

            # 内容读取完毕，终止循环
            if not content:
                break

            # 将读取到的数据写入到新对象中
            new_obj.write(content)
```

### seek() 和 tell()

```python
# 二进制文件
with open('demo.txt', 'rb') as file_obj:
    # print(file_obj.read(30))
    # print(file_obj.read(100))

    # tell() 方法用来查看当前读取的位置
    print('当前读取到了 -->', file_obj.tell())

    # seek() 方法可以修改当前读取的位置
    # seek() 需要两个参数：
    #   第一个：要切换到的位置
    #   第二个：计算位置的方式
    #       可选值：
    #           0 从头计算，默认值
    #           1 从当前位置计算
    #           2 从最后位置开始计算
    # file_obj.seek(55)
    # file_obj.seek(80, 0)
    # file_obj.seek(70, 1)
    file_obj.seek(-10, 2)  # 从文件的最后往前读 10 个字符
    print(file_obj.read())

# 文本文件
with open('demo.txt', 'rt', encoding='utf-8') as file_obj:
    print('当前读取到了 -->', file_obj.tell())

    file_obj.seek(9)  # 从头开始计算，切换到第 9 个字节开始读取
    print(file_obj.read())
```

- 文本文件的操作模式下，seek() 方法的第二个参数只能写 0。
- 如果希望 seek() 方法的第二个蚕食是 1 或者 2，必须在二进制文件的操作模式下。

### 文件的其他操作

```python
import os
from pprint import pprint

# os.listdir()：获取指定目录的目录结构
#   需要一个路径作为参数，会获取到该路径下的目录结构，默认路径为 . ，即当前目录
#   该方法会返回一个列表，目录中的每一个文件（夹）的名字都是列表中的一个元素
r = os.listdir()
pprint(r)  # ['.idea', 'aa.flac', 'demo.txt', 'hello', 'main.py', '__pycache__']

# os.getcwd()：获取当前所在的目录
r = os.getcwd()
pprint(r)  # 'D:\\JetBrainsWorkSpace\\PycharmProjects'

# os.chdir()：切换当前所在的目录 作用相当于cd命令
# os.chdir('c:/')
# r = os.getcwd()
# pprint(r)  # 'c:\\'

# os.mkdir()；创建目录
os.mkdir("aaa")  # 在当前目录下创建一个名字为 aaa 的目录，如果已存在，则报错

# os.rmdir()：删除目录
os.rmdir('aaa')  # 在当前目录下删除一个名字为 aaa 的目录，如果不存在，则报错

# open()；打开文件
open('aa.txt', 'w')

# os.remove()：删除文件
os.remove('aa.flac')

# os.rename('旧名字','新名字')：可以对一个文件进行重命名，也可以用来移动一个文件
os.rename('hello', 'bb.txt')
# os.rename('bb.txt', 'c:/users/lilichao/desktop/bb.txt')

pprint(r)
```

### csv 文件

```python
import csv, random
from my_package import my_tools

lista = []


def random_info(n=100):
    subjects = ['python', 'java', 'C++', 'html']
    names = []
    for i in range(n // len(subjects)):
        name = my_tools.random_string(random.randint(3, 6))
        names.append(name)
    for i in range(n):
        subject = random.choice(subjects)
        score = random.randint(50, 100)
        name = random.choice(names)
        for j in lista:
            if j[0] == name and j[1] == subject:
                break
        else:
            lista.append([name, subject, score])


def average():
    with open('data.csv', mode='r', encoding='utf-8') as f:
        cf = csv.reader(f)
        head = next(cf)  # 获取表头
        scores = []
        for i in cf:
            scores.append(int(i[2]))
        return sum(scores) / len(scores)


def make_datas():
    with open('data.csv', mode='a', encoding='utf-8') as f:
        cf = csv.writer(f)
        random_info()
        cf.writerows(lista)


make_datas()
# result = average()
# print('大家的平均分是',round(result,2))
```

data.csv：

```tex
hTRI,java,83
kjRfoP,python,94
KhXf,python,56
VAytb,python,77
QXvn,python,99
jGNTql,html,72
JDkeB,C++,54
mWumj,html,87
ZqeOSv,java,63
RPV,java,65
FAug,python,88
kjRfoP,C++,54
KhXf,python,74
hTRI,java,96
dJruAL,python,94
hTRI,html,85
jGNTql,html,80
KBsbou,C++,83
nmwWP,python,53
zDPclC,java,62
YRhNw,html,83
gVw,python,72
KhXf,python,63
FAug,C++,86
KhXf,C++,54
ifNLzB,java,84
qOl,java,51
vWBbN,C++,64
LCF,html,91
ZqeOSv,html,64
RPV,C++,87
RPV,html,53
vWBbN,python,87
YRhNw,java,52
FAug,python,78
gVw,html,88
LCF,java,63
lfL,html,65
FAug,html,61
KBsbou,html,89
ifNLzB,C++,69
vWBbN,java,61
hTRI,python,54
QXvn,java,55
rIgeHN,C++,59
qOl,java,51
ijIB,java,92
LCF,python,67
YRhNw,java,55
YRhNw,html,66
JDkeB,python,77
YRhNw,python,69
LCF,C++,66
qOl,java,84
hTRI,html,60
ZqeOSv,html,78
lfL,python,55
KhXf,C++,83
nmwWP,html,57
gVw,java,100
VAytb,C++,74
lfL,C++,90
VAytb,C++,66
nmwWP,html,84
rIgeHN,C++,84
lfL,html,65
mWumj,python,62
LCF,html,70
Vgd,python,52
ZqeOSv,java,67
dJruAL,java,66
gVw,python,66
VAytb,java,76
ifNLzB,python,55
YRhNw,java,98
qOl,html,66
RPV,html,61
jGNTql,html,90
qOl,python,60
lfL,html,54
dJruAL,java,94
FAug,C++,84
KhXf,html,90
LCF,C++,96
gVw,C++,70
dJruAL,java,54
QXvn,java,50
kjRfoP,python,88
ifNLzB,python,80
ZqeOSv,C++,100
zDPclC,C++,81
mWumj,html,86
LCF,java,85
VAytb,C++,77
mWumj,html,74
nmwWP,java,92
LCF,C++,78
rIgeHN,html,94
mWumj,java,77
vWBbN,html,66
```

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/PythonLanguage/python.md