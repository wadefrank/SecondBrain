# pcl-1.11.1+Ubuntu20.04

## 安装依赖库

```shell
sudo apt-get update
sudo apt-get install git build-essential linux-libc-dev
sudo apt-get install cmake cmake-gui
sudo apt-get install libusb-1.0-0-dev libusb-dev libudev-dev
sudo apt-get install mpi-default-dev openmpi-bin openmpi-common
sudo apt-get install libflann1.9 libflann-dev
# sudo apt-get install libeigen3-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libvtk7*
sudo apt-get install libqhull* libgtest-dev
sudo apt-get install freeglut3-dev pkg-config
sudo apt-get install libxmu-dev libxi-dev
sudo apt-get install mono-complete
sudo apt-get install openjdk-8-jdk openjdk-8-jre
```

## 修改CMakeLists.txt

修改CMakeLists.txt，使用自定义目录下的Eigen（而非系统目录/usr/local/include下）

```shell
cd ~/packages/pcl-pcl-1.11.1
vim ./CMakeLists.txt
```

将297行的代码：

```cmake
# Eigen (required)
find_package(Eigen 3.1 REQUIRED)
include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS})
```

修改为：

```cmake
# Eigen (required)
# find_package(Eigen 3.1 REQUIRED)
# include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS})
include_directories(SYSTEM "/home/qxit-ln-002408/packages/eigen-3.3.9/install/include/eigen3")
```

## 编译安装

```shell
cd ~/packages/pcl-pcl-1.11.1
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/pcl-pcl-1.11.1/install ..
make
make install
```