> *`Author: ACatSmiling`*
>
> *`Since: 2024-09-27`*

## 基础语法

Python 的语法比较简单，采用缩进方式，写出来的代码就像下面的样子：

```python
# print absolute value of an integer:
a = 100
if a >= 0:
    print(a)
else:
    print(-a)
```

- Python 程序是**`大小写敏感`**的，如果写错了大小写，程序会报错。
- Python 中的每一行就是一条语句，每条语句以换行结束，每一行语句不要过长（规范中建议每行不要超过 80 个字符）。**当语句以冒号`:`结尾时，缩进的语句视为代码块。**
- Python 中一条语句可以分多行编写，多行编写时语句后边以`\`结尾。
- Python 是**`缩进严格`**的语言，所以在 Python 中不要随便写缩进。按照约定俗成的惯例，**应该始终坚持使用 4 个空格的缩进**。当重构代码时，粘贴过去的代码必须重新检查缩进是否正确。此外，IDE 很难像格式化 Java 代码那样格式化 Python 代码。

## 注释

Python 中注释分为：

1. 块注释。

   ```python
   # 块注释
   print('我在学习python')
   ```

   - 以**`#`**开始，一直到本行结束都是注释。
   - 为了保证代码的可读性，# 后面建议先添加一个空格，然后再编写相应的说明文字（PEP8）。
   - 注释不会运行。
   - 代码是给机器执行用的，注释是给人看的，方便阅读代码时，能够快速的了解代码的功能。

2. 行内注释。

   ```python
   print('我在学习python')  # 行内注释，与代码同行，# 前面至少有两个空格
   ```

   - 以 # 开始，一直到本行结束都是注释。
   - 与代码写在同一行，# 前面至少有两个空格。

3. 多行注释。

   ```python
   """
       我是多行注释
       我是多行注释
   """
   print('我在学习python')
   
   '''
       我是多行注释
       我是多行注释
   '''
   print('我在学习python')
   ```

   - 使用三个单引号**`'''`**表示多行注释，中间的任何内容都是注释，可以有多行，但中间不能再出现三个单引号。
   - 使用三个双引号**`"""`**表示多行注释，中间的任何内容都是注释，可以有多行，但中间不能再出现三个双引号。

## print() 函数

**`print()`**：输出/打印指定内容。语法：

```python
print(*objects, sep=' ', end='\n', file=None, flush=False)
```

- **在一行中打印多个内容，使用逗号隔开。**
- `sep`：设置多个内容之间的分隔符，**默认分隔符是空格**。
- `end`：设置结束符，**默认结束符是 '\n'**。
- `file`：表示输出的目标，默认是标准的输出（控制台），也可以设置为一个可写入的文件句柄。
- `flush`：表示立即输出，即从缓存立即刷新，默认 False。

示例：

```python
# 打印数字
print(2024)

# 打印字符串
print('我是mia')

# 打印变量
year = 2024
print(year)

# 在一行中打印多个内容，中间以 , 隔开
print(year, '年，我要读100本书')
print(year, '年，我要去10个城市旅游')

# 设置分隔符为空，结束符为双换行
print(year, '年，我要读100本书', sep='', end='\n\n')
print(year, '年，我要去10个城市旅游', sep='', end='\n\n')

# 格式化输出
name = 'xxx'
age = 15
print("我的名字是%s，年龄是%d" % (name, age))
print("我的名字是{0}，年龄是{1}".format(name, age))
```

输出结果：

```python
2024
我是mia
2024
2024 年，我要读100本书
2024 年，我要去10个城市旅游
2024年，我要读100本书

2024年，我要去10个城市旅游

我的名字是xxx，年龄是15
我的名字是xxx，年龄是15
```

### 格式化字符串

**`%`**：格式化操作符，专门用于处理字符串中的格式。语法：

```python
print("格式化字符串" % (变量1, 变量2...))
```

- 包含 % 的字符串，被称为**格式化字符串**。

- % 和不同的字符连用，不同类型的数据需要使用不同的格式化字符。

  | **格式化字符** | **含义**                                                     |
  | -------------- | ------------------------------------------------------------ |
  | `%s`           | 字符串                                                       |
  | `%d`           | 有符号十进制整数。例如：%06d 表示输出的整数显示 6 位数，不足的地方使用 0 补全 |
  | `%f`           | 浮点数。例如：%.2f 表示小数点后只显示两位                    |
  | `%%`           | 输出 %                                                       |

