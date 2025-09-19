homepage:

[https://pyyaml.org/wiki/PyYAMLDocumentation](https://pyyaml.org/wiki/PyYAMLDocumentation)

installation

```shell
pip install pyyaml
```

# module

## load()

从yaml文件中读取数据，返回一个dict

## safe_load()

类似yaml.load()，更安全，同样返回一个dict

用法：

```python
f = open("/home/wade/Code/thirdparty/123.yaml")
data = yaml.safe_load(f)
```