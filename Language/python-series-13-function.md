> *`Author: ACatSmiling`*
>
> *`Since: 2025-02-20`*

## 函数的基本使用

函数的简单定义：

```python
def 函数名():
    函数体
```

函数的调用：

```python
函数名()
```

## 函数的参数

**单个参数**：

```python
# 定义，形参
def 函数名(参数名):
    函数体
    
# 调用，实参
函数名(参数值)
```

**多个参数**：

```python
# 定义，形参以逗号分割
def 函数名(参数名1, 参数名2):
    函数体
    
# 调用方式一，实参以逗号分割，实参与形参一一对应
函数名(参数值1, 参数值2)

# 调用方式二，指明形参对应的实参名称，此时，可以不必按照形参的顺序
函数名(参数名1=参数值1, 参数名2=参数值2)
```

**不定长参数**：

```python
# 方式一
# 定义，形参名前加一个 * 号表示不定长参数，在函数体中，可以直接以元组变量的方式使用该参数
def 函数名(*参数名):
    函数体
    
# 调用，根据实际需求，传递对应的实参
函数名(参数值1, 参数值2, 参数值3, ...)


# 方式二
# 定义，形参名前加两个 ** 号表示不定长参数，在函数体中，可以直接以字典变量的方式使用该参数
def 函数名(**参数名):
    函数体
    
# 调用，根据实际需求，传递对应的实参名称与实参
函数名(参数名1=参数值1, 参数名2=参数值2, 参数名3=参数值3, ...)
```

> 补充：装包和拆包。

**缺省参数**：

```python
# 定义，形参给予默认值，有默认值的形参，在调用时可以不用给定实参
def 函数名(参数名1=默认值1, 参数名2=默认值2):
    函数体

# 调用，如果没给定参数值，会直接在函数体中使用默认值
函数名(参数值1, 参数值2)
```

**参数传递机制**：

- `值传递`：函数接收的是变量的副本，在函数内部参数的改变，不会影响函数外部的变量。

- `引用传递`：函数接收的是变量的引用（内存地址），在函数内部参数的改变，会影响函数外部的变量。

- **在 Python 中，是一种对象引用传递，也叫传值引用。如果数据类型是不可变类型（如整数、浮点数、字符串、元组等），传递的是对象的值的副本，在函数内部对参数的修改，不会影响函数外部的变量，此时，表现得像值传递；如果数据类型是可变类型（如列表、字典、集合等），传递的是对象的引用，在函数内部对参数的修改，会影响函数外部的变量，此时，表现得像引用传递。**

  ```python
  # 参数是不可变类型
  def change(num):
      print(id(num))  # 2034374541840
      print(num)  # 10
      num = 666
      print(id(num))  # 2034374556880，修改方法中 num 的值后，num 发生了变化
  
  
  b = 10
  print(id(b))  # 2034374541840
  change(b)
  print((id(b)))  # 2034374541840，修改方法中 num 的值后，方法外 b 的值没有改变
  
  print("----------------------")
  
  
  # 参数是可变类型
  def change(num):
      print(id(num))  # 2089520266368
      print(num)  # [1, 2, 3]
      num.append(4)
      print(id(num))  # 2089520266368
      print(num)  # [1, 2, 3, 4]
  
  
  b = [1, 2, 3]
  print(id(b))  # 2089520266368
  change(b)
  print((id(b)))  # 2089520266368
  print(b)  # [1, 2, 3, 4]
  ```

## 函数的返回值

```python
def 函数名():
    函数体
    return 返回值
```

## 函数的使用描述

```python
# 在函数体上面，添加三个双引号对注释
def 函数名():
    """
    函数的说明信息
    """
    函数体
```

通过`help(函数名)`，可以查看函数的使用描述。一般情况下，函数的使用描述，需要包含以下信息：

- 函数的功能。
- 函数的参数含义、类型、默认值等。
- 函数的返回值含义、类型等。

## 偏函数