示例：

```python
# 格式化输出
month = 2
date = 2
day = '一'
weather = '晴'
temp = 15.41
print('今天是 %d 年 %02d 月 %02d 日，星期 %s ，今天的天气 %s ，气温 %.1f 度' % (year, month, date, day, weather, temp))
```

输出结果：

```python
今天是 2024 年 02 月 02 日，星期 一 ，今天的天气 晴 ，气温 15.4 度
```

## input() 函数

**`input()`**：输入指定内容。语法：

```python
变量 = input("提示信息：")
```

- 所谓输入，就是用代码获取用户通过键盘输入的信息。
- 在 Python 中，如果要获取用户在键盘上的输入信息，需要使用到 input() 函数。
- 用户输入的任何内容，Python 都认为是一个字符串。

示例：

```python
# 获取输入的姓名
name = input('请你输入你的名字：')
print('你输入的是：', name)

# 计算你的出生年份
year = 2024
age = input('请输入你的年龄：')
age = int(age)
print('你的出生年份是%d'% (year - age))
```

输出结果：

```python
请你输入你的名字：dsa
你输入的是： dsa
请输入你的年龄：15
你的出生年份是2009
```

## 字面量和变量

**`字面量`**：就是一个一个的值，比如：1，2，3，4，5，6，'HELLO'。字面量所表示的意思就是它的字面的值，在程序中可以直接使用字面量。

**`变量（variable）`**：可以用来保存字面量，并且变量中保存的字面量是不定的。变量本身没有任何意思，它会根据不同的字面量表示不同的意思。语法：

```python
变量名 = 变量值
```

- 变量的特点：

  - **Python 中使用变量，不需要声明，直接为变量赋值即可。**
  - Python 中**不能使用没有进行过赋值的变量**，如果使用没有赋值过的变量，会报错 "NameError: name 'b' is not defined"。
  - Python 是一个动态类型的语言，可以为变量赋任意类型的值，也可以任意修改变量的值。
  
- 在 Python 中，`等号`是赋值语句，**可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量。**

  ```python
  a = 123  # a 是整数
  print(a)  # 123
  a = 'ABC'  # a 变为字符串
  print(a)  # ABC
  ```

  - **这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。**静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。例如 Java 是静态语言。

- 等号也可以把一个变量 a 赋值给另一个变量 b，这个操作实际上是把变量 b 指向变量 a 所指向的数据。

  ```python
  a = 'ABC'
  b = a
  a = 'XYZ'
  print(b)  # ABC，不是 XYZ
  ```

- `常量`：就是不能变的变量，比如常用的数学常数 π 就是一个常量。在 Python 中，通常用全部大写的变量名表示常量：

  ```python
  PI = 3.14159265359
  ```

  - 事实上，PI 仍然是一个变量，Python 根本没有任何机制保证 PI 不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量 PI 的值，也没人能拦住你。

> 一般在开发时，很少直接使用字面量，都是将字面量保存到变量中，再通过变量来引用字面量。

## 标识符

**`标识符`**：Python 中所有可以自主命名的内容都属于标识符，比如变量名、函数名、类名。

标识符的命名规范：

- 标识符中**可以含有字母、数字、_，但是不能使用数字开头。**

- 标识符**不能是 Python 中的关键字和保留字**，也不建议使用 Python 中的函数名作为标识符，因为这样会导致函数被覆盖。

  ![image-20240928091427597](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240928144548934-538602746.png)

- 在 Python 中注意遵循两种命名规范：
  - `下划线命名法`：所有字母小写，单词之间使用 _ 分割。如：max_length、min_length、hello_world。
  - `帕斯卡命名法`：大驼峰命名法，首字母大写，每个单词开头字母大写，其余字母小写。如：MaxLength、MinLength、HelloWorld。
  
- 如果使用不符合标准的标识符，会报错 "SyntaxError: invalid syntax"。

## 类型检查

**`类型检查`**：可以检查变量对应的值的类型。

- `type()`：检查值的类型，该函数会将检查的结果作为返回值返回，可以通过变量来接收函数的返回值。示例：

  ```python
  a = 123  # 数值
  b = '123'  # 字符串
  print(type(a))  # <class 'int'>
  print(type(b))  # <class 'str'>
  c = type(a)
  print(c)  # <class 'int'>
  c = type('123')
  print(c)  # <class 'str'>
  print(type(1))  # <class 'int'>
  print(type(1.5))  # <class 'float'>
  print(type(True))  # <class 'bool'>
  print(type('hello'))  # <class 'str'>
  print(type(None))  # <class 'NoneType'>
  ```

