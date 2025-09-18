# libjpeg-turbo+Ubuntu20.04

## 下载

链接：[https://github.com/libjpeg-turbo/libjpeg-turbo/releases/tag/3.0.1](https://github.com/libjpeg-turbo/libjpeg-turbo/releases/tag/3.0.1)

## 安装

```
cd ~/packages
uzip libjpeg-turbo-3.0.1.zip
mv libjpeg-turbo-3.0.1 libjpeg-turbo
cd libjpeg-turbo
sudo apt install nasm
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/libjpeg-turbo/install ..
make
make install
make test
make testclean
```