**`偏函数`**：当编写一个参数比较多的函数时，如果有些参数，大部分场景下都是某一个固定值，那么为了简化使用，就可以创建一个新函数，指定需要使用的函数的某个参数为固定的值，这个新函数就叫偏函数。

定义：

```python
def aaa(a, b, c, d):
    print(a + b + c + d)


# 场景一：在该场景下，d 的值都是 1
# 定义偏函数 aaa1，默认 d 的值为 1，然后函数体中调用 aaa 函数
def aaa1(a, b, c, d=1):
    aaa(a, b, c, d)


# aaa1(1, 2, 3)  # 调用结果，控制台打印 7


# 场景二：在该场景下，d 的值都是 2
# 定义偏函数 aaa2，默认 d 的值为 2，然后函数体中调用 aaa 函数
def aaa2(a, b, c, d=2):
    aaa(a, b, c, d)


# aaa2(1, 2, 3)  # 调用结果，控制台打印 8

# Python 中，提供了 functools 模块的 partial 函数，能够快速的创建偏函数
import functools

# 参数 1 是需要创建偏函数的原函数，参数 2 是原函数中需要 "偏爱" 的参数名及其默认值
partial_function = functools.partial(aaa, d=3)
print(partial_function, type(
    partial_function))  # functools.partial(<function aaa at 0x000001FF9ABA3D90>, d=3) <class 'functools.partial'>

# 调用定义的偏函数，在该场景下，d 的值都是 3
partial_function(1, 2, 3)  # 调用结果，控制台打印 9
```

应用场景：

```python
s = "11011"

# 使用 int()，可以将一个数值的字符串，转为 int 值，默认是以十进制转换
print(int(s))  # 11011

# 如果需要以二进制转换，则需要指定 base 参数
print(int(s, base=2))  # 27

# 假设有一个场景，都需要以二进制转换，则可以创建一个偏函数
import functools

partial_function = functools.partial(int, base=2)
print(partial_function(s))  # 27
```

## 高阶函数

**`高阶函数`**：当一个函数 A 的参数，接收的是另一个函数时，则把这个函数 A 称之为高阶函数。

应用场景一：

```python
l = [{"name": "zhangsan", "age": 18}, {"name": "lisi", "age": 16}, {"name": "wangwu", "age": 20}, {"name": "maer", "age": 22}]


def get_key_name(p):
    return p["name"]


def get_key_age(p):
    return p["age"]


# sorted() 函数就是一个高阶函数，key 参数接收的是另一个函数

# 按照 name 排序，默认升序
result = sorted(l, key=get_key_name)
print(result)  # [{'name': 'lisi', 'age': 16}, {'name': 'maer', 'age': 22}, {'name': 'wangwu', 'age': 20}, {'name': 'zhangsan', 'age': 18}]

# 按照 age 排序，默认升序
result = sorted(l, key=get_key_age)
print(result)  # [{'name': 'lisi', 'age': 16}, {'name': 'zhangsan', 'age': 18}, {'name': 'wangwu', 'age': 20}, {'name': 'maer', 'age': 22}]
```

应用场景二：

```python
# 定义一个高阶函数，动态计算两个数据
def calculate(num1, num2, calculate_function):
    print(calculate_function(num1, num2))


# 加法运算
def addition_function(a, b):
    return a + b


# 减法运算
def subtraction_function(a, b):
    return a - b


# 乘法运算
def multiplication_function(a, b):
    return a * b


# 除法运算
def division_function(a, b):
    return a / b


# 调用
a = 10
b = 5
calculate(a, b, addition_function)  # 15
calculate(a, b, subtraction_function)  # 5
calculate(a, b, multiplication_function)  # 50
calculate(a, b, division_function)  # 2.0
```

## 返回函数

**`返回函数`**：是指一个函数内部，它返回的结果是另一个函数。

应用场景：

