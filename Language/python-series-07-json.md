>*`Author: ACatSmiling`*
>
>*`Since: 2024-10-09`*

需求：将文件中的 JSON 格式数据，指定 Key 的值替换。

**对于文件的整个文本为 JSON 格式的数据。**示例：

```json
{
    'A': {
        'a': 23.45,
        'b': 56.78,
        'c': 89.01
    }
}
```

Python 代码：

```python
import json

# 读取原始 JSON 数据
with open('original_data.json', 'r') as file:
    data = json.load(file)

# 处理数据
inner_data = data['A']
inner_data['a'] /= 100
inner_data['b'] /= 100
inner_data['c'] /= 100

# 将处理后的数据重新封装为 JSON 并写入文件
with open('processed_data.json', 'w') as file:
    json.dump(data, file)
```

**对于文件的每一行为 JSON 格式的数据。**示例：

```json
{'A': {'a': 23.45, 'b': 56.78, 'c': 89.01}}
{'A': {'a': 23.45, 'b': 56.78, 'c': 89.01}}
{'A': {'a': 23.45, 'b': 56.78, 'c': 89.01}}
```

Python 代码：

```python
import json

with open('original_file.txt', 'r') as infile, open('processed_file.txt', 'w') as outfile:
    for line in infile:
        data = json.loads(line)
        for key, inner_dict in data.items():
            if 'a' in inner_dict:
                inner_dict['a'] /= 100
            if 'b' in inner_dict:
                inner_dict['b'] /= 100
            if 'c' in inner_dict:
                inner_dict['c'] /= 100
        outfile.write(json.dumps(data) + '\n')
```

