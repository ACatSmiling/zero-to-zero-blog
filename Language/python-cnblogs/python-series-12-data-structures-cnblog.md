> `Author: ACatSmiling`
>
> `Since: 2025-01-20`

## 数值

**表现形式**：

- `整数（int）`
  - 二进制：0b，只能包含 0 和 1。
  - 八进制：0o，只能包含 0 ~ 7。
  - 十进制：只能包含 0 ~ 9。
  - 十六进制：0x，只能包含 0 ~ 9，以及 a ~ f。
- `浮点数（float）`
  - 由整数部分和小数部分组成，可以使用科学计数法表示，例如 1.68e2。
- `复数（complex）`
  - 由实部和虚部组成，表示为 a + bj，其中 a 和 b 都是浮点数，也可以使用 complex(a, b) 函数表示。

**运算符**：

- 算术运算符：`+ - * / ** // % =`。
- 复合运算符：`+= -+ *= /= **= //=`。
- 比较运算符：`> < != <> >= == is`。
- 逻辑运算符：`not and or`。
- 注意：
  - 当 int 和 float 类型进行算数运算时，结果会被提升为 float 类型。
  - 非布尔类型的值，如果作为真假来判断，一般都是非空即真；数值类型如果参与逻辑运算，参照的原则是非零即真。

**函数**：

- 数学函数

  - 内建函数：`abs(num)，max(num1, num2, ...)，min(num1, num2, ...)，round(num[, n])，pow(x, y)。`示例如下：

    ```pyth
    # abs(num)
    #   返回数值的绝对值
    num = -10
    print(abs(num))  # 10
    
    # 最大值
    print(max(1, 12, 3, 5))  # 12，多数值参数
    print(max([1, 12, 3, 5]))  # 12，单列表参数
    
    # 最小值
    print(min(1, 12, 3, 5))  # 1，多数值参数
    print(min([1, 12, 3, 5]))  # 1，单列表参数
    
    # round(num[, n])
    #   四舍五入，n 表示四舍五入的位数，可以省略
    p = 3.14
    print(round(p))  # 3，省略 n，直接取整
    print(round(p, 1))  # 3.1，保留一位小数
    
    # pow(x, y)
    #   返回 x 的 y 次幂，等同于 x ** y
    print(pow(2, 3))  # 8
    print(2 ** 3)  # 8
    ```

  - math 模块函数：`ceil(num)，floor(num)，sqrt(num)，log(x, base)。`示例如下：

    ```python
    import math
    
    # 语法：math.函数名(参数)
    
    # ceil(num)
    #   向上取整
    print(math.ceil(5.15))  # 6
    print(math.ceil(5.75))  # 6
    
    # floor(num)
    #   向下取整
    print(math.floor(5.15))  # 5
    print(math.floor(5.75))  # 5
    
    # sqrt(num)
    #   开平方
    print(math.sqrt(16))  # 4.0
    
    # log(x, base)
    #   以 base 为基数，求 x 的对数
    print(math.log(10000, 10))  # 4.0
    print(math.log(10000, 100))  # 2.0
    ```

- 随机函数：导入 random 模块。

  - `random()`：返回一个区间 [0, 1) 范围内的随机小数。

  - `choice(seq)`：返回一个序列中的随机数值。

  - `uniform(x, y)`：返回一个区间 [x, y] 范围内的随机小数。

  - `randint(x, y)`：返回一个区间 [x, y] 范围内的随机整数。

  - `randrange(start, stop=None, step=1)`：返回一个区间 [start, stop) 范围内的随机整数，step 是以 start 为起点的步长，默认为 1。

  - 示例：

    ```python
    import random
    
    # random()
    #   返回一个区间 [0, 1) 范围内的随机小数
    print(random.random())  # 0.177762474320725
    
    # choice(seq)
    #   返回一个序列中的随机数值
    seq = [1, 3, 5, 6, 8, 9]
    print(random.choice(seq))  # 9
    
    # uniform(x, y)
    #   返回一个区间 [x, y] 范围内的随机小数
    print(random.uniform(1, 10))  # 9.730491798458493
    
    # randint(x, y)
    #   返回一个区间 [x, y] 范围内的随机整数
    print(random.randint(1, 10))  # 3
    
    # randrange(start, stop=None, step=1)
    #   返回一个区间 [start, stop) 范围内的随机整数，step 是以 start 为起点的步长，默认为 1
    print(random.randrange(1, 10))  # 2
    print(random.randrange(1, 10, 2))  # 7，步长为 2，只会出现奇数随机数
    ```

- 三角函数：导入 math 模块。

  - `math.pi`：数学常量 π。

  - `sin(x) 和 cos(x)`：正弦函数和余弦函数。

  - `tan(x)`：正切函数。

  - `asin(x) 和 acos(x)`：反正弦函数和反余弦函数。

  - `atan(x)`：反正切函数。

  - `degress(x)`：弧度转角度。

  - `radians(x)`：角度转弧度。

  - 示例：

    ```python
    import math
    
    # 数学常量 π
    PI = math.pi
    print(PI)  # 3.141592653589793
    
    # sin(x) 正弦函数
    print(math.sin(PI / 6))  # 0.49999999999999994，即 0.5，存在精度问题
    
    # cos(x) 余弦函数
    print(math.cos(PI / 3))  # 0.5000000000000001
    
    # degress(x) 弧度转角度
    print(math.degrees(PI / 2))  # 90.0
    
    # radians(x) 角度转弧度
    print(math.radians(90))  # 1.5707963267948966，即 π/2
    ```

## 布尔值

布尔值有两个：`True`和`False`。**布尔值是 int 类型的子类**。

- 布尔值可以当作 int 类型，参与相关运算。
- 布尔值可以作为比较表达式的运算结果。
- 布尔值可以用于 if 判定条件和 while 循环条件。

示例：

```python
print(issubclass(bool, int))  # True，bool 是 int 的子类
print(True + 2)  # 3
print(False + 2)  # 2
```

## 字符串

**`字符串`**：由单个字符组成的一个集合。

**表现形式**：

- `非原始字符串`：对于一些特殊含义的字符，需要添加转义字符。

  - 使用单引号包含，例如 'abc'。

  - 使用双引号包含，例如 "abc"。

  - 使用三个单引号包含，例如 '''abc'''。

  - 使用三个双引号包含，例如 """abc"""。

  - 示例：

    ```python
    # 使用单引号包含
    str1 = 'abc'
    print(str1, type(str1))  # abc <class 'str'>
    
    # 使用双引号包含
    str1 = "abc"
    print(str1, type(str1))  # abc <class 'str'>
    
    # 使用三个单引号包含
    str1 = '''abc'''
    print(str1, type(str1))  # abc <class 'str'>
    
    # 使用三个双引号包含
    str1 = """abc"""
    print(str1, type(str1))  # abc <class 'str'>
    
    # 续行符 \，位于行尾
    print('aaa' \
          'bbb')  # aaabbb
    
    # 单引号 \'
    print('a\'aa')  # a'aa
    
    # 双引号 \"
    print('a\"aa')  # a"aa
    
    # 换行符 \n
    print('a\naa')  # a 和 aa 换行
    
    # 横向制表符 \t
    print('a\taa')  # a	aa
    ```

- `原始字符串`：在非原始字符串前面加上 r，原始字符串内的字符，都会原样输出。示例：

  ```python
  # 使用单引号包含
  str1 = r'abc'
  print(str1, type(str1))  # abc <class 'str'>
  
  # 使用双引号包含
  str1 = r"abc"
  print(str1, type(str1))  # abc <class 'str'>
  
  # 使用三个单引号包含
  str1 = r'''abc'''
  print(str1, type(str1))  # abc <class 'str'>
  
  # 使用三个双引号包含
  str1 = r"""abc"""
  print(str1, type(str1))  # abc <class 'str'>
  
  # 加了 r 之后，原先的转义符失去了作用，成为普通的字符
  print(r'a\'aa')  # a\'aa
  
  # 双引号 \"
  print(r'a\"aa')  # a\"aa
  
  # 换行符 \n
  print(r'a\naa')  # a\naa
  
  # 横向制表符 \t
  print(r'a\taa')  # a\taa
  ```