- `isinstance()`：判断值的类型，如果类型一致，返回 True，否则返回 False。示例：

  ```python
  from types import NoneType
  
  a = 123  # 数值
  b = '123'  # 字符串
  print(isinstance(a, int))  # True
  print(isinstance(b, str))  # True
  c = isinstance(a, int)
  print(c)  # True
  c = isinstance('123', str)
  print(c)  # True
  print(isinstance(1, int))  # True
  print(isinstance(1.5, float))  # True
  print(isinstance(True, bool))  # True
  print(isinstance('hello', str))  # True
  print(isinstance(None, NoneType))  # True
  ```

## 类型转换

**`类型转换`**：将一个类型的对象转换为其他对象。

- **类型转换不是改变对象本身的类型，而是根据当前对象的值来创建一个新对象。**

- 类型转换四个函数：`int()`，`float()`，`str()`，`bool()`。

- `int()`：可以用来将其他的对象转换为整型。

  ```python
  a = True
  a = int(a)
  print(a)  # 1
  
  a = False
  a = int(a)
  print(a)  # 0
  
  a = '123'
  a = int(a)
  print(a)  # 123
  
  a = 11.6
  a = int(a)
  print(a)  # 11
  
  a = '11.5'
  a = int(a)  # ValueError
  print(a)
  
  a = None
  a = int(a)  # TypeError
  print(a)
  ```

  - 布尔值：True ---> 1，False ---> 0。
  - 浮点数：直接取整，省略小数点后的内容。
  - 字符串：如果是一个合法的整数字符串，则直接转换为对应的数字。如果不是一个合法的整数字符串，则报错 "ValueError: invalid literal for int() with base 10: '11.5'"。
  - 对于其他不可转换为整型的对象，直接抛出异常 TypeError。
  - int() 数不会对原来的变量产生影响，它是将对象转换为指定的类型并将其作为返回值返回，如果希望修改原来的变量，则需要对变量进行重新赋值。

- `float()`：和 int() 基本一致，不同的是它会将对象转换为浮点数。

  ```python
  a = 1
  a = float(a)
  print(a)  # 1.0
  
  a = False
  a = float(a)
  print(a)  # 0.0
  
  a = True
  a = float(a)
  print(a)  # 1.0
  ```

- `str()`：可以将对象转换为字符串。

  ```python
  a = 123
  a = str(a)
  print(a)  # 123
  
  b = 456
  print('hello' + str(b))  # hello456
  ```

  - True ---> 'True'。
  - False ---> 'False'。
  - 123 ---> '123' 。
  
- `bool()`：可以将对象转换为布尔值，任何对象都可以转换为布尔值。

  ```python
  a = None
  a = bool(a)
  print(a)  # False
  
  a = 0
  a = bool(a)
  print(a)  # False
  
  a = ''
  a = bool(a)
  print(a)  # False
  ```

  - 规则：**对于所有表示空性的对象都会转换为 False，其余的转换为 True。**表示空性的对象：0、None、''等。

## 运算符（操作符）

`运算符`：可以对一个值或多个值进行运算或各种操作。比如 +、-、= 都属于运算符。

常用运算符的分类：

- 算术运算符
- 赋值运算符
- 关系运算符（比较运算符）
- 逻辑运算符
- 条件运算符（三元运算符）  

### 算术运算符

**`算术运算符`**：

- 加法运算符：`+`。如果是两个字符串之间进行加法运算，则会进行拼串操作。

- 减法运算符：`-`。

- 乘法运算符：`*`。如果将字符串和数字相乘，则会对字符串进行复制操作，将字符串重复指定次数。

- 除法运算符：`/`。运算时结果总会返回一个浮点类型。

- 幂运算：`**`。求一个值的几次幂。

- 整除：`//`。向下取整，只会保留计算后的整数位，总会返回一个整型。

- 取模：`%`。即取余，求两个数相除的余数。

- 运算时，注意正负号的问题。

- 在对浮点数做算术运算时，结果也会返回一个浮点数。

示例：