```python
# 根据不同的参数，获取不同的操作
def get_function(flag):
    # 加法运算
    def addition_function(a, b):
        return a + b

    # 减法运算
    def subtraction_function(a, b):
        return a - b

    # 乘法运算
    def multiplication_function(a, b):
        return a * b

    # 除法运算
    def division_function(a, b):
        return a / b

    # 根据 flag 的值，返回不同的操作函数
    if flag == "+":
        return addition_function
    elif flag == "-":
        return subtraction_function
    elif flag == "*":
        return multiplication_function
    elif flag == "/":
        return division_function


result_function = get_function("+")
print(result_function(1, 2))  # 3

result_function = get_function("*")
print(result_function(1, 2))  # 2
```

## 匿名函数

定义：

```python
lambad 参数1, 参数2, ...: 表达式
```

- 只能写一个表达式，不能直接 retrun。
- 表达式的结果就是函数的返回值。
- 只适用于一些简单的操作处理。

应用场景一：

```python
# 方式一
# 首先，定义一个匿名函数
fun = lambda x, y: x + y
# 然后，调用匿名函数
result = fun(1, 2)
print(result)  # 3

# 方式二
# 定义和调用匿名函数
result = (lambda x, y: x + y)(1, 2)
print(result)  # 3
```

应用场景二：

```python
# 简化高阶函数章节的示例，直接使用匿名函数，替换 get_key_name() 和 get_key_key()

l = [{"name": "zhangsan", "age": 18}, {"name": "lisi", "age": 16}, {"name": "wangwu", "age": 20}, {"name": "maer", "age": 22}]

# sorted() 函数就是一个高阶函数，key 参数接收的是另一个函数

# 按照 name 排序，默认升序
result = sorted(l, key=lambda p: p["name"])
print(result)  # [{'name': 'lisi', 'age': 16}, {'name': 'maer', 'age': 22}, {'name': 'wangwu', 'age': 20}, {'name': 'zhangsan', 'age': 18}]

# 按照 age 排序，默认升序
result = sorted(l, key=lambda p: p["age"])
print(result)  # [{'name': 'lisi', 'age': 16}, {'name': 'zhangsan', 'age': 18}, {'name': 'wangwu', 'age': 20}, {'name': 'maer', 'age': 22}]
```

## 闭包

**`闭包`**：在函数嵌套的前提下，内层函数引用了外层函数中的变量（包括外层函数的入参），外层函数又把内层函数当作返回值进行返回，这个内层函数与其所引用的外层函数中的变量（包括外层函数的入参），称为闭包。

示例：

```python
# 内部函数引用外部函数中定义的变量
def outer():
    num = 10

    def inner():
        return num

    return inner


func = outer()
print(func(), type(func))  # 10 <class 'function'>


# # 内部函数引用外部函数的入参
def outer(num):
    def inner():
        return num

    return inner


func = outer(10)
print(func(), type(func))  # 10 <class 'function'>
```

应用场景：外层函数根据不同的参数，生成不同功能的函数。

```python
# 闭包应用场景：根据配置信息，生成不同的分割线函数
def line_config(content, length):
    def line():
        print("-" * (length // 2) + content + "-" * (length // 2))

    return line


# 调用
line1 = line_config("闭包", 20)
line1()  # ----------闭包----------
line1()  # ----------闭包----------

line2 = line_config("abc", 30)
line2()  # ---------------abc---------------
line2()  # ---------------abc---------------

line3 = line_config("123", 40)
line3()  # --------------------123--------------------
line3()  # --------------------123--------------------
```

注意事项：

- 闭包中，如果需要修改引用的外层函数变量，需要使用`nonlocal 变量`声明，否则当作是闭包中，新定义的变量。

  ```python
  def aaa():
      num = 10
  
      def bbb():
          num = 666
          print(num)
  
      # 说明闭包内修改 num 值，不会影响外层函数中的变量
      print(num)  # 10
      bbb()  # 666
      print(num)  # 10
  
      return bbb
  
  
  aaa()
  
  
  def aaa():
      num = 10
  
      def bbb():
          # 声明闭包中的 num，就是外部函数中的变量
          nonlocal num
          num = 666
          print(num)
  
      # 说明闭包内修改 num 值，不会影响外层函数中的变量
      print(num)  # 10
      bbb()  # 666
      print(num)  # 666
  
      return bbb
  
  
  aaa()
  ```