- 特点：

  - 单双引号：混合使用可以避免使用引号转义符。

    ```python
    print('abc"d"')  # abc"d"
    print("abc'd'")  # abc'd'
    ```

  - 三引号：可以直接跨行书写，也可以用于注释。

**字符串一般操作**：

- `字符串拼接`

  - 方式一：使用 + 运算符，str1 + str2。

  - 方式二：直接拼接，str1str2。

  - 方式三：使用占位符，"xxx%sxxx"。

  - 方式四：使用 * 运算符，str1 * 3。

  - 示例：

    ```python
    # + 运算符
    print("我是" + "小明")  # 我是小明
    # 直接拼接
    print("我是""小明")  # 我是小明
    # 占位符
    print("我是%s" % "小明")  # 我是小明
    print("我是%s，%d" % ("小明", 123))  # 我是小明，123
    # * 运算符
    print("我是小明" * 3)  # 我是小明我是小明我是小明
    ```

- `字符串切片`：获取一个字符串的某个片段。

  - **获取某一个字符：name[下标]。**下标是字符串中每个字符的编号，正向的下标从 0 开始，反向的下标从 -1 开始。注意不要下标越界。

    ```python
    str = "abcdefg"
    
    # 正向的下标从 0 开始
    print(str[0])  # a
    print(str[6])  # g
    
    # 反向的下标从 -1 开始
    print(str[-1])  # g
    print(str[-7])  # a
    
    # 下标越界
    print(str[7])  # IndexError: string index out of range
    print(str[-8])  # IndexError: string index out of range
    ```

  - **获取某一个片段：name[起始索引:结束索引:步长]。**注意，截取的片段，包含起始索引位置的字符，但不包含结束索引位置的字符。默认情况下，起始索引为 0，结束索引为字符串的长度，步长为 1。如果步长大于 0，则从左往右正向截取，如果步长小于 0，则从右往左反向截取。当截取方向确定时，步长的正负取值需要与截取方向保持一致。

    ```python
    str = "abcdefg"
    
    print(str[::])  # abcdefg
    
    print(str[0:len(str):1])  # abcdefg
    
    print(str[0:3])  # abc
    
    print(str[::2])  # aceg
    
    print(str[::-2])  # geca
    
    print(str[1:6:1])  # bcdef，步长取值合法
    print(str[1:6:-1])  # 步长取值不合法，输出空字符串
    print(str[-2:-7:-1])  # fedcb，步长取值合法
    print(str[-2:-7:1])  # 步长取值不合法，输出空字符串
    
    a = str[1:6:-1]
    print(a == "", type(a))  # True <class 'str'>
    ```

- `字符串反转`

  ```python
  str = "abcdefg"
  
  print(str[::-1])  # gfedcba
  ```

**字符串函数操作**：

- 查找计算类：

  - `len`：计算字符串中字符的个数。

  - `find`：查找子串的索引位置，从左往右开始查找，找到后立即返回，返回值是被查找子串首字符的下标。

  - `rfind`：功能同 find，区别是从右往左开始查找。

  - `index`：获取子串的索引位置，从左到右开始获取，找到后立即返回下标，找不到报错。

  - `rindex`：功能同 index，区别是从右往左开始查找。

  - `count`：计算子串出现的个数。

  - 示例：

    ```python
    str = "abcdefgde"
    
    # len 计算字符串中字符的个数
    print(len(str))  # 9
    print(len("我是一个中国人"))  # 7，不管是中文，还是英文，都是一个字符
    
    # find 查找子串的索引位置，从左往右开始查找，找到后立即返回，返回值是被查找子串首字符的下标
    print(str.find("de"))  # 3
    print(str.find("de", 4, 8))  # -1，未发现
    print(str.find("de", 4, 9))  # 7，查找范围包含 start，但不包含 end
    
    # rfind 功能同 find，区别是从右往左开始查找
    print(str.rfind("de"))  # 7
    print(str.rfind("de", 4, 8))  # -1
    print(str.rfind("de", 4, 9))  # 7
    
    # index 获取子串的索引位置，从左到右开始获取，找到后立即返回下标，找不到报错
    print(str.index("de"))  # 3
    print(str.index("de", 4, 9))  # 7
    # print(str.index("de", 4, 8))  # 报错，ValueError: substring not found
    
    # rindex 功能同 index，区别是从右往左开始查找
    print(str.rindex("de"))  # 7
    print(str.rindex("de", 4, 9))  # 7
    # print(str.rindex("de", 4, 8))  # 报错
    
    # count 计算子串出现的个数
    print(str.count("de"))  # 2
    print(str.count("de", 4, 8))  # 0
    print(str.count("de", 4, 9))  # 1，查找范围包含 start，但不包含 end
    ```

- 转换类：

  - `replace`：使用给定的新字符串，替换原字符串中的旧字符串，该操作不影响原字符串。

  - `capitalize`：将字符串的首字母大写，该操作不影响原字符串。

  - `title`：将字符串中每个单词的首字母大写，该操作不影响原字符串。

  - `lower`：将字符串中每个字符都变为小写，该操作不影响原字符串。

  - `upper`：将字符串中每个字符都变为大写，该操作不影响原字符串。

  - 示例：

    ```python
    str = "abcdefgde"
    
    # replace 使用给定的新字符串，替换原字符串中的旧字符串，该操作不影响原字符串
    print(str.replace("de", "DE"))  # abcDEfgDE，全部替换
    print(str.replace("de", "DE", 1))  # abcDEfgde，只替换第一个
    
    # capitalize 将字符串的首字母大写，该操作不影响原字符串
    print(str.capitalize())  # Abcdefgde
    
    # title 将字符串中每个单词的首字母大写，该操作不影响原字符串
    print("today is sunday".title())  # Today Is Sunday
    print("today-is-sunday, tomorrow%is%monday".title())  # Today-Is-Sunday, Tomorrow%Is%Monday，字符串中间非英文字母分割，即视为多个单词
    
    # lower 将字符串中每个字符都变为小写，该操作不影响原字符串
    print("ABCDEFG".lower())  # abcdefg
    
    # upper 将字符串中每个字符都变为大写，该操作不影响原字符串
    print("abcdefg".upper())  # ABCDEFG
    ```

- 填充压缩类：

  - `ljust`：使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串结尾右边，该操作不影响原字符串。

  - `rjust`：使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串结尾左边，该操作不影响原字符串。

  - `center`：使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串左右两边，原字符串居中，该操作不影响原字符串。

  - `lstrip`：移除原字符串中所有指定的字符，默认为空白字符（空格，\n，\t 等），要求左边第一个就是指定的字符串，该操作不影响原字符串。

  - `rstrip`：移除原字符串中所有指定的字符，默认为空白字符（空格，\n，\t 等），要求右边第一个就是指定的字符串，该操作不影响原字符串。

  - 示例：

    ```python
    str = "abcdefgde"
    
    # ljust 使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串结尾右边，该操作不影响原字符串
    print(str.ljust(5, "X"))  # abcdefgde，原字符串长度大于 5，当前不做填充操作
    print(str.ljust(12, "X"))  # abcdefgdeXXX
    # print(str.ljust(12, "XXX"))  # 报错，TypeError: The fill character must be exactly one character long
    
    # rjust 使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串结尾左边，该操作不影响原字符串
    print(str.rjust(5, "X"))  # abcdefgde，原字符串长度大于 5，当前不做填充操作
    print(str.rjust(12, "X"))  # XXXabcdefgde
    # print(str.rjust(12, "XX"))  # 报错，TypeError: The fill character must be exactly one character long
    
    # center 使用指定字符（长度仅限为 1），将原字符串填充到指定长度，被填充的新字符位于原字符串左右两边，原字符串居中，该操作不影响原字符串
    print(str.center(10, "X"))  # abcdefgdeX
    print(str.center(11, "X"))  # XabcdefgdeX
    print(str.center(12, "X"))  # XabcdefgdeXX
    print(str.center(13, "X"))  # XXabcdefgdeXX
    
    # lstrip 移除原字符串中所有指定的字符，默认为空白字符（空格，\n，\t 等），要求左边第一个就是指定的字符串，该操作不影响原字符串
    print("dabcbad".lstrip("d"))  # abcbad
    print("ddabcbad".lstrip("d"))  # abcbad
    print("addabcbad".lstrip("d"))  # addabcbad
    print("addabcbad".lstrip("ad"))  # bcbad，ad这两个字符都会被移除，与顺序无关
    
    # rstrip 移除原字符串中所有指定的字符，默认为空白字符（空格，\n，\t 等），要求右边第一个就是指定的字符串，该操作不影响原字符串
    print("dabcbad".rstrip("d"))  # dabcba
    print("dabcbadd".rstrip("d"))  # dabcba
    print("dabcbadda".rstrip("d"))  # dabcbadda
    print("dabcbadda".rstrip("ad"))  # dabcb，ad这两个字符都会被移除，与顺序无关
    ```

