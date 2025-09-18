# 环境配置（Ubuntu20.04）

## 下载

```shell
git clone https://github.com/JakobEngel/dso.git
```

## 安装依赖项

```shell
# Required Dependencies
sudo apt-get install libsuitesparse-dev libeigen3-dev libboost-all-dev

# Optional Dependencies
# OpenCV
sudo apt-get install libopencv-dev

# Pangolin

# ziplib
sudo apt-get install zlib1g-dev
cd dso/thirdparty
tar -zxvf libzip-1.1.1.tar.gz
cd libzip-1.1.1/
./configure
make
sudo make install
sudo cp lib/zipconf.h /usr/local/include/zipconf.h   # (no idea why that is needed).
```

## 安装DSO

打开dso/CMakeLists.txt

```cmake
// 在15行处添加以下代码：
set(EIGEN3_INCLUDE_DIR "/home/wade/third_party/eigen-3.3.9/install/include/eigen3")
set(OpenCV_DIR "/home/wade/third_party/opencv-3.3.1/install/share/OpenCV")
set(Pangolin_DIR "/home/wade/third_party/Pangolin-0.6/install/lib/cmake/Pangolin")

// 替换以下代码
find_package(Eigen3 3.3.9 REQUIRED)  // find_package(Eigen3 REQUIRED)
find_package(OpenCV 3.3.1 REQUIRED)  // find_package(OpenCV QUIET)
find_package(Pangolin 0.6 REQUIRED)  // find_package(Pangolin 0.2 QUIET)

// 打印三方库版本和路径（可选）
message(STATUS "Eigen3 version: ${Eigen_VERSION}")
message(STATUS "Eigen3 include dir: ${EIGEN3_INCLUDE_DIR}")
message(STATUS "OpenCV version: ${OpenCV_VERSION}")
message(STATUS "OpenCV include dirs: ${OpenCV_INCLUDE_DIRS}")
message(STATUS "Pangolin version: ${Pangolin_VERSION}")
message(STATUS "Pangolin include dirs: ${Pangolin_INCLUDE_DIRS}")
```

编译

```shell
cd dso
mkdir build
cd build
cmake ..
make -j4
```

## 调试

创建.vscode/launch.json（点击create a launch.json file，选择CMake Debug）

```json
{
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Debug DSO",
        "type": "cppdbg",
        "request": "launch",
        "program": "${workspaceFolder}/build/bin/dso_dataset",  // 可执行文件路径
        "args": [
          "files=${workspaceFolder}/data/sequence_25/images.zip",       // 数据集路径
          "calib=${workspaceFolder}/data/sequence_25/camera.txt",       // 相机内参文件路径
          "gamma=${workspaceFolder}/data/sequence_25/pcalib.txt",       // 相机光度参数文件路径(gamma)
          "vignette=${workspaceFolder}/data/sequence_25/vignette.png",  // 相机光度参数文件路径(vignette)
          "mode=0",                                                     // 是否使用标定好的光度参数(0-有光度标定参数，1-没有光度标定参数，2-没有光度标定)
          "preset=0"                                                    // 是否实时运行(0-不强制实时运行（2k pts），1-强制实时运行（2k pts），2-不强制实时运行（800pts），3-强制5x实时运行（800pts）)
        ],
        "stopAtEntry": false,
        "cwd": "${workspaceFolder}",
        "environment": [],
        "externalConsole": false,
        "MIMode": "gdb",
        "setupCommands": [
          {
            "description": "Enable pretty-printing for gdb",
            "text": "-enable-pretty-printing",
            "ignoreFailures": true
          }
        ],
        // "preLaunchTask": "build-dso"  // 调试前自动编译
      }
    ]
}
```