- 当闭包内，引用了一个后期会发生变化的变量时，一定要注意

  ```python
  # 当函数被调用的时候，才会真正确定内部变量标识对应的值，在此之前，都是作为一个普通的变量标识存在
  
  # 在函数内部打印一个变量 a，但是 a 未定义
  def aaa():
      print(a)
  
  
  print("a")  # 此时，程序并不会报错，因为没有调用 aaa()
  
  
  # aaa()  # 此时，程序会报错，NameError: name 'a' is not defined
  
  # 根据上面分析的思路，当闭包内，引用了一个后期会发生变化的变量时，一定要注意变量的定义顺序
  def bbb():
      num = 10
  
      def ccc():
          print(num)
  
      num = 20
      return ccc
  
  
  func = bbb()
  func()  # 最终结果，打印的 num 是 20，不是 10
  
  
  # 示例二
  def ddd():
      funcs = []
      for i in range(1, 4):
          def eee():
              print(i)
  
          funcs.append(eee)
      return funcs
  
  
  new_func = ddd()
  print(
      new_func)  # [<function ddd.<locals>.eee at 0x000002062A833C70>, <function ddd.<locals>.eee at 0x000002062A833D00>, <function ddd.<locals>.eee at 0x000002062A833D90>]
  # 调用，打印的都是 3，虽然在循环中定义新的函数，但最终调用的时候，i 值是循环的最后一个赋值，也就是 3
  new_func[0]()  # 3
  new_func[1]()  # 3
  new_func[2]()  # 3
  
  
  # 如果想打印 1，2，3，做如下改进
  def ddd():
      funcs = []
      for i in range(1, 4):
          def eee(num):
              def fff():
                  print(num)
  
              return fff
  
          funcs.append(eee(i))
      return funcs
  
  
  new_func = ddd()
  print(
      new_func)  # [<function ddd.<locals>.eee.<locals>.fff at 0x0000025C3C973EB0>, <function ddd.<locals>.eee.<locals>.fff at 0x0000025C3C973BE0>, <function ddd.<locals>.eee.<locals>.fff at 0x0000025C3C973F40>]
  new_func[0]()  # 1
  new_func[1]()  # 2
  new_func[2]()  # 3
  ```

## 装饰器

**`装饰器`**：在函数名以及函数体不改变的情况下，给一个函数附加一些额外的代码。

定义：

```python
# 定义装饰器
def check_login(func):
    print("装饰器的执行时间，是立即执行")  # 当代码运行时，即使没有调用函数，只要添加了装饰器的语法糖，就会打印这段话

    def inner():
        print("登陆验证")
        func()

    return inner


# 定义功能函数，并使用语法糖添加装饰器，语法糖的效果，等同于：fss = check_login(fss)
@check_login
def fss():
    print("发说说")

# 调用功能函数
# fss()
```

进阶：

- **多个装饰器叠加时，顺序是：从上到下装饰，从下到上执行。**

  ```python
  # 装饰器一
  def wrapper_line(func):
      def inner():
          print("-" * 30)
          func()
  
      return inner
  
  
  # 装饰器二
  def wrapper_start(func):
      def inner():
          print("*" * 30)
          func()
  
      return inner
  
  
  # 定义功能函数，添加两个装饰器，顺序是：从上到下装饰，从下到上执行。
  @wrapper_line
  @wrapper_start
  def aaa():
      print("aaa")
  
  
  # 调用功能函数
  aaa()
  ```

  ```python
  ------------------------------
  ******************************
  aaa
  ```