- 分割拼接类：

  - `split`：将字符串按照指定分隔符进行分割，返回一个列表，该操作不影响原字符串。

  - `partition`：将字符串按照指定分隔符进行分割，返回一个元组，该操作不影响原字符串。

  - `rpatition`：功能同 partition，但是从字符串右侧开始查找分隔符。

  - `splitlines`：按照换行符（\r，\n），将字符串拆分为多个元素，返回一个列表，该操作不影响原字符串。

  - `join`：根据给定的字符串，将给定的可迭代对象（字符串，列表，元组等，能通过循环遍历的），进行拼接，返回拼接后的新字符串。

  - 示例：

    ```python
    str = "abc-def-g"
    
    # split 将字符串按照指定分隔符进行分割，返回一个列表，该操作不影响原字符串
    print(str.split("-"))  # ['abc', 'def', 'g']
    print(str.split("-", 1))  # ['abc', 'def-g']，只分割一次，默认是全部分割
    
    # partition 将字符串按照指定分隔符进行分割，返回一个元组，该操作不影响原字符串
    print(str.partition("-"))  # ('abc', '-', 'def-g')，原字符串存在指定分隔符
    print(str.partition("+"))  # ('abc-def-g', '', '')，原字符串不存在指定分隔符
    
    # rpartition 功能同 partition，但是从字符串右侧开始查找分隔符
    print(str.rpartition("-"))  # ('abc-def', '-', 'g')
    print(str.rpartition("+"))  # ('', '', 'abc-def-g')
    
    # splitlines 按照换行符（\r，\n），将字符串拆分为多个元素，返回一个列表，该操作不影响原字符串
    str = "Monday\nTuesday\nWednesDay\nThursday\nFriday\nSaturday\nSunday"
    print(str.splitlines())  # ['Monday', 'Tuesday', 'WednesDay', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    print(str.splitlines(
        True))  # ['Monday\n', 'Tuesday\n', 'WednesDay\n', 'Thursday\n', 'Friday\n', 'Saturday\n', 'Sunday']，添加 True 参数后，会保留换行符
    
    # join 根据给定的字符串，将给定的可迭代对象（字符串，列表，元组等，能通过循环遍历的），进行拼接，返回拼接后的新字符串
    print(".".join("abcdefg"))  # a.b.c.d.e.f.g
    print(".".join(["a", "b", "c", "d", "e", "f", "g"]))  # a.b.c.d.e.f.g
    print(".".join(("a", "b", "c", "d", "e", "f", "g")))  # a.b.c.d.e.f.g
    ```

- 判定类：

  - `isalpha`：判定字符串中是否所有的字符都是字母，不包含数字、特殊符号、标点符号等。

  - `isdigit`：判定字符串中是否所有的字符都是数字，不包含字母、特殊符号、标点符号等。

  - `isalnum`：判定字符串中是否所有的字符都是字母或者数字，不包含特殊符号、标点符号等。

  - `isspace`：判定字符串中是否所有的字符都是空白符，包含空格、制表符、换行符等。

  - `startswith`：判定给定字符串是否以某个前缀开头。

  - `endswith`：判定给定字符串是否以某个后缀结尾。

  - `in`：判定一个字符串，是否被另一个字符串包含。

  - `not in`：判定一个字符串，是否不被另一个字符串包含。

  - 示例：

    ```python
    # isalpha 判定字符串中是否所有的字符都是字母，不包含数字、特殊符号、标点符号等
    print("abcdEFG".isalpha())  # True
    print("123456".isalpha())  # False
    print("abc-def-g".isalpha())  # False
    print("123,./-+".isalpha())  # False
    print("".isalpha())  # False
    
    # isdigit 判定字符串中是否所有的字符都是数字，不包含字母、特殊符号、标点符号等
    print("abcdEFG".isdigit())  # False
    print("123456".isdigit())  # True
    print("abc-def-g".isdigit())  # False
    print("123,./-+".isdigit())  # False
    print("".isdigit())  # False
    
    # isalnum 判定字符串中是否所有的字符都是字母或者数字，不包含特殊符号、标点符号等
    print("abcdEFG".isalnum())  # True
    print("123456".isalnum())  # True
    print("abc-def-g".isalnum())  # False
    print("123,./-+".isalnum())  # False
    print("".isalnum())  # False
    
    # isspace 判定字符串中是否所有的字符都是空白符，包含空格、制表符、换行符等
    print("abcdEFG".isspace())  # False
    print("123456".isspace())  # False
    print("abc-def-g".isspace())  # False
    print("123,./-+".isspace())  # False
    print("".isspace())  # False
    print(" \t \r \n".isspace())  # True
    
    # startswith 判定给定字符串是否以某个前缀开头
    print("abc".startswith("a"))  # True
    print("abc".startswith("ab"))  # True
    print("abc".startswith("abc"))  # True
    print("abc".startswith("abcd"))  # False
    
    # endswith 判定给定字符串是否以某个后缀结尾
    print("abc".endswith("c"))  # True
    print("abc".endswith("bc"))  # True
    print("abc".endswith("abc"))  # True
    print("abc".endswith("abcd"))  # False
    
    # in 判定一个字符串，是否被另一个字符串包含
    print("abc" in "abcdef")  # True
    
    # not in 判定一个字符串，是否不被另一个字符串包含
    print("abc" not in "abcdef")  # False
    ```

## 列表

**`列表`**：有序的可变的元素集合。

**列表的定义**：

- 方式一：`[]`。

  ```python
  # 使用 [] 定义列表，列表中的元素类型可以不一致，也可以不包含元素
  items = [1, 2, "a", "b", [3, 4], True]
  print(items, type(items))  # [1, 2, 'a', 'b', [3, 4], True] <class 'list'>
  ```

- 方式二：列表生成式，`range(start, stop, step)`，start 默认为 0，step 默认为 1。

  ```python
  # 列表生成式：range(start, stop, step)，start 默认为 0，step 默认为 1
  # 为防止生成的列表没有使用，占用内存，Python 3 做了优化，不会立即生成列表
  
  # 0~100，包含 0，不包含 100
  items = range(100)
  print(items, type(items))  # range(0, 100) <class 'range'>
  
  # 0~100 之间的偶数
  items = range(0, 100, 2)
  print(items[0], items[1], items[2], items[len(items) - 1], type(items))  # 0 2 4 98 <class 'range'>
  
  # 0~100 之间的奇数
  items = range(1, 100, 2)
  print(items[0], items[1], items[2], items[len(items) - 1], type(items))  # 1 3 5 99 <class 'range'>
  ```

- 方式三：列表推导式，即从一个列表推导出另一个列表。

  ```python
  nums = [1, 2, 3, 4, 5]
  
  # 原始方式
  # resultNum = []
  # for num in nums:
  #     if num % 2 == 0:
  #         continue
  #     resultNum.append(num ** 2)
  # print(resultNum)
  
  # 列表推导式：映射，语法：[表达式 for 变量 in 列表]
  resultNum = [num ** 2 for num in nums]  # 对 nums 中的每一个元素求平方
  print(resultNum)  # [1, 4, 9, 16, 25]
  
  # 列表推导式：过滤，语法：[表达式 for 变量 in 列表 if 条件]
  resultNum = [num ** 2 for num in nums if num % 2 != 0]  # 对 nums 中的奇数求平方
  print(resultNum)  # [1, 9, 25]
  
  # 循环嵌套
  nums = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
  resultNum = [num2 ** 2 for num in nums for num2 in num]
  print(resultNum)  # [1, 4, 9, 16, 25, 36, 49, 64, 81]
  ```

