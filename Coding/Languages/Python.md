
# 简介

python 3.8

[https://docs.python.org/3.8/index.html](https://docs.python.org/3.8/index.html)

# 数据结构

## 字典（dictionary）

```python
# 创建一个空字典
my_dict = {}

# 创建一个具有初始值的字典
my_dict = {'name': 'Alice', 'age': 25, 'city': 'New York'}

# 访问字典中的值
print(my_dict['name'])  # 输出: Alice
print(my_dict['age'])   # 输出: 25

# 修改字典中的值
my_dict['age'] = 26
print(my_dict['age'])   # 输出: 26

# 添加新键值对
my_dict['email'] = 'alice@example.com'
print(my_dict)
# 输出: {'name': 'Alice', 'age': 26, 'city': 'New York', 'email': 'alice@example.com'}

# 删除键值对
del my_dict['city']
print(my_dict)
# 输出: {'name': 'Alice', 'age': 26, 'email': 'alice@example.com'}
```

### items()

返回一个包含所有键值对的视图对象。

```python
for key, value in my_dict.items():
    print(f"{key}: {value}")
# 输出:
# name: Alice
# age: 26
# email: alice@example.com
```

### keys()

返回一个包含所有键的视图对象。

```python
keys = my_dict.keys()
print(keys)  # 输出: dict_keys(['name', 'age', 'email'])
```

### values()

返回一个包含所有值的视图对象。

```python
values = my_dict.values()
print(values)  # 输出: dict_values(['Alice', 26, 'alice@example.com'])
```

### get()

返回指定键的值，如果键不存在则返回默认值。

```python
print(my_dict.get('name'))      # 输出: Alice
print(my_dict.get('city', 'Not Found'))  # 输出: Not Found
```

### pop()

删除指定键并返回对应的值。

```python
age = my_dict.pop('age')
print(age)       # 输出: 26
print(my_dict)   # 输出: {'name': 'Alice', 'email': 'alice@example.com'}
```