- **对有参数的函数进行装饰：使用元组和字典，接收被装饰函数的参数。**

  ```python
  # 装饰器
  def wrapper_line(func):
      # 不同的被装饰函数，入参可能不相同，使用元组和字典，接收被装饰函数的参数（会自动匹配哪些参数为元组，哪些参数为字典，拆包和装包）
      def inner(*args, **kwargs):
          print("-" * 30)
          print(args, kwargs)
          func(*args, **kwargs)
  
      return inner
  
  
  # 功能函数一，单个参数
  @wrapper_line
  def aaa(num):
      print(num)
  
  
  # 功能函数二，多个参数
  @wrapper_line
  def bbb(num1, num2, num3):
      print(num1, num2, num3)
  
  
  # 调用功能函数
  aaa(123)
  bbb(111, 222, num3="abc")
  ```

- **对有返回值的函数进行装饰：在闭包中，将被装饰函数调用后的返回值也返回。**

  ```python
  # 装饰器
  def wrapper_line(func):
      # 不同的被装饰函数，入参可能不相同，使用元组和字典，接收被装饰函数的参数（会自动匹配哪些参数为元组，哪些参数为字典，拆包和装包）
      def inner(*args, **kwargs):
          print("-" * 30)
          # print(args, kwargs)
          return func(*args, **kwargs)
  
      return inner
  
  
  # 功能函数一，无返回值
  @wrapper_line
  def aaa(num):
      print(num)
  
  
  # 功能函数二，有返回值
  @wrapper_line
  def bbb(num1, num2, num3):
      print(num1, num2, num3)
      return num1 + num2 + num3
  
  
  # 调用功能函数
  result1 = aaa(123)
  result2 = bbb(111, 222, num3=666)
  
  # 打印功能函数调用的结果
  print("*" * 30)
  print(result1, result2)  # None 999
  ```

- **带有参数的装饰器：把 @ 符号后面的内容，整体作为装饰器，用来装饰被装饰的函数。**

  ```python
  # 外层：获取内部装饰器的装饰器，带有一个参数，根据不同的参数，获取不同的装饰器
  def get_wrapper(char):
      # 内层：根据外层装饰器入参，打印不同分割线的装饰器
      def wrapper_char(func):
          # 不同的被装饰函数，入参可能不相同，使用元组和字典，接收被装饰函数的参数（会自动匹配）
          def inner(*args, **kwargs):
              print(char * 30)
              # print(args, kwargs)
              return func(*args, **kwargs)
  
          return inner
  
      return wrapper_char
  
  
  # 功能函数一，打印 - 分割线
  @get_wrapper("-")
  def aaa(num):
      print(num)
  
  
  # 功能函数二，打印 * 分割线
  @get_wrapper("*")
  def bbb(num):
      print(num)
  
  
  # 调用
  aaa(666)
  bbb(666)
  ```

## 生成器

**`生成器`**：是一个特殊的迭代器，但是其抽象程度更高（反过来，迭代器不一定是生成器）。其拥有迭代器的特性：惰性计算数据，节省内存；能够记录状态，并通过 next() 函数，访问下一个状态；具备可迭代性。但是，如果想打造一个自己的迭代器，需要实现很多方法，比较复杂，而生成器则可以很方便的实现。

定义：

- **生成器表达式：把列表推导式的 [] 修改成 ()。**

  ```python
  # 列表推导式
  l = [i for i in range(1, 100) if i % 2 == 0]
  print(
      l)  # [2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48, 50, 52, 54, 56, 58, 60, 62, 64, 66, 68, 70, 72, 74, 76, 78, 80, 82, 84, 86, 88, 90, 92, 94, 96, 98]
  # 弊端：如果是一个很大的列表，无法做到数据惰性生成，占用大量内存
  
  # 生成器表达式
  l = (i for i in range(1, 100) if i % 2 == 0)
  print(l, type(l))  # <generator object <genexpr> at 0x0000023D2A8B5BD0> <class 'generator'>
  
  # 生成器的调用---遍历，同迭代器（遍历的方式不会报错）
  for i in l:
      print(i)
  ```