列表的常用操作：

- 增：

  - `append`：往列表中，追加一个新的元素，新元素在列表的结尾。

  - `insert`：往列表中，指定索引处插入一个新的元素。

  - `extend`：往列表中，扩展另外一个可迭代序列，比如字符串、列表、元组等。

  - `乘法运算`

  - `加法运算`

  - 示例：

    ```python
    nums = [1, 2, 3, 4]
    print(nums)  # [1, 2, 3, 4]
    
    # append 在列表结尾追加一个新元素
    nums.append(5)
    print(nums)  # [1, 2, 3, 4, 5]
    
    # insert 在列表指定索引处插入一个新元素
    nums.insert(0, 0)
    print(nums)  # [0, 1, 2, 3, 4, 5]
    
    # extend 扩展可迭代序列
    str = "abcde"
    strs = ["a", "b", "c", "d", "e"]
    # nums.extend(str)
    nums.extend(strs)
    print(nums)  # [0, 1, 2, 3, 4, 5, 'a', 'b', 'c', 'd', 'e']
    
    # 列表的乘法运算
    nums = [1, 2, 3, 4]
    print(nums * 3)  # [1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4]
    
    # 列表的加法运算
    nums = [1, 2, 3, 4]
    print(nums + ["a", "b", "c", "d"])  # [1, 2, 3, 4, 'a', 'b', 'c', 'd']
    print(nums + "abcd")  # 报错，TypeError: can only concatenate list (not "str") to list，只能同为列表类型的相加
    ```

- 删：

  - `del`：删除列表中的一个指定的元素。

  - `pop`：移除并返回列表中指定索引位置对应的元素，如果不指定索引位置，默认移除列表最后一个元素。

  - `remove`：移除列表中指定元素，没有返回值，如果元素不存在，会报错，如果元素存在多个，则只会删除列表最左边的一个。

  - 示例：

    ```python
    nums = [1, 2, 3, 4]
    print(nums)  # [1, 2, 3, 4]
    
    # del 删除指定的元素
    del nums[1]  # 删除索引位置为 1 的元素
    print(nums)  # [1, 3, 4]
    # 补充L：del 也可以直接删除变量
    a = "666"
    print(a)  # 666
    del a
    # print(a)  # 报错，NameError: name 'a' is not defined
    
    # pop 移除并返回列表中指定索引对应的元素
    print(nums.pop(1))  # 3
    print(nums)  # [1, 4]
    
    # remove 移除列表中指定的元素，没有返回值
    print(nums.remove(1))  # None
    print(nums)  # [4]
    # print(nums.remove(1))  # 报错，ValueError: list.remove(x): x not in list
    
    # 注意：循环 remove 删除列表中指定元素的坑
    nums = [1, 2, 2, 3, 4, 5, 2]
    for num in nums:
        print(num)
        if num == 2:
            nums.remove(num)
    print(nums)  # [1, 3, 4, 5, 2]，会发现，列表最后一个 2 没有被删除
    # 这是因为 for 循环每次移动指针，remove 只删除多个元素的最左边一个，当第一个 2 被删除时，列表后面的数据前移，因为两个 2 靠在一起，会导致后面的
    # 2 迁移到当前指针位置，然后下次循环时，会把这个 2 错过，最终导致会少删除一个 2。也就是说，两个或以上一样的元素靠在一起会出现这个问题，分开则不会，
    # 而且位置不同，最终结果也不太一样，例如 [1, 2, 2, 3, 4, 5] 处理后的结果是 [1, 2, 3, 4, 5]
    # 正确处理方式
    nums = [1, 2, 2, 3, 4, 5, 2]
    while 2 in nums:
        nums.remove(2)
    print(nums)  # [1, 3, 4, 5]
    ```

- 改：

  - `names[index] = 修改后的值`：通过索引位置修改元素的值。

  - 示例：

    ```python
    nums = [1, 2, 3, 4]
    print(nums)  # [1, 2, 3, 4]
    
    # 修改 2 为 -2
    nums[1] = -2
    print(nums)  # [1, -2, 3, 4]
    
    # 超出列表长度的索引，会报错
    nums[4] = 5
    print(nums)  # 报错，IndexError: list assignment index out of range
    ```

- 查：

  - 获取单个元素：`items[index]`，负索引表示从尾部开始。

    ```python
    nums = range(3, 13)
    
    # 获取单个元素
    print(nums[5])  # 8，索引值为 5 对应的元素
    print(nums[-2])  # 11，负索引表示从尾部开始
    ```

  - 获取元素索引：`index()`，从左到右开始查找，返回第一个找到的元素的索引，可以指定查找的起始和结束位置。

    ```python
    nums = range(3, 13)
    
    # 获取元素索引
    idx = nums.index(8)
    print(idx)  # 5，元素 8 对应的索引值
    
    # nums = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
    # idx = nums.index(8, 6, 9) # range() 函数生成的列表，不能指定 index() 方法的起始和结束位置
    # print(idx)  # 报错，ValueError: 8 is not in list
    ```

  - 获取指定元素个数：`count()`。

    ```python
    nums = range(3, 13)
    
    # 获取元素的个数
    print(nums.count(1))  # 0
    print(nums.count(5))  # 1
    ```

  - 获取多个元素：`items[start:end:step]`，切片。

    ```python
    nums = range(3, 13)
    
    # 获取多个元素
    print(nums[0:4:1])  # range(3, 7)
    
    nums = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
    print(nums[0:4:1])  # [3, 4, 5, 6]
    ```

  - 遍历：

    ```python
    nums = range(3, 13)
    
    # 方式一：根据元素进行遍历，语法：for item in list
    currentIndex = 0
    for num in nums:
        print(currentIndex, ":", num)
    
        currentIndex += 1
    
    # 方式二：根据索引进行遍历，语法：for index in range(len(list))
    for index in range(len(nums)):
        print(index, ":", nums[index])
    
    # 方式三：创建对应的枚举对象，然后遍历枚举对象
    # enumerate(nums)：根据列表，创建枚举对象
    # print(enumerate(nums)) # <enumerate object at 0x0000025EF8F0FF40>
    # print(list(enumerate(nums)))  # [(0, 3), (1, 4), (2, 5), (3, 6), (4, 7), (5, 8), (6, 9), (7, 10), (8, 11), (9, 12)]，列表中的每一个元素是一个索引和值组成的元组
    for index, num in enumerate(nums):
        print(index, ":", num)
    
    # 方式四：使用迭代器进行遍历
    iterL = iter(nums)
    for item in iterL:
        print(item)
    
    # 补充，迭代器和迭代对象
    import collections
    
    print(isinstance(nums, collections.abc.Iterable))  # True，判断一个对象是不是可迭代对象
    
    print(isinstance(nums, collections.abc.Iterator))  # False，判断一个对象是不是可迭代器
    
    # iter() 将一个字符串、列表、元组、字典等转换为可迭代器
    i = iter(nums)
    print(i)  # <range_iterator object at 0x0000010822091C30>
    print(isinstance(i, collections.abc.Iterator))  # True
    
    # 可迭代器可以使用 next() 方法，迭代器是可迭代对象，但可迭代对象不一定是可迭代器
    # print(next(i))  # 3
    # print(next(nums))  # TypeError: 'range' object is not an iterator，报错，nums 不是可迭代器
    
    # 迭代器仅仅在迭代到某个元素时才会处理该元素，在此之前，元素可以不存在，在此之后，元素可以被销毁，特别适合用于遍历一些巨大的或是无限的集合
    # 可迭代器可作用于 for in，其内部会自动调用迭代器对象的 next()，同时，也会自动处理迭代完毕的错误
    for v in i:
        print(v)
    
    print("---------")
    
    for v in i:
        print(v)  # 此时不会有输出，迭代器一般不能多次迭代
    ```

- 判定：

  - `元素 in/not in 列表`。

    ```python
    # 判定
    # 元素 in 列表
    # 元素 not in 列表
    
    nums = range(10)
    
    print(1 in nums)  # True
    print(-1 not in nums)  # Truep
    ```

