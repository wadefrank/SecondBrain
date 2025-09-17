## wiringPi+Ubuntu20.04

```shell
# 目前运行稳定的wiringPi库按照如下方法安装：
sudo apt-get install wiringpi

# 安装之后需要将oss中（oss://recap/computer_vision/library/lidar_visiual_collection/wiringPi头文件/）
# 对应头文件拷贝至默认路径/usr/include

#此外，相关代码一般还需要安装libgpiod-dev
sudo apt-get install libgpiod-dev
```