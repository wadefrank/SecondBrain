# Pangolin-0.6+Ubuntu20.04

## 下载

```shell
# git clone 代码
git clone <https://github.com/stevenlovegrove/Pangolin.git> Pangolin-0.6
```

## 安装

```shell
# 安装依赖项
sudo apt install libgl1-mesa-dev libglew-dev pkg-config libegl1-mesa-dev libwayland-dev libxkbcommon-dev wayland-protocols

# 安装Pangolin
cd Pangolin-0.6
git checkout v0.6
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/wade/third_party/Pangolin-0.6/install ..
make -j4
make install
```

## 调用

```cmake
set(Pangolin_DIR "/home/wade/third_party/Pangolin-0.6/install/lib/cmake/Pangolin")

find_package(Pangolin 0.6 REQUIRED)  // find_package(Pangolin 0.2 QUIET)

include_directories( ${Pangolin_INCLUDE_DIRS} ) 

message(STATUS "Pangolin version: ${Pangolin_VERSION}")
message(STATUS "Pangolin include dirs: ${Pangolin_INCLUDE_DIRS}")
```