```python
# 加法运算符
a = 10 + 5  # 15
a = 'hello' + ' ' + 'world'  # hello world，拼串

# 减法运算符
a = 10 - 5  # 5
a = 5 - True  # 4
a = a - 2  # 2，用变量 a 的值减去 2，然后再赋值给 a
# a = 'hello' - 'h'  # TypeError

# 乘法运算符
a = 5 * 5  # 25

# 除法运算符
a = 10 / 5  # 2.0
a = 5 / 2  # 2.5
# a = 5 / 0  # ZeroDivisionError: division by zero
a = 10 / 3  # 3.33333...

# 幂运算
a = 2 ** 2  # 4
a = 10 ** 5  # 100000
a = 16 ** 0.5  # 4.0，求 16 的平方根

# 整除
a = 10 // 3  # 3
a = 5 // 2  # 2
a = 10 // -3  # -4
a = -5 // 2  # -3

# 取模
a = 10 % 5  # 0
a = 10 % 4  # 2
a = 10 % 3  # 1
a = 10 % 2  # 0

print("a =", a)

# 在对浮点数做算术运算时，结果也会返回一个浮点数
b = 25.0 / 5
print(b)  # 5.0
```

### 赋值运算符

**`赋值运算符`**：

- `=`：可以将等号右侧的值赋值给等号左侧的变量。
- `+=`：a += 5 相当于 a = a + 5。
- `-=`：a -= 5 相当于 a = a - 5。
- `*=`：a *= 5 相当于 a = a * 5。
- `/=`：a /= 5 相当于 a = a / 5。
- `**=`：a \**= 5 相当于 a = a ** 5。
- `//=`：a //= 5 相当于 a = a // 5。
- `%=`：a %= 5 相当于 a = a % 5。

### 关系运算符（比较运算符）

**`关系运算符`**：用来比较两个值之间的关系，总会返回一个布尔值。如果关系成立，返回 True，否则返回 False。

- `>`：比较左侧值是否大于右侧值。
- `>=`：比较左侧的值是否大于或等于右侧的值。
- `<`：比较左侧值是否小于右侧值。
- `<=`：比较左侧的值是否小于或等于右侧的值。
- `==`：比较两个对象的值是否相等，比较的是对象的值。
- `!=`：比较两个对象的值是否不相等，比较的是对象的值。
- `is`：比较两个对象是否是同一个对象，**比较的是对象的 id**。
- `is not`：比较两个对象是否不是同一个对象，**比较的是对象的 id**。

示例：

```python
result = 10 > 20  # False
result = 10 >= 10  # True
result = 2 > True  # True
# result = 2 > '1'  # TypeError: '>' not supported between instances of 'int' and 'str'
result = 30 < 20  # False

# 在 Python 中可以对两个字符串进行大于（等于）或小于（等于）的运算，
#   当对字符串进行比较时，实际上比较的是字符串的 Unicode 编码
#   比较两个字符串的 Unicode 编码时，是逐位比较的，比较出现结果时，直接返回，后续不再比较
#   利用该特性可以对字符串按照字母顺序进行排序，但是对于中文来说意义不是特别大
#   注意：如果不希望比较两个字符串的 Unicode 编码，则需要将其转换为数字然后再比较

result = '2' > '1'  # True，2 的 Unicode 编码为 0032，1 的 Unicode 编码为 0031
result = '2' > '11'  # True，逐位比较，先拿 2 和 1 比，结果为 True，直接返回，后续不再比较
result = 'a' > 'b'  # False，a 的 Unicode 编码为 0061，b 的 Unicode 编码为 0062
result = 'c' < 'd'  # True
result = 'ab' > 'b'  # False，逐位比较，先拿 a 和 b 比，结果为 False，直接返回，后续不再比较

print(int('2') > int('11'))  # False

result = 1 == 1  # True
result = 'hello' == 'hello'  # True
result = 'abc' == 'bcd'  # False
result = 'abc' != 'bcd'  # True
result = 1 == True  # True
result = 1 is True  # False
result = 1 is not True  # True
print(id(1), id(True))  # 140708473153312 140708472870736
```

### 逻辑运算符

**`逻辑运算符`**：主要用来做一些逻辑判断。

- `and`：逻辑与。

  - and 可以对符号两侧的值进行与运算。

  - 只有在符号两侧的值都为 True 时，才会返回 True，只要有一个 False 就返回 False。

  - Python 中的与运算是**短路的与**，如果第一个值为 False，则不再看第二个值，直接返回 False。

    ```python
    True and print('男：你猜我出来吗？')  # 第一个值是 True，会看第二个值，所以 print() 会执行
    False and print('女：你猜我出来吗？')  # 第一个值是 False，不会看第二个值，所以 print() 不会执行
    ```

