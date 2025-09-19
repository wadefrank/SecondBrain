
# 简介

python 3.8

[https://docs.python.org/3.8/index.html](https://docs.python.org/3.8/index.html)

The Python Standard Library(python 3.8)

[https://docs.python.org/3.8/library/index.html](https://docs.python.org/3.8/library/index.html)

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

# Standard Library

## Built-in Functions

### getattr(_object_, _name_[, _default_])

Return the value of the named attribute of _object_.

### isinstance(_object_, _classinfo_)

Return `True` if the _object_ argument is an instance of the _classinfo_ argument, or of a (direct, indirect or [virtual](https://docs.python.org/3.8/glossary.html#term-abstract-base-class)) subclass thereof.

### reversed(_seq_)

Return a reverse iterator.

返回一个序列的逆序迭代器。

### sorted(_iterable_, ***, _key=None_, _reverse=False_)

Return a new sorted list from the items in _iterable_.

从iterable中返回一个新的排序之后的列表。

### super([_type_[, _object-or-type_]])

Return a proxy object that delegates method calls to a parent or sibling class of _type_. This is useful for accessing inherited methods that have been overridden in a class.

返回一个代理对象，该对象将方法调用委托给该类型的父类或同级类。 这对于访问类中已重写的继承方法很有用。

### @staticmethod

Transform a method into a static method.

将一个方法转换成一个静态方法。

Python3有三种方法：

**静态方法**: 用 @staticmethod 装饰的不带 self 参数的方法叫做静态方法，类的静态方法可以没有参数，可以直接使用类名调用。

**普通方法**: 默认有个self参数，且只能被对象调用。

**类方法**: 默认有个 cls 参数，可以被类和对象调用，需要加上 @classmethod 装饰器。

```python
class Classname:
    @staticmethod
    def fun():
        print('静态方法')

    @classmethod
    def a(cls):
        print('类方法')

    # 普通方法
    def b(self):
        print('普通方法')

Classname.fun()
Classname.a()

C = Classname()
C.fun()
C.a()
C.b()
```

## Built-in Types

[https://docs.python.org/3.8/library/stdtypes.html#](https://docs.python.org/3.8/library/stdtypes.html#)

### Sequence Types

[https://docs.python.org/3.8/library/stdtypes.html#sequence-types-list-tuple-range](https://docs.python.org/3.8/library/stdtypes.html#sequence-types-list-tuple-range)

- **Mutable Sequence**: 可变序列，可变序列是指可以在原内存地址上对数据进行修改的序列。这类序列包括列表`list`、字节数组`bytearray`、数组`array.array`，内存视图`memoryview`等。
- **Immutable Sequence:** 不可变序列指的是不可以在原内存地址上对序列进行修改。这类序列包括字符序列`str`、元祖`tuple`、和字节序列`bytes`。

#### Mutable Sequence Operations

|Operation|Result|
|---|---|
|`s[i] = x`|item _i_ of _s_ is replaced by _x_|
|`s[i:j] = t`|slice of _s_ from _i_ to _j_ is replaced by the contents of the iterable _t_|
|`del s[i:j]`|same as `s[i:j] = []`|
|`s[i:j:k] = t`|the elements of `s[i:j:k]` are replaced by those of _t_|
|`del s[i:j:k]`|removes the elements of `s[i:j:k]` from the list|
|`s.append(x)`|appends _x_ to the end of the sequence (same as `s[len(s):len(s)] = [x]`)|
|`s.clear()`|removes all items from _s_ (same as `del s[:]`)|
|`s.copy()`|creates a shallow copy of _s_ (same as `s[:]`)|
|`s.extend(t)` or `s += t`|extends _s_ with the contents of _t_ (for the most part the same as `s[len(s):len(s)] = t`)|
|`s *= n`|updates _s_ with its contents repeated _n_ times|
|`s.insert(i, x)`|inserts _x_ into _s_ at the index given by _i_ (same as `s[i:i] = [x]`)|
|`s.pop()` or `s.pop(i)`|retrieves the item at _i_ and also removes it from _s_|
|`s.remove(x)`|remove the first item from _s_ where `s[i]` is equal to _x_|
|`s.reverse()`|reverses the items of _s_ in place|

## str—Text Sequence Type

### endswith(_suffix_[, _start_[, _end_]])

判断一个字符串是否以suffix作为结尾。

## argparse

[https://docs.python.org/3.8/library/argparse.html](https://docs.python.org/3.8/library/argparse.html)

命令行参数解析器模块

常用函数

```python
# 新建一个ArgumentParser类
parser = argparse.ArgumentParser(
                    prog='ProgramName',
                    description='What the program does',
                    epilog='Text at the bottom of help')

# positional argument
parser.add_argument('filename')

# optinal argument
parser.add_argument('--foo', help='foo help')

args = parser.parse_args()
```

## os

[https://docs.python.org/3.8/library/os.html](https://docs.python.org/3.8/library/os.html)

### path

#### exists()

判断是否路径存在。

用法：

```python
if not os.path.exists(fpath):
    raise FileNotFoundError(fpath)
```

#### join()

智能的将多个路径组件加起来。

用法：

```python
path_1 = "/home/wade/"
path_2 = "Code/"
path_3 = "thirdparty/"
path_4 = "123.txt"

path = os.path.join(path_1, path_2, path_3, path_4)
```

## typing

类型注解模块：支持类型检查

[https://docs.python.org/3.8/library/typing.html](https://docs.python.org/3.8/library/typing.html)

```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```

### Union

Union type; `Union[X, Y]` means either X or Y.


# Tutorial

## 3 Data model

### 3.3 Special method names

object.init(_self_[, _..._])

## 4 More Control Flow Tools

### 4.6 Defining Functions

### 4.7. More on Defining Functions

#### 4.7.2 Keyword Arguments

```python
def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw, ":", keywords[kw])
```

- `*` 接收tuple
- `**` 接收dict
- 如果同时使用 * 和 ** 时， * 必须要在 ** 之前

#### 4.7.3 Special parameters

[https://docs.python.org/3.8/tutorial/controlflow.html#special-parameters](https://docs.python.org/3.8/tutorial/controlflow.html#special-parameters)

```python
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
      -----------    ----------     ----------
        |             |                  |
        |        Positional or keyword   |
        |                                - Keyword only
         -- Positional only
```

- Positional-only parameters are placed before a `/`
- Keyword-only parameters are placed after a `*`