- 排序：

  - 方式一：`sorted(itrearble, key=None, reverse=False)`。

  - 方式二：`list.sort(key=None, reverse=False)`。

  - 示例：

    ```shell
    str = "acbeald"
    intL = ["3", "7", "4", "1", "9"]
    s = [("sz", 18), ("sz2", 16), ("sz1", 17), ("sz3", 15)]
    
    # 方式一
    # 语法 sorted(itrearble, key=None, reverse=False)
    # 可以对所有可迭代对象进行排序，key 为排序的关键字，是一个函数，reverse 为升序降序，默认升序
    # 结果返回一个排序后的列表
    print(sorted(str))  # ['a', 'a', 'b', 'c', 'd', 'e', 'l']
    print(sorted(intL, reverse=True))  # ['9', '7', '4', '3', '1']
    print(sorted(s))  # [('sz', 18), ('sz1', 17), ('sz2', 16), ('sz3', 15)]，根据元组的首个元素排序
    
    
    # 定义一个函数，接受一个元组的数据，返回元组的第二个元素
    def getKey(x):
        return x[1]
    
    
    print(sorted(s, key=getKey))  # [('sz3', 15), ('sz2', 16), ('sz1', 17), ('sz', 18)]
    
    # 方式二
    # 列表的对象方法，语法 list.sort(key=None, reverse=False)
    # 会改变原列表
    intL.sort()  # 调用方法排序，无返回值
    print(intL)  # ['1', '3', '4', '7', '9']
    s.sort(key=getKey, reverse=True)
    print(s)  # [('sz', 18), ('sz1', 17), ('sz2', 16), ('sz3', 15)]
    ```

- 乱序：

  - `random.shuffle(list)`

    ```python
    import random
    
    # 乱序 random.shuffle
    intL = [1, 2, 3, 4, 5]
    print(random.shuffle(intL), intL)  # None [4, 2, 1, 3, 5]
    ```
    

- 比较：

  - `比较运算符`：>，==，<。

    ```python
    print([2, 3, 4] > [2, 3, 3])  # True
    print([2, 3, 4] > [2, 3])  # True
    ```
    
  
- 反转：

  - 方式一：`list.reverse()`

  - 方式二：切片反转，`list[::-1]`。

  - 示例：

    ```python
    import random
    
    intL = [1, 2, 3, 4, 5]
    
    # 方式一 list.reverse()
    # print(intL.reverse(), intL)  # None [5, 4, 3, 2, 1]
    
    # 方式二 list[::-1]
    print(intL[::-1], intL)  # [5, 4, 3, 2, 1] [1, 2, 3, 4, 5]
    ```

## 元组

**`元组`**：有序的不可变的元素集合，和列表的区别就是，元组元素不能修改。

**元组的定义**：

- 一个元素的写法：`(元素,)`，逗号必须添加，避免与数值运算的括号混淆。

  ```python
  t = (1,)
  print(t, type(t))  # (1,) <class 'tuple'>
  ```

- 多个元素的写法：`(元素1, 元素2, 元素3, ...)`，以逗号隔开。

  ```python
  t = (1, 2)
  print(t, type(t))  # (1, 2) <class 'tuple'>
  # 元组中的元素，可以是多种类型
  t = ("abc", [1, 2, 3], 45, "ds")
  print(t, type(t))  # ('abc', [1, 2, 3], 45, 'ds') <class 'tuple'>
  # 多个元素以逗号分割，默认为元组，不常用
  t = 1, 2, 3, "abc"
  print(t, type(t))  # (1, 2, 3, 'abc') <class 'tuple'>
  ```

- 从列表转换为元组：

  ```python
  l = [1, 2, 3, 4, 5]
  c = tuple(l)
  print(c, type(c))  # (1, 2, 3, 4, 5) <class 'tuple'>
  ```

- 元组嵌套：元组中的元素，也可以是元组。

  ```python
  t = (1, ("a", "b"), 2, "ab")
  print(t, type(t))  # (1, ('a', 'b'), 2, 'ab') <class 'tuple'>
  ```

**元组的常用操作**：

- 由于元组是不可变的元素，因此，元组没有增、删、改方法。

- 查：

  ```python
  t = (1, ("a", "b"), 2, "ab")
  
  # 获取单个元素
  print(t[1])  # ('a', 'b')
  # print(t[4])  # 报错，IndexError: tuple index out of range
  print(t[-1])  # ab
  
  # 获取多个元素，切片
  print(t[0:2])  # (1, ('a', 'b'))
  print(t[3:1:-1])  # ('ab', 2)
  print(t[1:3:-1])  # ()
  ```

- 获取：

  - `tuple.count(item)`：统计元组中指定元素的个数。

  - `tuple.index(item)`：获取元组中指定元素的索引。

  - `len(tuple)`：返回元组中元素的个数。

  - `max(tuple)`：返回元组中元素最大的值。

  - `min(tuple)`：返回元组中元素最小的值。

  - 示例：

    ```python
    t = (1, 2, 3, 2, 4)
    
    # tuple.count(item)：统计元组中指定元素的个数
    print(t.count(2))  # 2
    # tuple.index(item)：获取元组中指定元素的索引
    print(t.index(2))  # 1
    # print(t.index(12)) # 报错，ValueError: tuple.index(x): x not in tuple
    # len(tuple)：返回元组中元素的个数
    print(len(t))  # 5
    # max(tuple)：返回元组中元素最大的值
    print(max(t))  # 4
    # min(tuple)：返回元组中元素最小的值
    print((min(t)))  # 1
    ```

- 判定：

  - `元素 in/not in 元组`。

    ```python
    t = (1, 2, 3, 2, 4)
    
    # 元素 in 元组
    print(1 in t)  # True
    # 元素 not in 元组
    print(1 not in t)  # False
    ```

- 比较：

  - `比较运算符`：>，==，<。

    ```python
    # 比较运算符
    print((1, 2) > (3, 2))  # False
    ```

- 拼接：

  - 乘法：`元组 * int类型数值`。

  - 加法：`元组1 + 元组2`。

  - 示例：

    ```python
    # 元组的乘法
    print((1, 2) * 3)  # (1, 2, 1, 2, 1, 2)
    # 元组的加法
    print((1, 2) + ("a", "b"))  # (1, 2, 'a', 'b')
    print((1, 2) + ("a", "b", "c"))  # (1, 2, 'a', 'b', 'c')
    ```

- 拆包：

  - `b, a = (a, b)`。

    ```python
    # 拆包
    a = 10
    b = 20
    b, a = (a, b)
    print(a, b)  # 20 10
    ```

## 字典

**`字典`**：无序的，可变的键值对集合。

**字典的定义**：

- 方式一：`{key1:value1, key2:value2, ...}`。

  ```python
  d = {"name": "zhangsan", "age": 18}
  print(d, type(d))  # {'name': 'zhangsan', 'age': 18} <class 'dict'>
  print(d["name"])  # zhangsan
  print(d["age"])  # 18
  ```

- 方式二：静态方法调用，`dict.fromkeys()`。

  ```python
  # 类调用，第一个参数是一个序列
  d = dict.fromkeys("abc", 666)
  print(d, type(d))  # {'a': 666, 'b': 666, 'c': 666} <class 'dict'>
  
  d = dict.fromkeys(["a", "b", "c"], 666)
  print(d, type(d))  # {'a': 666, 'b': 666, 'c': 666} <class 'dict'>
  
  # 对象调用，实用性不大
  e = {}
  f = e.fromkeys("abc", 666)
  print(f, type(d))  # {'a': 666, 'b': 666, 'c': 666} <class 'dict'>
  ```
  

**字典的特点**：

- `key 不能重复`，如果重复，后面的值会覆盖前面的值。

  ```python
  # key 不能重复，如果重复，后面的值会覆盖前面的值
  d = {1: "a", 2: "b", 1: "c"}
  print(d[1])  # c
  ```

