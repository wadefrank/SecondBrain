# Eigen-3.3.9+Ubuntu20.04

## 下载

Eigen 3.3.9安装包下载: [https://gitlab.com/libeigen/eigen/-/releases/3.3.9](https://gitlab.com/libeigen/eigen/-/releases/3.3.9)

## 安装

```shell
# 安装依赖项
sudo apt install cmake
sudo apt install g++

cd eigen-3.3.9
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/wade/third_party/eigen-3.3.9/install ..
make -j4
make install
```

## 调用

调用Eigen库的CMakeLists.txt需要改成：

```cmake
// set(EIGEN_INCLUDE_DIR "/home/wade/third_party/eigen-3.3.9/install/include/eigen3")
// set(EIGEN3_INCLUDE_DIR "/home/wade/third_party/eigen-3.3.9/install/include/eigen3")
set(Eigen3_DIR   	        "/home/qxit-ln-002408/packages/eigen-3.3.9/install/share/eigen3/cmake")

find_package(Eigen3 3.3.9 REQUIRED)

include_directories(${EIGEN3_INCLUDE_DIR})

message(STATUS "Eigen3 version: ${Eigen_VERSION}")
message(STATUS "Eigen3 include dir: ${EIGEN3_INCLUDE_DIR}")
```