- `or`：逻辑或。

  - or 可以对符号两侧的值进行或运算。

  - 或运算两个值中只要有一个 True，就会返回 True。

  - Python 中的或运算是**短路的或**，如果第一个值为 True，则不再看第二个值，直接返回 True。

    ```python
    False or print('男：你猜我出来吗？')  # 第一个值为 False，继续看第二个，所以打印语句执行
    True or print('女：你猜我出来吗？')  # 第一个值为 True，不看第二个，所以打印语句不执行
    ```

- `not`：逻辑非。

  - not 可以对符号右侧的值进行非运算。
  - 对于布尔值，非运算会对其进行取反操作，True 变 False，False 变 True。
  - 对于非布尔值，非运算会先将其转换为布尔值，然后再取反。

- `非布尔值的与或运算`：当我们对非布尔值进行与或运算时，Python 会将其当做布尔值运算，最终会返回原值。

  - 与运算的规则：

    ```python
    # True and True
    result = 1 and 2  # 2
    
    # True and False
    result = 1 and 0  # 0
    
    # False and True
    result = 0 and 1  # 0
    
    # False and False
    result = None and 0  # None
    ```

    - 与运算是找 False 的，如果第一个值是 False，则不看第二个值。
    - **如果第一个值是 False，则直接返回第一个值，否则返回第二个值。**

  - 或运算的规则：

    ```python
    # True or True
    result = 1 or 2  # 1
    
    # True or False
    result = 1 or 0  # 1
    
    # False or True
    result = 0 or 1  # 1
    
    # False or False
    result = None or 0  # 0
    ```

    - 或运算是找 True 的，如果第一个值是 True，则不看第二个值。
    - **如果第一个值是 True，则直接返回第一个值，否则返回第二个值。**

### 条件运算符（三元运算符）

语法：

```python
语句1 if 条件表达式 else 语句2
```

- 条件运算符在执行时，会先对条件表达式进行求值判断。如果判断结果为 True，则执行语句 1，并返回执行结果；如果判断结果为 False，则执行语句 2，并返回执行结果。

示例：

```python
a = 30
b = 50
print('a的值比较大！') if a > b else print('b的值比较大！')

# 获取 a 和 b 之间的较大值
maxNum = a if a > b else b
print(maxNum)
```

### 位运算符

示例：

```python
# 按位与 &
'''
计算规则：
    101
    111
    ----
    101
'''
print(5 & 7)  # 5

# 按位或 |
'''
计算规则：
    011
    100
    ----
    111
'''
print(3 | 4)  # 7

# 按位异或 ^
'''
计算规则：
    010
    100
    ----
    110
'''
print(2 ^ 4)  # 6

# 按位取反 ~
'''
计算规则：
    01
    ---
    10
'''
print(~1)  # -2

# 左移 右移
'''
计算规则：
    101
    ----
    10100
'''
print(5 << 2)  # 20
```

### 成员运算符

示例：

```python
# in 和 not in
print('12' in '123')  # True
print('hi' not in 'hello')  # True

# is 和 is not
a = 1
b = 2
print(a is b)  # False
print(a is not b)  # True
```

## 对象初识

**Python 是一门面向对象的语言。**

- 一切皆对象！
- 程序运行当中，所有的数据都是存储到内存当中然后再运行的！
- 对象就是内存中专门用来存储指定数据的一块区域。
- 对象实际上就是一个容器，专门用来存储数据。
- 数值、字符串、布尔值、None 都是对象。

### 对象的结构

每个对象中都要保存三种数据：

- **`id（标识）`**

  - id 用来标识对象的唯一性，每一个对象都有唯一的 id。

  - 对象的 id 就相当于人的身份证号一样。

  - **可以通过`id()`函数来查看对象的 id。**

    ```python
    print(id(123))  # 140708231919200
    print(id('a'))  # 2270491024176
    ```

  - id 是由解析器生成的，在 CPython 中，id 就是对象的内存地址。

  - 对象一旦创建，则它的 id 永远不能再改变。

- **`type（类型）`**

  - 类型用来标识当前对象所属的类型，比如：int，str，float，bool。

  - 类型决定了对象有哪些功能。

  - **通过`type()`函数来查看对象的 type。**

    ```python
    print(type(123))  # <class 'int'>
    print(type('a'))  # <class 'str'>
    ```

  - Python 是一门强类型的语言，对象一旦创建类型便不能修改。