- `key 必须是任意不可变类型`。可变类型：列表、字典、可变集合等，不可变类型：数值、布尔值、字符串、元组等。

  ```python
  # key 必须是任意不可变类型
  # d = {[1, 2, 3]: "abc"}  # 报错，TypeError: unhashable type: 'list'，key不能是 list
  
  num = 10
  print(id(num))  # 140718840294104
  num = 20
  print(id(num))  # 140718840294424
  l = [1, 2, 3]
  print(id(l))  # 1638094475584
  l.append(4)
  print(id(l))  # 1638094475584，值发生了改变，但 id 未变，是可变类型
  ```

- 原因：Python 的字典，采用哈希（hash）的方式实现，即将 key 转换为 hash 值再存储。

**字典的常用操作**：

- 增：`dict[key] = value`，当 key 在原字典中不存在时，即为新增操作。

  ```python
  d = {"name": "zhangsan", "age": 18}
  print(d)  # {'name': 'zhangsan', 'age': 18}
  d["height"] = 180
  print(d)  # {'name': 'zhangsan', 'age': 18, 'height': 180}
  ```

- 删：

  - `del dic[key]`：key 必须存在，否则报错。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    del dic["age"]
    print(dic)  # {'name': 'zhangsan'}
    del dic["age2"]  # 报错，KeyError: 'age2'
    print(dic)
    ```

  - `dic.pop(key[, default])`：删除指定的 key，并返回对应的 value，如果 key 不存在，则直接返回给定的 default 值，如果没有指定 default 值，则会报错。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    v = dic.pop("name")
    print(dic)  # {'age': 18}
    print(v)  # zhangsan
    # 如果 key 不存在，则必须给定 default 值，否则报错
    # print(dic.pop("age2"))  # 报错，KeyError: 'age2'
    print(dic.pop("age2", 666))  # 666
    ```

  - `dic.popitem()`：删除字典按照 key 升序排序后的第一个键值对，并以元组方式返回，如果字典为空，会报错。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    print(dic.popitem())  # ('age', 18)
    print(dic)  # {'name': 'zhangsan'}
    dic = {}
    dic.popitem()  # 报错，KeyError: 'popitem(): dictionary is empty'
    ```

  - `dic.clear()`：删除字典内所有的键值对，返回 None，注意，字典本身还存在，只是字典内的值被清空了，这与 del 不同，del 是直接把字典删除。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    print(dic.clear())  # None
    print(dic)  # {}，字典还存在
    
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    del dic  # dic 被删除了
    print(dic)  # 报错，NameError: name 'dic' is not defined. Did you mean: 'dir'?
    ```

- 改：

  - 修改只能修改字典中的 value，不能修改 key。

  - 修改单个键值对：`dic[key] = newValue`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    dic["age"] = 20
    print(dic)  # {'name': 'zhangsan', 'age': 20}
    ```

  - 批量修改键值对：`oldDic.update(newDic)`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    dic.update({"name": "lisi", "age": 25, "address": "hefei"})
    print(dic)  # {'name': 'lisi', 'age': 25, 'address': 'hefei'}
    ```

- 查：

  - 获取单个值：

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    # 方式一 dic[key]，如果 key 不存在，会报错
    print(dic["name"])  # zhangsan
    
    # 方式二 dic.get(key[, default])，如果 key 不存在，则返回指定的 default 值，如果没有指定 default，返回 None，不会报错
    print(dic.get("name"))  # zhangsan
    print(dic.get("name2", 666))  # 666
    print(dic.get("name2"))  # None
    
    # 方式三 dic.setdefault(key[, default])，如果 key 不存在，则返回指定的 default 值，并将该键值对添加到字典中，如果没有指定 default，则使用 None 代替
    print(dic)  # {'name': 'zhangsan', 'age': 18}
    print(dic.setdefault("address", "hefei"))  # hefei
    print(dic.setdefault("school"))  # None
    print(dic)  # {'name': 'zhangsan', 'age': 18, 'address': 'hefei', 'school': None}
    ```

  - 获取所有值：`dic.values()`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    print(dic.values())  # dict_values(['zhangsan', 18])
    ```

  - 获取所有的键：`dic.keys()`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    print(dic.keys())  # dict_keys(['name', 'age'])
    ```

  - 获取字典的键值对：`dic.items()`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    print(dic.items())  # dict_items([('name', 'zhangsan'), ('age', 18)])
    ```

  - 遍历：

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    # 方式一 遍历所有的 key，根据 key 获取 value
    keys = dic.keys()
    for key in keys:
        print(key, dic[key])
    
    # 方式二 遍历所有的键值对
    kvs = dic.items()
    for key, value in kvs:
        print(key, value)
    ```

- 计算：

  - 键值对的个数：`len(dic)`。

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    print(len(dic))  # 2
    ```

- 判定：

  - 判断是否含有某个 key：

    ```python
    dic = {"name": "zhangsan", "age": 18}
    
    # 方式一 key in dic
    print("name" in dic)  # True
    
    # 方式二 key not in dic
    print("name" not in dic)  # False
    ```

## 集合

**`集合`**：无序的，不可随机访问的，不可重复的元素集合。

- 可变集合：`set`，可以进行增删改操作。
- 不可变集合：`frozenset`，创建之后，无法进行增删改操作。

**集合的定义**：

- 可变集合：

  ```python
  # 方式一 {元素1, 元素2, ...}
  s = {1, 2, 3}
  print(s, type(s))  # {1, 2, 3} <class 'set'>
  
  a = {}
  print(type(a))  # <class 'dict'>，单独定义的 {}，是一个字典，不是集合
  
  # 方式二 set(iterable)，将可迭代对象转为集合，例如字符串、列表、元组、字典等
  l = {1, 2, 3}
  s = set(l)
  print(s, type(s))  # {1, 2, 3} <class 'set'>
  dic = {"name": "zhangsan", "age": 18}
  s = set(dic)
  print(s, type(s))  # {'age', 'name'} <class 'set'>，如果是字典，则只会将 key 放入集合中
  
  # 方式三 集合推导式
  s = set(x ** 2 for x in range(10) if x % 2 == 0)  # 10 以内偶数的平方
  print(s, type(s))  # {0, 64, 4, 36, 16} <class 'set'>
  s = {x ** 2 for x in range(20) if x % 2 == 0}  # 此方式也可以
  print(s, type(s))  # {0, 64, 256, 4, 36, 100, 196, 324, 16, 144} <class 'set'>
  ```

- 不可变集合：

  ```python
  # 方式一 frozenset(iterable)，将可迭代对象转为集合，例如字符串、列表、元组、字典等
  l = {1, 2, 3}
  s = frozenset(l)
  print(s, type(s))  # frozenset({1, 2, 3}) <class 'frozenset'>
  dic = {"name": "zhangsan", "age": 18}
  s = frozenset(dic)
  print(s, type(s))  # frozenset({'age', 'name'}) <class 'frozenset'>，如果是字典，则只会将 key 放入集合中
  
  # 方式二 集合推导式
  s = frozenset(x ** 2 for x in range(10) if x % 2 == 0)  # 10 以内偶数的平方
  print(s, type(s))  # frozenset({0, 64, 4, 36, 16}) <class 'frozenset'>
  ```

- 注意事项：

  - 创建空集合时，使用`set()`或者`frozenset()`，不能使用 {}。
  - 集合中的元素，必须是可哈希的值。
  - 如果集合中的元素出现重复，则会合并为一个。

**集合的常用操作**：

- 单一集合的操作：

  - 可变集合：增删改查。

    ```python
    s = {1, 2, 3}
    print(s, type(s))  # {1, 2, 3} <class 'set'>
    
    # 增：
    s.add(4)
    print(s)  # {1, 2, 3, 4}
    
    # 删：
    # 方式一 s.remove(element)，如果 element 不存在，则报错
    # s.remove(4)
    # print(s)  # {1, 2, 3}
    # s.remove(4)  # 报错，KeyError: 4
    
    # 方式二 s.discard(element)，如果 element 不存在，则 do nothing
    # s.discard(4)
    # print(s)  # {1, 2, 3}
    # s.discard(4)  # do nothing
    
    # 方式三 s.pop()，随机删除并返回一个集合中的元素，如果集合为空，则返回一个错误
    # print(s.pop(), s)  # 1 {2, 3, 4}
    # s2 = set()
    # s2.pop()  # 报错，KeyError: 'pop from an empty set'
    
    # 方式四 s.clear()，清空集合中所有的元素
    # s.clear()
    # print(s)  # set()
    
    # 改：集合中元素是不可变类型，无法修改
    
    # 查：无法通过索引或 key 查询
    # 方式一 通过 for in 遍历
    for element in s:
        print(element)
    
    # 方式二 通过迭代器访问
    its = iter(s)
    for element in its:
        print(element)
    ```

  - 不可变集合：不可增删改，只能查。

    ```python
    s = frozenset({1, 2, 3})
    print(s, type(s))  # {1, 2, 3} <class 'set'>
    
    # 方式一 通过 for in 遍历
    for element in s:
        print(element)
    
    # 方式二 通过迭代器访问
    its = iter(s)
    for element in its:
        print(element)
    ```