- **生成器函数：函数中包含 yield 语句，这个函数的执行结果就是生成器。**

  ```python
  # 生成器函数，包含 yield 语句，yield 语句，就是一个一个的状态
  # yield 语句可以阻断当前的函数执行，当使用 next() 或者 __next__() 时，都可以让函数继续执行，
  # 当执行遇到下一个 yield 语句时，又会被阻断
  def generator_function():
      yield 1
      print("a")
      yield 2
      print("b")
      yield 3
      print("c")
      yield 4
      print("d")
  
  
  # 生成一个生成器
  func = generator_function()
  print(func, type(func))
  
  
  # 调用生成器
  print(next(func))  # 在 yield 1 之前，没有输出，控制台输出 1，然后停止
  print(next(func))  # 在 yield 1 和 yield 2 之间，先输出 a，再输出 2，然后停止
  print(next(func))  # 在 yield 2 和 yield 3 之间，先输出 b，再输出 3，然后停止
  print(next(func))  # 在 yield 3 和 yield 4 之间，先输出 c，再输出 4，然后停止
  print(next(func))  # 在 yield 4 之后，先输出 d，后续没有 yield 语句，也就没有状态了，会报一个错误 StopIteration
  
  
  # 简化版
  def generator_function():
      for i in range(1, 9):
          print("num: ", i ** 2)
          yield i
  
  
  func = generator_function()
  print(func.__next__())
  print(func.__next__())
  ```

方法：

- `send() 方法`：**send() 方法有一个参数，指定的是上一次被挂起的 yield 语句的返回值，相比于 next()，可以额外的给 yield 语句传值。**

  ```python
  # yield 语句，正常是无返回值的
  def generator_function():
      yield_1 = yield 1
      print(yield_1)
      yield_2 = yield 2
      print(yield_2)
      yield_3 = yield 3
      print(yield_3)
      yield_4 = yield 4
      print(yield_4)
  
  
  # 生成一个生成器
  func = generator_function()
  print(func, type(func))
  
  # 使用 next() 方法或者 __next()__ 方法调用生成器，yield 语句输出都是 None
  # print(next(func))  # 在 yield 1 之前，没有输出，控制台输出 1，然后停止
  # print(next(func))  # 在 yield 1 和 yield 2 之间，先输出 None，再输出 2，然后停止
  # print(next(func))  # 在 yield 2 和 yield 3 之间，先输出 None，再输出 3，然后停止
  # print(next(func))  # 在 yield 3 和 yield 4 之间，先输出 None，再输出 4，然后停止
  # print(next(func))  # 在 yield 4 之后，先输出 None，后续没有 yield 语句，也就没有状态了，会报一个错误 StopIteration
  
  # 使用 send() 方法调用生成器，给 send() 方法传参，可以成为 yield 语句的输出内容
  # 注意：第一次调用 send() 方法时，需要传递 None，否则报错：TypeError: can't send non-None value to a just-started generator
  # 因为 send() 方法的入参是上一个 yield 语句的输出，第一次调用，阻断时前面没有 yield 语句
  # print(func.send(None))  # 在 yield 1 之前，没有输出，控制台输出 1，然后停止
  # print(func.send("ooo"))  # 在 yield 1 和 yield 2 之间，先输出 ooo，再输出 2，然后停止
  # print(func.send("ooo"))  # 在 yield 2 和 yield 3 之间，先输出 ooo，再输出 3，然后停止
  
  # 或者先调用 next()/__next()__ 方法，再调用 send() 方法
  print(func.__next__())  # 在 yield 1 之前，没有输出，控制台输出 1，然后停止
  print(func.send("ooo"))  # 在 yield 1 和 yield 2 之间，先输出 ooo，再输出 2，然后停止
  print(func.send("ooo"))  # 在 yield 2 和 yield 3 之间，先输出 ooo，再输出 3，然后停止
  ```

