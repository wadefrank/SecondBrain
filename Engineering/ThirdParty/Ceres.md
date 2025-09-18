
# Ceres-1.14.0+Ubuntu20.04

Github: [https://github.com/ceres-solver/ceres-solver/tree/1.14.0](https://github.com/ceres-solver/ceres-solver/tree/1.14.0)

## 下载

```shell
git clone -b 1.14.0 <https://github.com/ceres-solver/ceres-solver.git> ceres-solver-1.14.0
```

## 安装依赖库

```shell
# CMake
sudo apt-get install cmake
# google-glog + gflags
sudo apt-get install libgoogle-glog-dev libgflags-dev
# Use ATLAS for BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3
sudo apt-get install libeigen3-dev
# SuiteSparse (optional)
sudo apt-get install libsuitesparse-dev
```

## 安装

```shell
cd ~/packages/ceres-solver
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/ceres-solver/install ..
make -j30
make test
make install
```

## 调用

CMakeLists.txt

```cmake
# Ceres
set(Ceres_DIR "/home/qxit-ln-002408/packages/ceres-solver/install/lib/cmake/Ceres")
# find_package(Ceres REQUIRED NO_MODULE NO_DEFAULT_PATH)
find_package(Ceres REQUIRED)
target_link_libraries(${PROJECT_NAME} ${CERES_LIBRARIES})
```

# Ceres-2.0.0+Ubuntu20.04

## 下载


## 安装依赖库

```shell
# CMake
sudo apt-get install cmake
# google-glog + gflags
sudo apt-get install libgoogle-glog-dev libgflags-dev
# Use ATLAS for BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3
sudo apt-get install libeigen3-dev
# SuiteSparse (optional)
sudo apt-get install libsuitesparse-dev
```

## 安装

```shell
cd ~/packages/ceres-solver
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/ceres-solver/install ..
make -j30
make test
make install
```

## 调用

CMakeLists.txt

```cmake
# Ceres
set(Ceres_DIR "/home/qxit-ln-002408/packages/ceres-solver/install/lib/cmake/Ceres")
# find_package(Ceres REQUIRED NO_MODULE NO_DEFAULT_PATH)
find_package(Ceres REQUIRED)
target_link_libraries(${PROJECT_NAME} ${CERES_LIBRARIES})
```