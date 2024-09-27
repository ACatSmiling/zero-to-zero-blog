>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-27`*

## 异常

程序在运行过程当中，不可避免的会出现一些错误，比如：使用了没有赋值过的变量，使用了不存在的索引，除 0 等。这些错误在程序中，我们称其为**`异常`**。

-  程序运行过程中，一旦出现异常将会导致程序立即终止，异常以后的代码全部都不会执行！

### 处理异常

程序运行时出现异常，目的并不是让我们的程序直接终止！Python 是希望在出现异常时，我们可以编写代码来对异常进行处理！

**`try 语句`**：

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

- try 是必须的，else 语句有没有都行，except 和 finally 至少有一个。

- 可以将可能出错的代码放入到 try 语句，这样如果代码没有错误，则会正常执行，如果出现错误，则会执行 expect 子句中的代码，这样我们就可以通过代码来处理异常，避免因为一个异常导致整个程序的终止。

### 异常的传播

当在函数中出现异常时，如果在函数中对异常进行了处理，则异常不会再继续传播，如果函数中没有对异常进行处理，则异常会继续向函数调用处传播，如果函数调用处处理了异常，则不再传播，如果没有处理则继续向调用处传播，直到传递到全局作用域（主模块），如果依然没有处理，则程序终止，并且显示异常信息。

当程序运行过程中出现异常以后，所有的异常信息会被保存一个专门的异常对象中，而异常传播时，实际上就是异常对象抛给了调用处。比如：

- **ZeroDivisionError 类的对象专门用来表示除 0 的异常。**
- **NameError 类的对象专门用来处理变量错误的异常。**
- 在 Python 为我们提供了多个异常对象。

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

可以使用`raise 语句`来抛出异常，raise 语句后需要跟一个异常类或异常的实例。

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

通过 Python 程序来对计算机中的各种文件进行增删改查的操作。

操作文件的步骤：

- 打开文件。
- 对文件进行各种操作（读、写），然后保存。
- 关闭文件。

### 打开文件

```python
# open(file, mode='r', buffering=-1, encoding_=None, errors=None, newline=None, closefd=True, opener=None)
# 使用 open() 函数来打开一个文件
# 参数：
#   file 要打开的文件的名字（路径）
# 返回值：
#   返回一个对象，这个对象就代表了当前打开的文件

# 创建一个变量，来保存文件的名字
# 如果目标文件和当前文件在同一级目录下，则直接使用文件名即可
file_name = 'demo.txt'

# 在 windows 系统使用路径时，可以使用 / 来代替 \
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

        # 简化写法
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

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/PythonLanguage/python.md