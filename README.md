# 使用 Python 解析 JSON 数据

[![推广](https://github.com/bright-cn/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/) 

本指南介绍如何使用 Python 中的 `json` 模块解析 JSON 数据、将其转换为 Python 字典，以及执行反向转换。

- [Python 中的 JSON 简介](#python-中的-json-简介)
- [使用 Python 解析 JSON 数据](#使用-python-解析-json-数据-1)
- [将 JSON 字符串转换为 Python 字典](#将-json-字符串转换为-python-字典)
  - [将 JSON API 响应转换为 Python 字典](#将-json-api-响应转换为-python-字典)
  - [将 JSON 文件加载到 Python 字典中](#将-json-文件加载到-python-字典中)
  - [从 JSON 数据到自定义 Python 对象](#从-json-数据到自定义-python-对象)
- [将 Python 数据转换为 JSON](#将-python-数据转换为-json)
- [`json` 标准模块的局限性](#json-标准模块的局限性)
- [总结](#总结)

## Python 中的 JSON 简介

JavaScript 对象表示法（JSON）是一种轻量级的数据交换格式，通常用于通过 API 在服务器和 Web 应用程序之间传输数据。JSON 数据由键值对组成，其中每个键都是字符串，而每个值可以是字符串、数字、布尔值、null、数组或对象。

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

Python 通过 `json` 模块原生支持 [JSON](https://docs.python.org/3/library/json.html)，该模块是 [Python 标准库](https://docs.python.org/3/library/index.html)的一部分。这意味着你无需安装任何其他库，即可在 Python 中处理 JSON。你可以按如下方式导入 `json`：

```python
import json
```

Python 内置的 `json` 库提供了一套用于处理 JSON 的完整 API。具体来说，它包含两个关键函数：[`loads`](https://docs.python.org/3/library/json.html#json.loads) 和 [`load`](https://docs.python.org/3/library/json.html#json.load)。`loads` 函数用于解析字符串中的 JSON 数据，而 `load` 函数用于将 JSON 数据解析为字节。

通过这两种方法，`json` 可让你在 JSON 数据与等效的 Python 对象（例如[字典](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)和[列表](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)）之间进行双向转换。此外，`json` 模块还允许你创建自定义编码器和解码器，以处理特定的数据类型。

## 使用 Python 解析 JSON 数据

## 将 JSON 字符串转换为 Python 字典

假设你有一些存储在字符串中的 JSON 数据，并希望将其转换为 Python 字典。JSON 数据如下所示：

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true
}
```

以下是它在 Python 中的字符串表示形式：

```python
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
```

> **注意**\
> 对于较长的多行 JSON 字符串，可以考虑使用 Python 的三引号约定进行存储。

你可以使用以下代码验证 `smartphone` 是否包含有效的 Python 字符串：

```python
print(type(smartphone))
```

这将输出：

```
<class 'str'>
```

[`str`](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) 表示“字符串”，意味着 smartphone 变量属于文本序列类型。

使用 json.loads() 方法，将 smartphone 中包含的 JSON 字符串解析为 Python 字典，如下所示：

```python
import json

# JSON string
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
# from JSON string to Python dict
smartphone_dict = json.loads(smartphone_json)

# verify the type of the resulting variable
print(type(smartphone_dict)) # dict
```

运行此代码片段后，你将得到：

```
<class 'dict'>
```

现在，`smartphone_dict` 包含一个有效的 Python 字典。

接下来，将有效的 JSON 字符串传递给 `json.loads()`，以将其转换为 Python 字典。

现在，你可以像往常一样访问生成的字典字段：

```python
product = smartphone_dict['name'] # smartphone
priced = smartphone['price'] # 999.99
colors = smartphone['colors'] # ['black', 'white', 'red', 'blue']
```

`json.loads()` 函数并不总是返回字典。具体而言，返回的数据类型取决于输入字符串。例如，如果 JSON 字符串包含一个简单值，它将被转换为等效的 Python 原始值：

```python
import json
 
json_string = '15.5'
float_var = json.loads(json_string)

print(type(float_var)) # <class 'float'>
```

同样，包含数组的 JSON 字符串将变为 Python 列表：

```python
import json
 
json_string = '[1, 2, 3]'
list_var = json.loads(json_string)
print(json_string) # <class 'list'>
```

下方的[转换表](https://docs.python.org/3/library/json.html#json-to-py-table)说明了 `json` 如何将 JSON 值转换为 Python 数据：

| **JSON 值** | **Python 数据** |
| - | - | - |
| `string` | `str` |
| `number (integer)` | `int` |
| `number (real)` | `float` |
| `true` | `True` |
| `false` | `False` |
| `null` | `None` |
| `array` | `list` |
| `object` | `dict` |

### 将 JSON API 响应转换为 Python 字典

假设你需要发起 API 请求，并将其 JSON 响应转换为 Python 字典。在下面的示例中，我们将调用 [{JSON} Placeholder](https://jsonplaceholder.typicode.com/) 项目的以下 API 端点，以获取一些模拟 JSON 数据：

```
https://jsonplaceholder.typicode.com/todos/1
```

该 RESTful API 返回以下 JSON 响应：

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

你可以使用标准库中的 [`urllib`](https://docs.python.org/3/library/urllib.html) 模块调用该 API，并按如下方式将得到的 JSON 转换为 Python 字典：

```python
import urllib.request
import json

url = "https://jsonplaceholder.typicode.com/todos/1"

with urllib.request.urlopen(url) as response:
     body_json = response.read()

body_dict = json.loads(body_json)
user_id = body_dict['userId'] # 1
```

[`urllib.request.urlopen()`](https://docs.python.org/3/library/urllib.request.html#urllib.request.urlopen) 执行 API 调用并返回一个 [`HTTPResponse`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse) 对象。然后使用其 [`read()`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse.read) 方法获取响应正文 body\_json，其中包含字符串形式的 API JSON 响应。最后，如前文所述，可以通过 `json.loads()` 将该字符串解析为 Python 字典。

同样，你也可以使用 [`requests:`](https://requests.readthedocs.io/en/latest/) 获得相同结果。

```python
import requests
import json

url = "https://jsonplaceholder.typicode.com/todos/1"
response = requests.get(url)

body_dict = response.json()
user_id = body_dict['userId'] # 1
```

> **注意**\
> [`.json()`](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=json#json-response-content) 方法会自动将包含 JSON 数据的响应对象转换为相应的 Python 数据结构。

### 将 JSON 文件加载到 Python 字典中

假设你有一些 JSON 数据存储在 `smartphone.json` 文件中，如下所示：

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

你的目标是读取该 JSON 文件，并将其加载到 Python 字典中。可以使用以下代码片段实现：

```python
import json

with open('smartphone.json') as file:
  smartphone_dict = json.load(file)

print(type(smartphone_dict)) # <class 'dict'>
features = smartphone_dict['features'] # ['5G', 'HD display', 'Dual camera']
```

内置的 [`open()`](https://docs.python.org/3/library/functions.html#open) 库允许你加载文件并获取相应的[文件对象](https://docs.python.org/3/glossary.html#term-file-object)。随后，`json.read()` 方法会将包含 JSON 文档的[文本文件](https://docs.python.org/3/glossary.html#term-text-file)或[二进制文件](https://docs.python.org/3/glossary.html#term-binary-file)反序列化为等效的 Python 对象。在本例中，`smartphone.json` 会变为 Python 字典。

### 从 JSON 数据到自定义 Python 对象

现在，让我们将一些 JSON 数据解析为自定义 Python 类。你的自定义 `Smartphone` Python 类如下所示：

```python
class Smartphone:
    def __init__(self, name, colors, price, in_stock):
        self.name = name    
        self.colors = colors
        self.price = price
        self.in_stock = in_stock
```

这里的目标是将以下 JSON 字符串转换为 `Smartphone` 实例：

```json
{
  "name": "iPear 23 Plus",
  "colors": ["black", "white", "gold"],
  "price": 1299.99,
  "inStock": false
}
```

创建自定义解码器来完成此任务。为此，请扩展 [`JSONDecoder`](https://docs.python.org/3/library/json.html#json.JSONDecoder) 类，并在 `__init__` 方法中设置 `object_hook` 参数。将包含自定义解析逻辑的类方法名称赋给该参数。在此解析方法中，你可以使用 `json.read()` 返回的标准字典中包含的值来实例化 `Smartphone` 对象。

按如下方式定义自定义 `SmartphoneDecoder`：

```python
import json
 
class SmartphoneDecoder(json.JSONDecoder):
    def __init__(self, object_hook=None, *args, **kwargs):
        # set the custom object_hook method
        super().__init__(object_hook=self.object_hook, *args, **kwargs)

    # class method containing the 
    # custom parsing logic
    def object_hook(self, json_dict):
        new_smartphone = Smartphone(
            json_dict.get('name'), 
            json_dict.get('colors'), 
            json_dict.get('price'),
            json_dict.get('inStock'),            
        )

        return new_smartphone
```

在自定义 `object_hook()` 方法中，使用 `get()` 方法读取字典值。这样可以确保字典中缺少某个键时不会引发 `KeyError`。此时将返回 `None` 值。

现在，将 `SmartphoneDecoder` 类传递给 `json.loads()` 中的 `cls` 参数，以将 JSON 字符串转换为 `Smartphone` 对象：

```python
import json

# class Smartphone:
# ...

# class SmartphoneDecoder(json.JSONDecoder): 
# ...

smartphone_json = '{"name": "iPear 23 Plus", "colors": ["black", "white", "gold"], "price": 1299.99, "inStock": false}'

smartphone = json.loads(smartphone_json, cls=SmartphoneDecoder)
print(type(smartphone)) # <class '__main__.Smartphone'>
name = smartphone.name # iPear 23 Plus
```

同样，你也可以将 `SmartphoneDecoder` 与 `json.load()` 搭配使用：

```
smartphone = json.load(smartphone_json_file, cls=SmartphoneDecoder)
```

## 将 Python 数据转换为 JSON

你也可以执行反向操作，将 Python 数据结构和原始值转换为 JSON。这可以通过 [`json.dump()`](https://docs.python.org/3/library/json.html#json.dump) 和 [`json.dumps()`](https://docs.python.org/3/library/json.html#json.dumps) 函数实现，它们遵循下方的[转换表](https://docs.python.org/3/library/json.html#py-to-json-table)：

| **Python 数据** | **JSON 值** |
| - | - | - |
| `str` | `string` |
| `int` | `number (integer)` |
| `float` | `number (real)` |
| `True` | `true` |
| `False` | `false` |
| `None` | `null` |
| `list` | `array` |
| `dict` | `object` |
| `Null` | 无  |

`json.dump()` 允许你将 JSON 字符串写入文件，如以下示例所示：

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

# serializing the sample dictionary to a JSON file
with open("user.json", "w") as json_file:
    json.dump(user_dict, json_file)
```

此代码片段会将 Python 变量 `user_dict` 序列化到 `user.json` 文件中。

同样，`json.dumps()` 会将 Python 变量转换为等效的 JSON 字符串：

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
运行此代码片段后，你将得到：

```json
{"name": "John", "surname": "Williams", "age": 48, "city": "New York"}
```

> **注意**\
> 请参阅[官方文档](https://docs.python.org/3/library/json.html#json.JSONEncoder)，了解如何指定自定义编码器。

### `json` 标准模块的局限性

JSON [数据解析](https://www.bright.cn/blog/web-data/what-is-data-parsing)面临一些不容忽视的挑战。

两个常见示例包括：

- 面对无效、损坏或非标准的 JSON 时，Python 的 `json` 模块将无法胜任。
- 解析来自不可信来源的 JSON 数据十分危险，因为恶意 JSON 字符串可能导致解析器崩溃或消耗大量资源。

这些局限性虽有解决方法，但最好使用能简化 JSON 解析的商业工具，例如[网页爬虫工具 API](https://www.bright.cn/products/web-scraper)。

## 总结

在 Python 中通过 `json` 标准模块原生解析 JSON 数据时，你需要可靠的代理服务器来绕过网站施加的限制。你可以试用领先、功能完善的商业数据解析解决方案，例如 Bright Data 的数据和[代理产品](https://www.bright.cn/proxy-types)。
