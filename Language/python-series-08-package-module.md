>*`Author: ACatSmiling`*
>
>*`Since: 2024-11-23`*

## 概述

**`模块（Module）`**：**模块是一个包含 Python 代码的文件，文件名就是模块名（去掉 .py 后缀）。**它可以包含变量定义、函数定义、类定义等各种 Python 代码元素。例如，创建一个名为 math_utils.py 的文件，在其中定义了一些数学相关的函数，如 add_numbers 和 multiply_numbers，那么这个 math_utils.py 就是一个模块。

**`包（Package）`**：**包是一种组织模块的方式，它是一个包含多个模块的目录。**为了让 Python 将一个目录识别为包，该目录下必须包含一个`__init__.py`文件，这个文件可以是空的，也可以包含一些用于初始化包的代码。例如，创建一个名为 my_package 的目录，在其中放入 \__init__.py 文件和几个 .py 模块文件（如 module1.py、module2.py），那么这个 my_package 就是一个包。

查看模块和包的存放位置：

```python
import json

# 查看 json 的路径
print(json.__file__)

# 查看 json 中包含的内容
print(dir(json))
```

输出结果：

```python
C:\Users\XiSun\AppData\Local\Programs\Miniconda3\envs\py310\lib\json\__init__.py
['JSONDecodeError', 'JSONDecoder', 'JSONEncoder', '__all__', '__author__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', '__version__', '_default_decoder', '_default_encoder', 'codecs', 'decoder', 'detect_encoding', 'dump', 'dumps', 'encoder', 'load', 'loads', 'scanner']
```

