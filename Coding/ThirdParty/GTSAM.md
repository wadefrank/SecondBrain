# GTSAM-4.0.3+Ubuntu20.04

[https://github.com/borglab/gtsam](https://github.com/borglab/gtsam)

## 安装依赖库

Intel Threaded Building Blocks (TBB)

```shell
sudo apt-get install libtbb-dev
```

Intel Math Kernel Library (MKL)
[https://www.intel.com/content/www/us/en/developer/articles/guide/installing-free-libraries-and-python-apt-repo.html](https://www.intel.com/content/www/us/en/developer/articles/guide/installing-free-libraries-and-python-apt-repo.html)

## 修改CMakeLists.txt

修改CMakeLists.txt，使用自定义目录下的Eigen（而非系统目录/usr/local/include下）

```shell
cd ~/packages/gtsam-4.0.3
vim ./CMakeLists.txt
```

在281行处下方添加代码。

添加之前：

```cmake
# Switch for using system Eigen or GTSAM-bundled Eigen
```

添加之后的代码为：

```cmake
# Switch for using system Eigen or GTSAM-bundled Eigen
set(GTSAM_USE_SYSTEM_EIGEN OFF)
```

将309处的代码：

```cmake
	# set full path to be used by external projects
	# this will be added to GTSAM_INCLUDE_DIR by gtsam_extra.cmake.in
	set(GTSAM_EIGEN_INCLUDE_FOR_INSTALL "include/gtsam/3rdparty/Eigen/")

	# The actual include directory (for BUILD cmake target interface):
	set(GTSAM_EIGEN_INCLUDE_FOR_BUILD "${CMAKE_SOURCE_DIR}/gtsam/3rdparty/Eigen/")
```

改为：

```cmake
	# set full path to be used by external projects
	# this will be added to GTSAM_INCLUDE_DIR by gtsam_extra.cmake.in
	set(GTSAM_EIGEN_INCLUDE_FOR_INSTALL "/home/qxit-ln-002408/packages/eigen-3.3.9/install/include/eigen3")

	# The actual include directory (for BUILD cmake target interface):
	set(GTSAM_EIGEN_INCLUDE_FOR_BUILD "/home/qxit-ln-002408/packages/eigen-3.3.9/install/include/eigen3")
```

## 编译安装

```shell
cd ~/packages/gtsam-4.0.3
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/qxit-ln-002408/packages/gtsam-4.0.3/install ..
make
make install
```

## 调用

**调用**GTSAM**库的CMakeLists.txt需要改成：**

```cmake
set(GTSAM_DIR           "/home/qxit-ln-002408/packages/gtsam-4.0.3/install/lib/cmake/GTSAM")
```