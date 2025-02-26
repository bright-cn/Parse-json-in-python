# 使用 Python 解析 JSON 数据

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/) 

本指南介绍如何使用 Python 的 `json` 模块解析 JSON 数据，并将其转换为 Python 字典以及相反方向的转换。

- [Python 中的 JSON 简介](#Python-中的-JSON-简介)
- [使用 Python 解析 JSON 数据](#使用-Python-解析-JSON-数据)
- [将 JSON 字符串转换为 Python 字典](#将-JSON-字符串转换为-Python-字典)
  - [将 JSON API 响应转换为 Python 字典](#将-JSON-API-响应转换为-Python-字典)
  - [将 JSON 文件加载到 Python 字典](#将-JSON-文件加载到-Python-字典)
  - [从 JSON 数据转换为自定义 Python 对象](#从-JSON-数据转换为自定义-Python-对象)
- [将 Python 数据转换成 JSON](#将-Python-数据转换成-JSON)
- [使用 `json` 标准模块的局限性](#使用-`json`-标准模块的局限性)
- [结论](#结论)

## Python 中的 JSON 简介

JavaScript Object Notation（JSON）是一种轻量级的数据交换格式，通常用于通过 API 在服务器和 Web 应用之间传输数据。JSON 数据由键值对组成，其中键是字符串，值可以是字符串、数字、布尔值、null、数组或对象。

以下是一个 JSON 示例：

```json
{
  "name": "Maria Smith",
  "age": 32,
  "isMarried": true,
  "hobbies": ["reading", "jogging"],
  "address": {
    "street": "123 Main St",
    "city": "San Francisco",
    "state": "CA",
    "zip": "12345"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "555-555-1234"
    },
    {
      "type": "work",
      "number": "555-555-5678"
    }
  ],
  "notes": null
}
```

Python 原生支持 [JSON](https://docs.python.org/3/library/json.html)，通过 `json` 模块实现，它是 [Python 标准库](https://docs.python.org/3/library/index.html) 的一部分。这意味着在 Python 中使用 JSON 时无需安装额外的库。你可以像下面这样导入 `json`：

```python
import json
```

Python 内置的 `json` 库提供了完整的 API 来处理 JSON，尤其有两个主要函数：[`loads`](https://docs.python.org/3/library/json.html#json.loads) 和 [`load`](https://docs.python.org/3/library/json.html#json.load)。`loads` 函数用于从字符串解析 JSON 数据，而 `load` 函数用于将 JSON 数据解析为字节。

借助这两个方法，`json` 允许将 JSON 数据转换为 Python 等价对象（如 [字典](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) 和 [列表](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)），反之亦然。此外，`json` 模块还允许自定义编码器和解码器，以处理特定的数据类型。

## 使用 Python 解析 JSON 数据

## 将 JSON 字符串转换为 Python 字典

假设你有一些保存在字符串中的 JSON 数据，想要将其转换为 Python 字典。以下是 JSON 数据的示例：

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true
}
```

这是它在 Python 中的字符串表示：

```python
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
```

> **注意**  
> 如果有较长的多行 JSON 字符串，可以考虑使用 Python 的三引号语法来存储。

你可以使用下面的代码来验证 `smartphone` 变量是否包含一个有效的 Python 字符串：

```python
print(type(smartphone))
```

这将输出：

```
<class 'str'>
```

[`str`](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) 表示“字符串”，说明 `smartphone` 变量是文本序列类型。

使用 `json.loads()` 方法可以将字符串中的 JSON 数据解析为一个 Python 字典：

```python
import json

# JSON 字符串
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
# 从 JSON 字符串转为 Python 字典
smartphone_dict = json.loads(smartphone_json)

# 验证结果变量的类型
print(type(smartphone_dict)) # dict
```

如果你运行这段代码，会得到：

```
<class 'dict'>
```

现在 `smartphone_dict` 就是一个合法的 Python 字典了。

接下来，传入一个有效的 JSON 字符串给 `json.loads()`，即可将 JSON 字符串转换为 Python 字典。

现在，你可以像一般字典一样访问其中的字段：

```python
product = smartphone_dict['name'] # smartphone
priced = smartphone['price']      # 999.99
colors = smartphone['colors']     # ['black', 'white', 'red', 'blue']
```

需要注意的是，`json.loads()` 并不总是返回字典，具体取决于输入字符串的内容。例如，如果 JSON 字符串是一种简单类型，就会转换为相应的 Python 原生类型值：

```python
import json
 
json_string = '15.5'
float_var = json.loads(json_string)

print(type(float_var)) # <class 'float'>
```

同样，如果 JSON 字符串包含数组，那么它会转换为 Python 列表：

```python
import json
 
json_string = '[1, 2, 3]'
list_var = json.loads(json_string)
print(json_string) # <class 'list'>
```

以下是 [官方文档](https://docs.python.org/3/library/json.html#json-to-py-table) 中给出的 JSON 值到 Python 数据类型的转换表：

| **JSON Value**       | **Python Data** |
|----------------------|-----------------|
| `string`            | `str`           |
| `number (integer)`  | `int`           |
| `number (real)`     | `float`         |
| `true`              | `True`          |
| `false`             | `False`         |
| `null`              | `None`          |
| `array`             | `list`          |
| `object`            | `dict`          |

### 将 JSON API 响应转换为 Python 字典

假设你需要调用一个 API，并将返回的 JSON 响应转换为 Python 字典。下面的示例中，我们请求 [{JSON} Placeholder](https://jsonplaceholder.typicode.com/) 项目的以下 API 端点来获取一段示例 JSON：

```
https://jsonplaceholder.typicode.com/todos/1
```

这个 RESTFul API 返回如下 JSON 响应：

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

可以使用标准库的 [`urllib`](https://docs.python.org/3/library/urllib.html) 模块来调用该 API 并将结果转换为 Python 字典，示例如下：

```python
import urllib.request
import json

url = "https://jsonplaceholder.typicode.com/todos/1"

with urllib.request.urlopen(url) as response:
     body_json = response.read()

body_dict = json.loads(body_json)
user_id = body_dict['userId'] # 1
```

[`urllib.request.urlopen()`](https://docs.python.org/3/library/urllib.request.html#urllib.request.urlopen) 用于执行 API 调用，并返回一个 [`HTTPResponse`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse) 对象。它的 [`read()`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse.read) 方法将响应体读取为 `body_json`，然后我们使用 `json.loads()` 将该字符串解析为 Python 字典。

同样地，也可以使用 [`requests`](https://requests.readthedocs.io/en/latest/) 库来完成相同的任务：

```python
import requests
import json

url = "https://jsonplaceholder.typicode.com/todos/1"
response = requests.get(url)

body_dict = response.json()
user_id = body_dict['userId'] # 1
```

> **注意**  
> [`.json()`](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=json#json-response-content) 方法会自动将返回对象中的 JSON 数据转换为对应的 Python 数据结构。

### 将 JSON 文件加载到 Python 字典

假设你在 `smartphone.json` 文件中存有如下 JSON 数据：

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true,
  "dimensions": {
    "width": 2.82,
    "height": 5.78,
    "depth": 0.30
  },
  "features": [
    "5G",
    "HD display",
    "Dual camera"
  ]
}
```

你希望读取该 JSON 文件并将其加载为一个 Python 字典，可以使用如下示例代码：

```python
import json

with open('smartphone.json') as file:
  smartphone_dict = json.load(file)

print(type(smartphone_dict))  # <class 'dict'>
features = smartphone_dict['features']  # ['5G', 'HD display', 'Dual camera']
```

内置函数 [`open()`](https://docs.python.org/3/library/functions.html#open) 会打开文件并返回相应的 [file object](https://docs.python.org/3/glossary.html#term-file-object)。`json.load()` 会将包含 JSON 文档的 [文本文件](https://docs.python.org/3/glossary.html#term-text-file)或[二进制文件](https://docs.python.org/3/glossary.html#term-binary-file)反序列化为等效的 Python 对象。在本例中，`smartphone.json` 被转换为一个 Python 字典。

### 从 JSON 数据转换为自定义 Python 对象

现在，让我们将某些 JSON 数据解析为自定义的 Python 类。以下是自定义的 `Smartphone` Python 类：

```python
class Smartphone:
    def __init__(self, name, colors, price, in_stock):
        self.name = name    
        self.colors = colors
        self.price = price
        self.in_stock = in_stock
```

目标是将下面的 JSON 字符串转换为一个 `Smartphone` 实例：

```json
{
  "name": "iPear 23 Plus",
  "colors": ["black", "white", "gold"],
  "price": 1299.99,
  "inStock": false
}
```

接下来可以创建一个自定义的解码器来完成这项任务。为此，需要继承 [`JSONDecoder`](https://docs.python.org/3/library/json.html#json.JSONDecoder) 类，并在 `__init__` 方法中设置 `object_hook` 参数，使其指向包含自定义解析逻辑的类方法。在该解析方法中，你可以利用 `json.load()` 返回的标准字典中的值来实例化一个 `Smartphone` 对象。

定义自定义的 `SmartphoneDecoder`，如下所示：

```python
import json
 
class SmartphoneDecoder(json.JSONDecoder):
    def __init__(self, object_hook=None, *args, **kwargs):
        # 设置自定义 object_hook 方法
        super().__init__(object_hook=self.object_hook, *args, **kwargs)

    # 包含自定义解析逻辑的类方法
    def object_hook(self, json_dict):
        new_smartphone = Smartphone(
            json_dict.get('name'), 
            json_dict.get('colors'), 
            json_dict.get('price'),
            json_dict.get('inStock'),            
        )

        return new_smartphone
```

在自定义 `object_hook()` 方法中使用 `get()` 方法来读取字典值，这样可以避免当字典中缺少某个键时触发 `KeyError`，而只会返回 `None`。

现在调用 `json.loads()` 并将 `SmartphoneDecoder` 传给 `cls` 参数，即可实现从 JSON 字符串到 `Smartphone` 对象的转换：

```python
import json

# class Smartphone:
# ...

# class SmartphoneDecoder(json.JSONDecoder): 
# ...

smartphone_json = '{"name": "iPear 23 Plus", "colors": ["black", "white", "gold"], "price": 1299.99, "inStock": false}'

smartphone = json.loads(smartphone_json, cls=SmartphoneDecoder)
print(type(smartphone)) # <class '__main__.Smartphone'>
name = smartphone.name  # iPear 23 Plus
```

同样，你也可以在使用 `json.load()` 时，指定 `SmartphoneDecoder`：

```
smartphone = json.load(smartphone_json_file, cls=SmartphoneDecoder)
```

## 将 Python 数据转换成 JSON

同样，你也可以将 Python 数据结构或原生类型转换为 JSON。可以使用 [`json.dump()`](https://docs.python.org/3/library/json.html#json.dump) 和 [`json.dumps()`](https://docs.python.org/3/library/json.html#json.dumps) 两个函数来完成此操作，对应的 [转换表](https://docs.python.org/3/library/json.html#py-to-json-table) 如下：

| **Python Data** | **JSON Value**      |
|-----------------|---------------------|
| `str`           | `string`           |
| `int`           | `number (integer)` |
| `float`         | `number (real)`    |
| `True`          | `true`             |
| `False`         | `false`            |
| `None`          | `null`             |
| `list`          | `array`            |
| `dict`          | `object`           |
| `Null`          | None (无)          |

`json.dump()` 可以将 JSON 字符串写入文件，例如：

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

# 将该字典序列化为 JSON 并写入 user.json 文件
with open("user.json", "w") as json_file:
    json.dump(user_dict, json_file)
```

这段示例代码会将 Python 字典 `user_dict` 序列化写入到 `user.json` 文件中。

同样，`json.dumps()` 用于将 Python 变量转换成对应的 JSON 字符串：

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

user_json_string = json.dumps(user_dict)

print(user_json_string)
```

运行后输出：

```json
{"name": "John", "surname": "Williams", "age": 48, "city": "New York"}
```

> **注意**  
> 如果需要指定自定义编码器，可以参阅 [官方文档](https://docs.python.org/3/library/json.html#json.JSONEncoder)。

### 使用 `json` 标准模块的局限性

在进行 JSON [数据解析](https://brightdata.com/blog/web-data/what-is-data-parsing) 时，会遇到一些不容忽视的挑战。

常见的两个例子：

- 当 JSON 非法、损坏或不遵循标准时，Python `json` 模块会处理不当。  
- 从不可信来源解析 JSON 数据是危险的，因为恶意的 JSON 字符串可能会导致解析器崩溃或消耗大量资源。

虽然可以在一定程度上应对这些问题，不过在商务或实际生产环境中，使用能简化 JSON 解析的商业化工具往往更可取，比如 [Web Scraper API](https://brightdata.com/products/web-scraper)。

## 结论

当你在 Python 中使用标准模块 `json` 以原生方式解析 JSON 数据时，往往需要可靠的代理服务器来绕过网站可能设置的访问限制。可以尝试使用诸如 Bright Data 的数据和 [代理产品](https://www.bright.cn/proxy-types) 等先进且功能全面的商业化数据采集方案，来进行数据解析。