- 集合之间的操作：

  - 交集：

    ```python
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5, 6, 7, 8}
    
    # intersection()，不会修改原集合对象
    s3 = s1.intersection(s2)
    print(s3, type(s3))  # {4, 5} <class 'set'>
    
    # intersection() 方法接受一个可迭代对象，可以是字符串、列表、元组、字典、集合等，等同于 s1 & s2
    print(s1.intersection("34567"))  # set()，入参是字符串，转为集合 {"3", "4", "5", "6", "7"}，与 int 值交集为空
    print(s1.intersection([3, 4, 5, 6, 7]))  # {3, 4, 5}
    print(s1.intersection({3: "3", 4: "4", 5: "5", 6: "6", 7: "7"}))  # {3, 4, 5}，与字典的 key 组成的集合求交集
    
    # 可变集合与不可变集合的交集运算
    s1 = frozenset({1, 2, 3, 4, 5})
    s2 = {4, 5, 6, 7, 8}
    s3 = s1.intersection(s2)
    print(s3, type(s3))  # frozenset({4, 5}) <class 'frozenset'>，不可变集合在前，求交集后返回的是不可变集合
    s4 = s2.intersection(s1)
    print(s4, type(s4))  # {4, 5} <class 'set'>，可变集合在前，求交集后返回的是可变集合
    
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5, 6, 7, 8}
    
    # intersection_update()，会修改原集合对象，如果调用者是不可变集合，会报错
    s3 = s1.intersection_update(s2)  # 方法无返回值
    print(s3, type(s3))  # None <class 'NoneType'>
    print(s1)  # {4, 5}
    print(s2)  # {4, 5, 6, 7, 8}
    ```

  - 并集：

    ```python
    s1 = {1, 2, 3}
    s2 = {4, 5, 6}
    
    # union()，不会修改原集合对象，等同于 s1 | s2，如果是可变集合与不可变集合的混合交集运算，得到的结果与调用者同类型
    s3 = s1.union(s2)
    print(s3, type(s3))  # {1, 2, 3, 4, 5, 6} <class 'set'>
    
    # update()，会修改原集合对象，如果调用者是不可变集合，会报错
    s4 = s1.update(s2)  # 方法无返回值
    print(s4)  # None
    print(s1)  # {1, 2, 3, 4, 5, 6}
    print(s2)  # {4, 5, 6}
    ```

  - 差集：

    ```python
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5, 6}
    
    # difference()，不会修改原集合对象，等同于 s1 - s2，如果是可变集合与不可变集合的混合交集运算，得到的结果与调用者同类型
    s3 = s1.difference(s2)
    print(s3, type(s3))  # {1, 2, 3} <class 'set'>
    
    # difference_update()，会修改原集合对象，如果调用者是不可变集合，会报错
    s3 = s1.difference_update(s2)  # 方法无返回值
    print(s3, type(s3))  # None <class 'NoneType'>
    print(s1)  # {1, 2, 3}
    print(s2)  # {4, 5, 6}
    ```

  - 判定：

    ```python
    # isdisjoint()，判定两个集合不相交
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5, 6}
    print(s1.isdisjoint(s2))  # False
    
    # issuperset()，判定一个集合包含另一个集合
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5}
    print(s1.issuperset(s2))  # True
    
    # issubset()，判定一个集合包含于另一个集合
    s1 = {1, 2, 3, 4, 5}
    s2 = {4, 5}
    print(s2.issubset(s1))  # True
    ```

## 时间日历

### time 模块

`time 模块`：提供了处理时间和表示之间转换的功能。

- 获取当前时间戳：`time.time()`。

  ```python
  import time
  
  # 时间戳：从 0 时区的 1970 年 1 月 1 日 0 时 0 分 0 秒，到当前时间的秒数，是一个浮点数
  result = time.time()
  print(result)  # 1739978517.524817
  
  year = result / (365 * 24 * 60 * 60) + 1970
  print(year)  # 2025.174356846931
  ```

- 获取时间元组：`time.localtime([seconds])`。很多 Python 时间函数将时间处理为 9 个数字的元组。

  | 序号 | 属性名   | 含义           | 范围                   |
  | :--- | :------- | :------------- | :--------------------- |
  | 0    | tm_year  | 年份           | [1900, 2199]           |
  | 1    | tm_mon   | 月份           | [1, 12]                |
  | 2    | tm_mday  | 日期           | [1, 31]                |
  | 3    | tm_hour  | 小时           | [0, 23]                |
  | 4    | tm_min   | 分钟           | [0, 59]                |
  | 5    | tm_sec   | 秒             | [0, 61]（允许闰秒）    |
  | 6    | tm_wday  | 星期几         | [0, 6]（0 表示星期一） |
  | 7    | tm_yday  | 一年中的第几天 | [1, 366]               |
  | 8    | tm_isdst | 是否为夏令时   | 0 或 1                 |

  ```python
  import time
  
  # time.localtime([seconds])，seconds 是可选的时间戳，默认是当前时间戳
  result = time.localtime()
  print(result)  # time.struct_time(tm_year=2025, tm_mon=2, tm_mday=19, tm_hour=23, tm_min=30, tm_sec=55, tm_wday=2, tm_yday=50, tm_isdst=0)
  
  result = time.localtime(1728878517.524817)
  print(result)  # time.struct_time(tm_year=2024, tm_mon=10, tm_mday=14, tm_hour=12, tm_min=1, tm_sec=57, tm_wday=0, tm_yday=288, tm_isdst=0)
  ```

- 获取格式化的时间：`time.ctime([seconds])`和`time.asctime([p_tuple])`。

  ```python
  import time
  
  # time.ctime([seconds])：将时间戳转换为格式化的时间，seconds 是可选的时间戳，默认是当前时间戳
  result = time.ctime()
  print(result)  # Wed Feb 19 23:39:49 2025
  
  result = time.ctime(1728878517.524817)
  print(result)  # Mon Oct 14 12:01:57 2024
  
  # time.asctime([p_tuple])：将时间元组转换为格式化的时间，p_tuple 是可选的时间元组，默认是当前时间元组
  result = time.asctime()  # Wed Feb 19 23:39:49 2025
  print(result)
  
  result = time.asctime(time.localtime(1728878517.524817))
  print(result)  # Mon Oct 14 12:01:57 2024
  ```

