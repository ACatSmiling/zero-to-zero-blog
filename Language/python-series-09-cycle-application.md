> `Author: ACatSmiling`
>
> `Since: 2024-12-05`

## 反转字符串

```python
"""
反转字符串
    源字符串："小荷才露尖尖角，早有蜻蜓立上头"
    反转字符串：”头上立蜓蜻有早，角尖尖露才荷小“
"""

notice = "小荷才露尖尖角，早有蜻蜓立上头"
result = ""
for char in notice:
    result = char + result
    
print(result)
```

## 九九乘法表

```python
"""
九九乘法表
    1 * 1 = 1
    1 * 1 = 1	2 * 2 = 4
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16	5 * 5 = 25
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16	5 * 5 = 25	6 * 6 = 36
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16	5 * 5 = 25	6 * 6 = 36	7 * 7 = 49
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16	5 * 5 = 25	6 * 6 = 36	7 * 7 = 49	8 * 8 = 64
    1 * 1 = 1	2 * 2 = 4	3 * 3 = 9	4 * 4 = 16	5 * 5 = 25	6 * 6 = 36	7 * 7 = 49	8 * 8 = 64	9 * 9 = 81
"""

for num in range(1, 10):
    nums = range(1, num + 1)
    for n in nums:
        print("%d * %d = %d" % (n, n, n * n), end="\t")
    # 换行
    print("")

```

## 水仙花数

```python
"""
水仙花数
    对于一个三位数，满足：百位数的 3 次方 + 十位数的 3 次方 + 个位数的 3 次方 = 数值本身
    水仙花数：153，370，371，407
"""

for num in range(100, 1000):
    # 百位数
    bai = num // 100
    # 十位数
    shi = num % 100 // 10
    # 个位数
    ge = num % 10
    result = (bai ** 3 + shi ** 3 + ge ** 3 == num)
    if result:
        print("水仙花数：%d" % num)
```