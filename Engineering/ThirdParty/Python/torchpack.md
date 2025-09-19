# Intro

Torchpack is a neural network training interface based on PyTorch, with a focus on flexibility.

homepage

[https://torchpack.readthedocs.io/en/latest/](https://torchpack.readthedocs.io/en/latest/)

github

[https://github.com/zhijian-liu/torchpack](https://github.com/zhijian-liu/torchpack)

Module Index

[https://torchpack.readthedocs.io/en/latest/py-modindex.html](https://torchpack.readthedocs.io/en/latest/py-modindex.html)

# Installation

```python
pip install torchpack
```

# Module

## distributed

## utils

### config

Source code

[https://torchpack.readthedocs.io/en/latest/_modules/torchpack/utils/config.html#Config](https://torchpack.readthedocs.io/en/latest/_modules/torchpack/utils/config.html#Config)

**load()**

```python
import os
from typing import Any, Dict, List, Tuple, Union
from torchpack.utils import io

    def load(self, fpath: str, *, recursive: bool = False) -> None:
        if not os.path.exists(fpath):
            raise FileNotFoundError(fpath)
        fpaths = [fpath]
        if recursive:
            while fpath:
                fpath = os.path.dirname(fpath)
                for fname in ['default.yaml', 'default.yml']:
                    fpaths.append(os.path.join(fpath, fname))
        for fpath in reversed(fpaths):
            if os.path.exists(fpath):
                self.update(io.load(fpath))
```

参数中有*，参考4.7.3 Special parameters

[**Tutorial**](https://www.notion.so/Tutorial-b3765f667ff44471aababbbdf1b47b79?pvs=21)

### update()

update会覆盖掉之前的值。

```python
from typing import Any, Dict, List, Tuple, Union
from multimethod import multimethod

    @multimethod
    def update(self, other: Dict) -> None:
        for key, value in other.items():
            if isinstance(value, dict):
                if key not in self or not isinstance(self[key], Config):
                    self[key] = Config()
                self[key].update(value)
            else:
                self[key] = value
```

## io

### load()

.yaml返回值：dict

```python
import yaml

def load_yaml(f: Union[str, TextIO], **kwargs) -> Any:
    with file_descriptor(f, mode='r') as fd:
        return yaml.safe_load(fd, **kwargs)

# yapf: disable
__io_registry = {
    '.json': {'load': load_json, 'save': save_json},
    '.jsonl': {'load': load_jsonl, 'save': save_jsonl},
    '.mat': {'load': load_mat, 'save': save_mat},
    '.npy': {'load': load_npy, 'save': save_npy},
    '.npz': {'load': load_npz, 'save': save_npz},
    '.pkl': {'load': load_pkl, 'save': save_pkl},
    '.pt': {'load': load_pt, 'save': save_pt},
    '.pth': {'load': load_pt, 'save': save_pt},
    '.pth.tar': {'load': load_pt, 'save': save_pt},
    '.yml': {'load': load_yaml, 'save': save_yaml},
    '.yaml': {'load': load_yaml, 'save': save_yaml},
}
# yapf: enable

def load(fpath: str, **kwargs) -> Any:
    assert isinstance(fpath, str), type(fpath)

    for extension in sorted(__io_registry.keys(), key=len, reverse=True):
        if fpath.endswith(extension) and 'load' in __io_registry[extension]:
            return __io_registry[extension]['load'](fpath, **kwargs)

    raise NotImplementedError(f'"{fpath}" cannot be loaded.')
```