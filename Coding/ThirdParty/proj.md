# proj-6.0.0+Ubuntu20.04

## 安装依赖库

```shell
sudo apt install sqlite3 libsqlite3-dev

```

## 编译安装

```shell
cd ~/packages/proj-6.0.0
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/proj-6.0.0/install ..
make
make install
```