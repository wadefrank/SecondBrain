# serial+Ubuntu20.04

## 编译安装

```shell
cd ~/packages/serial
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/serial/install/ ..
make
make install
```