- **`value（值）`**

  - 值就是对象中存储的具体的数据。
  - 对于有些对象，值是可以改变的。

### 对象的可变性

在 Python 中，对象分成两大类：不可变对象和可变对象。

1. **`不可变对象`**：指一旦创建，其值就不能被改变的对象。如果尝试修改不可变对象，实际上会创建一个新的对象，而原对象保持不变。

   - **常见的不可变对象有整数（int）、浮点数（float）、字符串（str）和元组（tuple）。**

   - 示例：

     ```python
     # 整数
     a = 5
     b = a
     b = 10
     print(a)  # 输出 5，说明整数对象不可变，给 b 重新赋值不会影响 a 的值
     
     # 字符串
     s = "hello"
     t = s
     t = "world"
     print(s)  # 输出 "hello"，说明字符串不可变，修改 t 不会影响 s 的值
     
     # 元组
     my_tuple = (1, 2, 3)
     try:
         my_tuple[0] = 4  # 会抛出 TypeError 错误，元组不可变，不能修改其元素
     except TypeError as e:
         print(f"Error: {e}")
     ```

2. **`可变对象`**：指可以在创建后修改其值而不创建新对象的对象。

   - **常见的可变对象有列表（list）、字典（dict）和集合（set）。**

   - 示例：

     ```python
     # 列表
     my_list = [1, 2, 3]
     another_list = my_list
     another_list.append(4)
     print(my_list)  # 输出 [1, 2, 3, 4]，说明列表是可变对象，修改 another_list 也会影响 my_list 的值
     
     # 字典
     my_dict = {'key': 'value'}
     new_dict = my_dict
     new_dict['new_key'] = 'new_value'
     print(my_dict)  # 输出 {'key': 'value', 'new_key': 'new_value'}，说明字典是可变对象，修改 new_dict 也会影响 my_dict 的值
     
     # 集合
     my_set = {1, 2, 3}
     another_set = my_set
     another_set.add(4)
     print(my_set)  # 输出 {1, 2, 3, 4}，说明集合是可变对象，修改 another_set 也会影响 my_set 的值
     ```

对象可变性的影响和注意事项：

1. **函数参数传递**：
   - 当不可变对象作为函数参数传递时，在函数内部对参数的修改不会影响到外部的变量，因为是值的传递（**实际上是对象引用的传递，只是因为不可变对象一旦被修改就会创建新的对象，所以看起来像是值传递**），函数内部操作的是参数的副本。
   - 当可变对象作为函数参数传递时，在函数内部对参数的修改可能会影响到外部的变量，因为传递的是对象的引用。
2. **赋值和拷贝**：
   - 对于不可变对象，多个变量可以指向同一个对象，因为不可变对象的值不会被修改，所以不会出现数据不一致的问题。
   - 对于可变对象，多个变量指向同一个对象时，需要注意在一个变量上的修改可能会影响到其他变量。如果需要创建一个独立的副本，可以使用深拷贝或浅拷贝的方法，具体取决于对象的结构和需求。

### 对象和变量的关系

对象并没有直接存储到变量中，在 Python 中变量更像是给对象起了一个别名。

- **变量中存储的不是对象的值，而是对象的 id（内存地址）。**当我们使用变量时，实际上就是在通过对象 id 在查找对象。

- 变量中保存的对象，只有在为变量重新赋值时才会改变。

- 变量和变量之间是相互独立的，修改一个变量不会影响另一个变量。

  ```python
  a = 10
  b = 10
  print(a)  # 10
  print(b)  # 10
  print(id(a))  # 140708432125984
  print(id(b))  # 140708432125984
  a = 456
  print(a)  # 456
  print(b)  # 10
  print(id(a))  # 2898418807216
  print(id(b))  # 140708432125984
  ```

### 对象的比较

两种比较的方式：

- `==`和`!=`：比较的是对象的值是否相等。
- `is`和`is not`：比较的是对象的 id 是否相等（比较两个对象是否是同一个对象）。

示例：

```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a, b)  # [1, 2, 3] [1, 2, 3]
print(id(a), id(b))  # 2081830016896 2081830015936
print(a == b)  # a 和 b 的值相等，使用 == 会返回 True
print(a is b)  # a 和 b 不是同一个对象，内存地址不同，使用 is 会返回 False
```

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/PythonLanguage/python.md