- 格式化字符串、时间元组、时间戳之间的转换：`time.strftime(格式符字符串, 时间元组)`，`time.strptime(格式化字符串, 格式符字符串)`和`time.mktime(时间元组)`。

  ```python
  import time
  
  # time.strftime(格式符字符串, 时间元组)：将时间元组转换为格式化字符串
  result = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
  print(result)  # 2025-02-19 23:44:13
  
  result = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(1728878517.524817))
  print(result)  # 2024-10-14 12:01:57
  
  # 年数保留两位
  result = time.strftime("%y-%m-%d %H:%M:%S", time.localtime())
  print(result)  # 25-02-19 23:45:24
  
  # time.strptime(格式化字符串, 格式符字符串)：将格式化字符串转换为时间元组，注意，给定的格式化字符串需要与格式符字符串的格式对应
  result = time.strptime("2024-10-14 12:01:57", "%Y-%m-%d %H:%M:%S")
  print(result)  # time.struct_time(tm_year=2024, tm_mon=10, tm_mday=14, tm_hour=12, tm_min=1, tm_sec=57, tm_wday=0, tm_yday=288, tm_isdst=-1)
  result = time.strptime("25-02-19 23:45:24", "%y-%m-%d %H:%M:%S")
  print(result)  # time.struct_time(tm_year=2025, tm_mon=2, tm_mday=19, tm_hour=23, tm_min=45, tm_sec=24, tm_wday=2, tm_yday=50, tm_isdst=-1)
  
  # time.mktime(时间元组)：将时间元组转换为时间戳
  result = time.mktime(time.localtime())
  print(result)  # 1739980654.0
  result = time.mktime(time.strptime("2024-10-14 12:01:57", "%Y-%m-%d %H:%M:%S"))
  print(result)  # 1728878517.0
  ```

  > 补充：Python 中常用的时间格式符。
  >
  > | 格式符 | 含义                             | 示例                       |
  > | :----- | :------------------------------- | :------------------------- |
  > | `%Y`   | 四位数的年份                     | `2024`                     |
  > | `%m`   | 两位数的月份（01-12）            | `02`                       |
  > | `%d`   | 两位数的日期（01-31）            | `20`                       |
  > | `%H`   | 两位数的小时（24小时制，00-23）  | `14`                       |
  > | `%I`   | 两位数的小时（12小时制，01-12）  | `02`                       |
  > | `%M`   | 两位数的分钟（00-59）            | `30`                       |
  > | `%S`   | 两位数的秒（00-61，允许闰秒）    | `45`                       |
  > | `%f`   | 微秒（六位数，000000-999999）    | `123456`                   |
  > | `%a`   | 本地化的星期几缩写               | `Wed`                      |
  > | `%A`   | 本地化的星期几全称               | `Wednesday`                |
  > | `%b`   | 本地化的月份缩写                 | `Feb`                      |
  > | `%B`   | 本地化的月份全称                 | `February`                 |
  > | `%c`   | 本地化的日期和时间表示           | `Wed Feb 20 14:30:45 2024` |
  > | `%x`   | 本地化的日期表示                 | `02/20/24`                 |
  > | `%X`   | 本地化的时间表示                 | `14:30:45`                 |
  > | `%p`   | 本地化的 AM/PM                   | `PM`                       |
  > | `%z`   | UTC 时区偏移量（+HHMM 或 -HHMM） | `+0800`                    |
  > | `%Z`   | 时区名称                         | `CST`                      |
  > | `%%`   | 字面量 `%`                       | `%`                        |

- 获取当前 CPU 时间：`time.perf_counter()`。通常用于测量时间间隔或计算程序运行时间，它提供了比`time.time()`更高的精度，并且不会受到系统时间调整的影响。

  ```python
  import time
  
  # 应用一 测量代码执行时间
  # 开始计时
  start_time = time.perf_counter()
  
  # 执行一些操作
  for i in range(1000000):
      pass
  
  # 结束计时
  end_time = time.perf_counter()
  
  # 计算执行时间
  elapsed_time = end_time - start_time
  print(f"Elapsed time: {elapsed_time:.6f} seconds")  # Elapsed time: 0.034983 seconds
  
  # 应用二 比较不同代码片段的性能
  # 测试代码片段 1
  start_time = time.perf_counter()
  result = sum(range(1000000))
  end_time = time.perf_counter()
  print(f"Sum range: Elapsed time = {end_time - start_time:.6f} seconds")  # Sum range: Elapsed time = 0.016414 seconds
  
  # 测试代码片段 2
  start_time = time.perf_counter()
  result = sum(x for x in range(1000000))
  end_time = time.perf_counter()
  print(f"Sum generator: Elapsed time = {end_time - start_time:.6f} seconds")  # Sum generator: Elapsed time = 0.057573 seconds
  
  
  # 应用三 实现一个简单的计时器
  class Timer:
      def __init__(self):
          self.start_time = None
          self.elapsed_time = 0
  
      def start(self):
          if self.start_time is not None:
              raise RuntimeError("Timer is already running.")
          self.start_time = time.perf_counter()
  
      def stop(self):
          if self.start_time is None:
              raise RuntimeError("Timer is not running.")
          self.elapsed_time = time.perf_counter() - self.start_time
          self.start_time = None
  
      def reset(self):
          self.start_time = None
          self.elapsed_time = 0
  
      def __enter__(self):
          self.start()
          return self
  
      def __exit__(self, exc_type, exc_val, exc_tb):
          self.stop()
  
  
  # 使用 Timer 类
  timer = Timer()
  timer.start()
  time.sleep(2)  # 模拟一些操作
  timer.stop()
  print(f"Elapsed time: {timer.elapsed_time:.6f} seconds")  # Elapsed time: 2.000239 seconds
  
  # 使用上下文管理器
  with Timer() as timer:
      time.sleep(1)
  print(f"Elapsed time: {timer.elapsed_time:.6f} seconds")  # Elapsed time: 1.000344 seconds
  ```

  - **精度**：time.perf_counter() 的精度通常比 time.time() 更高，但它返回的是从某个固定点开始的秒数，而不是绝对时间。
  - **系统依赖**：time.perf_counter() 的实现依赖于底层操作系统的支持，因此在不同系统上可能会有不同的行为。
  - **不适用于长时间测量**：虽然 time.perf_counter() 提供了高精度，但它主要用于测量短时间间隔。对于长时间测量，建议使用 time.time() 或 datetime 模块。

- 休眠：`time.sleep(seconds)`。

  ```python
  import time
  
  for i in range(5):
      print(f"Step {i + 1}")
      time.sleep(1)  # 每次循环暂停 1 秒
  ```

  - **阻塞调用**：time.sleep() 是一个阻塞调用，它会暂停当前线程的执行。如果在多线程环境中使用，只会暂停当前线程，不会影响其他线程。
  - **精度问题**：虽然 time.sleep() 的精度通常很高，但实际暂停时间可能会受到系统调度的影响，尤其是在高负载的系统上。
  - **浮点数支持**：time.sleep() 支持浮点数参数，可以指定更精确的暂停时间。例如，time.sleep(0.5) 会暂停 0.5 秒。
  - **中断信号**：在某些操作系统上，time.sleep() 可能会被中断信号（如 SIGINT）打断。如果需要处理这种情况，可以在代码中捕获信号。

### calendar 模块

`calendar 模块`：提供与日历相关的功能。

- 给指定的月份或年份，打印文本日历。

  ```python
  import calendar
  
  # calendar.month(theyear, themonth)
  print(calendar.month(2025, 2))
  ```

  ```python
     February 2025
  Mo Tu We Th Fr Sa Su
                  1  2
   3  4  5  6  7  8  9
  10 11 12 13 14 15 16
  17 18 19 20 21 22 23
  24 25 26 27 28
  ```

### datetime 模块

`datetime 模块`：Python 处理日期和时间的标准库，这个模块里有 datetime 类，date 类和 time 类，可以做一些计算之类的操作。

- 获取当前日期：

  ```python
  import datetime
  
  print(datetime.datetime.now())  # 2025-02-20 09:53:29.779501
  
  print(datetime.datetime.today())  # 2025-02-20 09:53:29.780498
  ```

- 单独获取当前的年月日时分秒：

  ```python
  import datetime
  
  now = datetime.datetime.now()
  print(now, type(now))  # 2025-02-20 09:59:01.140946 <class 'datetime.datetime'>
  print(now.year, now.month, now.day)  # 2025 2 20
  print(now.hour, now.minute, now.second)  # 9 59 1
  ```

- 计算 n 天之后的日期：

  ```python
  import datetime
  
  now = datetime.datetime.now()
  print(now)  # 2025-02-20 10:05:30.005794
  # datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
  print(now + datetime.timedelta(days=7))  # 2025-02-27 10:05:30.005794
  ```

- 获取两个日期时间的时间差：

  ```python
  import datetime
  
  first = datetime.datetime(2025, 2, 20, 12, 0, 0)
  end = datetime.datetime(2025, 1, 10, 12, 0, 0)
  result = first - end
  print(result, type(result))  # 41 days, 0:00:00 <class 'datetime.timedelta'>，两个时间的间隔天数
  print(result.total_seconds())  # 3542400.0，两个时间的间隔秒数
  ```

## 本文参考

https://www.bilibili.com/video/BV12b411g7G7