- `close() 方法`：关闭生成器。如果不想继续使用生成器了，可以使用 close() 方法关闭生成器，生成器关闭之后，无法再次被调用，否则会报错。

  ```python
  def generator_function():
      yield 1
      print("a")
      yield 2
      print("b")
      yield 3
      print("c")
      yield 4
      print("d")
  
  
  # 生成一个生成器
  func = generator_function()
  print(func, type(func))
  
  # 调用生成器
  print(next(func))
  
  # 关闭生成器
  func.close()
  
  # 生成器关闭后，如果再次调用，会报错
  print(next(func))  # 报错：StopIteration
  ```

注意事项：

- 生成器只能遍历一次，如果需要多次遍历，则需要再次生成。
- 生成器执行时，如果碰到 return 语句，直接终止，抛出 StopIteration 异常。

## 递归函数

**`递归函数`**：函数 A 内部，继续调用函数 A。

示例：

```python
# 求一个数字的阶乘
def func(num):
    if num == 1:
        return 1
    return num * func(num - 1)


print(func(9))  # 362880
```

## 函数的作用域

**`变量的作用域`**：变量的作用范围，也即变量的可操作范围。Python 是静态作用域，在 Python 中，变量的作用域源于它在代码中定义的位置，在不同的位置，可能有不同的命名空间。

**`命名空间`**：是指作用域的体现形式。Python 中有多种命名空间，可以用 Python-LEGB 表示：

- L：Local，函数内的命名空间。作用范围：当前整个函数体的范围。

- E：Enclosing function locals，外部嵌套函数的命令空间。作用范围：闭包函数。

- G：Global，全局命名空间。作用范围：当前模块（文件）。

- B：Builtin，内建模块命名空间。作用范围：所有模块（文件）。

- LEGB 规则：按照 L ---> E ---> G ---> B 的顺序查找变量。

- 注意：Python 中没有块级作用域，块级作用域是指代码块中，if、while、for 后面的代码块。、

  ```python
  if True:
      a = 10
  # if 块中定义的变量，在 if 块外面也可以使用
  print(a)
  ```

基于命名空间的常见变量类型：

- `局部变量`：在一个函数内部定义的变量，作用域为函数内部。使用 locals() 方法，可以查看所有的局部变量。

- `全局变量`：在函数外部，文件最外层定义的变量，作用域为整个文件内部。使用 globals() 方法，可以查看所有的局部变量。

- 局部变量和全局变量的查看：

  ```python
  # 全局变量
  num = 999
  
  
  def aaa():
      # 全局变量
      global num
      num = 10
      # 局部变量
      a = 1
      b = 2
      print(locals())
  
  
  aaa()  # {'a': 1, 'b': 2}
  print(globals())  # {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000029143EA48B0>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'D:\\Codes\\dify-0.15.3\\api\\test.py', '__cached__': None, 'num': 10, 'aaa': <function aaa at 0x0000029143DE3D90>}
  ```

- 局部变量和全局变量之间的切换：

  ```python
  # 全局变量
  num = 999
  
  
  def aaa():
      # 局部变量
      num = 10
      print(num)
  
  
  print(num)  # 999
  aaa()  # 10
  print(num)  # 999
  
  # 全局变量
  num = 999
  
  
  def aaa():
      # 使用 global 将 num 变为全局变量
      global num
      num = 10
      print(num)
  
  
  print(num)  # 999
  aaa()  # 10
  print(num)  # 10
  ```

- 注意事项：

  - 访问原则：从内到外。
  - 变量定义时，尽量将全局变量放在最上面，命名前加 g_ 用于区分。
  - 如果出现全局变量和局部变量重名的情况，按就近原则获取，如果在函数中使用全局变量，需添加 global 关键字声明。

## 本文参考

https://www.bilibili.com/video/BV12b411g7G7
