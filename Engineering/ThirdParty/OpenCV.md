
# OpenCV-3.3.1+Ubuntu20.04

## 下载

OpenCV 3.3.1 Github下载: [https://github.com/opencv/opencv/releases/tag/3.3.1](https://github.com/opencv/opencv/releases/tag/3.3.1)

opencv_contrib 3.3.1 Github下载: [https://github.com/opencv/opencv_contrib/releases/tag/3.3.1](https://github.com/opencv/opencv_contrib/releases/tag/3.3.1)

从以上Github网址下载zip文件，先解压opencv-3.3.1.zip，然后将opencv_contrib-3.3.1.zip解压到opencv-3.3.1文件夹下

## 安装

### 安装依赖项

```shell
# 安装依赖项
sudo apt install cmake build-essential libgtk2.0-dev libavcodec-dev 
sudo apt install libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev
sudo apt-get install libgtk2.0-dev
sudo apt-get install pkg-config

# 安装opencv-3.3.1
cd opencv-3.3.1
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D WITH_CUDA=OFF -D CMAKE_INSTALL_PREFIX=/home/wade/third_party/opencv-3.3.1/install OPENCV_EXTRA_MODULES_PATH=../opencv_contrib-3.3.1/modules ..
make -j4
make install
```

### 编译安装

### 安装报错

#### 1

```shell
In file included from /home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg.cpp:47:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp: In function ‘AVStream* icv_add_video_stream_FFMPEG(AVFormatContext*, AVCodecID, int, int, int, double, int)’:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp:1573:21: error: ‘CODEC_FLAG_GLOBAL_HEADER’ was not declared in this scope; did you mean ‘AV_CODEC_FLAG_GLOBAL_HEADER’?
 1573 |         c->flags |= CODEC_FLAG_GLOBAL_HEADER;
      |                     ^~~~~~~~~~~~~~~~~~~~~~~~
      |                     AV_CODEC_FLAG_GLOBAL_HEADER
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp: In function ‘int icv_av_write_frame_FFMPEG(AVFormatContext*, AVStream*, uint8_t*, uint32_t, AVFrame*)’:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp:1601:30: error: ‘AVFMT_RAWPICTURE’ was not declared in this scope
 1601 |     if (oc->oformat->flags & AVFMT_RAWPICTURE) {
      |                              ^~~~~~~~~~~~~~~~
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp: In member function ‘void CvVideoWriter_FFMPEG::close()’:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp:1775:35: error: ‘AVFMT_RAWPICTURE’ was not declared in this scope
 1775 |         if( (oc->oformat->flags & AVFMT_RAWPICTURE) == 0 )
      |                                   ^~~~~~~~~~~~~~~~
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp: In member function ‘bool CvVideoWriter_FFMPEG::open(const char*, int, double, int, int, bool)’:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp:2074:32: error: ‘AVFMT_RAWPICTURE’ was not declared in this scope
 2074 |     if (!(oc->oformat->flags & AVFMT_RAWPICTURE)) {
      |                                ^~~~~~~~~~~~~~~~
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp: In static member function ‘static AVStream* OutputMediaStream_FFMPEG::addVideoStream(AVFormatContext*, AVCodecID, int, int, int, double, AVPixelFormat)’:
/home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp:2379:25: error: ‘CODEC_FLAG_GLOBAL_HEADER’ was not declared in this scope; did you mean ‘AV_CODEC_FLAG_GLOBAL_HEADER’?
 2379 |             c->flags |= CODEC_FLAG_GLOBAL_HEADER;
      |                         ^~~~~~~~~~~~~~~~~~~~~~~~
      |                         AV_CODEC_FLAG_GLOBAL_HEADER
make[2]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/build.make:141: modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_ffmpeg.cpp.o] Error 1
make[1]: *** [CMakeFiles/Makefile2:4633: modules/videoio/CMakeFiles/opencv_videoio.dir/all] Error 2
make: *** [Makefile:163: all] Error 2
```

#### 解决方案如下（参考deepseek）：

打开文件opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp

```bash
vim /home/wade/third_party/opencv-3.3.1/modules/videoio/src/cap_ffmpeg_impl.hpp
```

进行以下修改

```cpp
// Line 1573
c->flags |= AV_CODEC_FLAG_GLOBAL_HEADER; // Replace CODEC_FLAG_GLOBAL_HEADER

// Line 2379
c->flags |= AV_CODEC_FLAG_GLOBAL_HEADER; // Replace CODEC_FLAG_GLOBAL_HEADER

// Line 1601
// Comment out or replace the condition
// if (oc->oformat->flags & AVFMT_RAWPICTURE) {
if (0) { // AVFMT_RAWPICTURE is deprecated

// Line 1775
// if( (oc->oformat->flags & AVFMT_RAWPICTURE) == 0 )
if (1) // Assume AVFMT_RAWPICTURE is not supported

// Line 2074
// if (!(oc->oformat->flags & AVFMT_RAWPICTURE)) {
if (1) { // Assume raw picture is not supported
```

**Explanation:**

- **CODEC_FLAG_GLOBAL_HEADER:** This flag was renamed in newer FFmpeg versions. Updating to `AV_CODEC_FLAG_GLOBAL_HEADER` aligns with the new API.
- **AVFMT_RAWPICTURE:** This flag was deprecated. The modifications effectively bypass the deprecated checks, assuming raw picture handling is no longer required. This approach is based on changes made in later OpenCV versions to accommodate FFmpeg updates.

**After making these changes, recompile OpenCV. Ensure FFmpeg libraries are correctly linked and consider testing video I/O functionality to confirm everything works as expected.**

### 2 ippicv下载问题

opencv 解决ippicv下载问题，离线:ippicv_2019_lnx_intel64_general_20180723.tgz

[https://blog.csdn.net/orDream/article/details/84311697](https://blog.csdn.net/orDream/article/details/84311697)

## 调用

```cmake
set(OpenCV_DIR "/home/wade/third_party/opencv-3.3.1/install/share/OpenCV")

find_package( OpenCV 3.3.1 REQUIRED )

include_directories( ${OpenCV_INCLUDE_DIRS} )

message(STATUS "OpenCV version: ${OpenCV_VERSION}")
message(STATUS "OpenCV include dirs: ${OpenCV_INCLUDE_DIRS}")
```

## 参考资料

安装OpenCV: [https://blog.csdn.net/louderIII/article/details/120815015](https://blog.csdn.net/louderIII/article/details/120815015)

安装opencv报错：[https://blog.csdn.net/qq_44357371/article/details/105966714](https://blog.csdn.net/qq_44357371/article/details/105966714)

# OpenCV使用

## 函数

### cv::bitwise_and()

用于执行两个图像之间的按位“且”操作，通常用于结合多个掩码、应用掩码到图像等。

```cpp
// 参数说明
// src1：第一个输入数组或图像（InputArray 类型）。
// src2：第二个输入数组或图像（InputArray 类型）。它必须与 src1 的大小和类型相同。
// dst：输出数组或图像（OutputArray 类型），它将存储结果。它的大小和类型将与 src1 和 src2 相同。
// mask：可选操作掩码（InputArray 类型），用于指定哪些元素需要处理。默认值为 noArray()，表示不使用掩码。
void bitwise_and(InputArray src1, InputArray src2, OutputArray dst, InputArray mask = noArray());
```

### cv::bitwise_or()

用于执行两个图像之间的按位“或”操作，通常用于合并掩码图像、应用图像效果等。

```cpp
// 参数说明
// src1：第一个输入数组或图像（InputArray 类型）。
// src2：第二个输入数组或图像（InputArray 类型）。它必须与 src1 的大小和类型相同。
// dst：输出数组或图像（OutputArray 类型），它将存储结果。它的大小和类型将与 src1 和 src2 相同。
// mask：可选操作掩码（InputArray 类型），用于指定哪些元素需要处理。默认值为 noArray()，表示不使用掩码。
void bitwise_or(InputArray src1, InputArray src2, OutputArray dst, InputArray mask = noArray());
```

### cv::Canny()

用于执行 Canny 边缘检测的函数，它能够有效地检测图像中的边缘。

```cpp
// 参数说明
// image：输入图像（InputArray 类型）。可以是单通道灰度图像（CV_8UC1）或多通道彩色图像（CV_8UC3 或 CV_8UC4）。对于彩色图像，通常是先转换为灰度图像再进行边缘检测。
// edges：输出的边缘图像（OutputArray 类型）。通常是一个单通道灰度图像，其中包含检测到的边缘信息。
// threshold1：第一个阈值。低阈值用于边缘连接的强度梯度。
// threshold2：第二个阈值。高阈值用于确定强边缘的梯度。
// apertureSize：Sobel 算子的孔径大小，默认为 3。可以是以下值之一：
//   - 3：使用 3x3 的 Sobel 滤波器。
//   - 5：使用 5x5 的 Sobel 滤波器。
//   - 7：使用 7x7 的 Sobel 滤波器。
// L2gradient：一个布尔值，默认为 false。如果设置为 true，则使用更精确的 L2 范数计算图像梯度幅值；否则使用 L1 范数。一般情况下设置为 false 即可。
void Canny(InputArray image, OutputArray edges, double threshold1, double threshold2, int apertureSize = 3, bool L2gradient = false);
```

### cv::countNonZero()

用于统计矩阵中非零元素个数。对于二值图像，它可以用来统计特定颜色（通常是白色）的像素数量。

```cpp
// 参数说明
// src：输入的单通道矩阵或图像（InputArray 类型）。通常是一个二值图像或灰度图像。
int countNonZero(InputArray src);
```

### cv::getStructuringElement()

用于创建结构元素（structuring element）的函数，通常用于图像形态学操作，如膨胀（dilation）、腐蚀（erosion）、开运算（opening）和闭运算（closing）等。

```cpp
// 参数说明
// shape：结构元素的形状，可以是以下几种形式之一：
//   - MORPH_RECT：矩形结构元素
//   - MORPH_CROSS：十字形结构元素
//   - MORPH_ELLIPSE：椭圆形结构元素
// ksize：结构元素的大小，即其宽度和高度。这是一个 Size 类型的参数。
// anchor：结构元素的锚点位置。默认情况下为 Point(-1,-1)，表示锚点位于结构元素中心。可以指定其他的 Point 类型值来调整锚点位置。
Mat getStructuringElement(int shape, Size ksize, Point anchor = Point(-1,-1));
```

###  cv::imwrite()

用于将图像保存到文件中。

```cpp
// 参数说明
// filename：保存图像的文件名。可以包含路径和文件扩展名（如 .png, .jpg 等）。
// img：要保存的图像（InputArray 类型）。通常是一个 cv::Mat 对象。
// params：可选参数，控制图像保存的格式和质量。这是一个整数向量，包含以下参数：
//   - CV_IMWRITE_JPEG_QUALITY：指定 JPEG 格式的图像质量（0-100），值越高质量越好，默认为 95。
//   - CV_IMWRITE_PNG_COMPRESSION：指定 PNG 格式的压缩级别（0-9），值越高压缩越强，默认为 3。
bool imwrite(const String& filename, InputArray img, const std::vector<int>& params = std::vector<int>());
```

###  cv::imread()

用于从文件中读取图像。

```cpp
cv::Mat img = cv::imread("path_to_your_image.png");
if (img.empty()) {
    std::cerr << "Error loading image" << std::endl;
    return -1;
}
```

### cv::imshow()

用于显示图像。

```cpp
cv::imshow("Original Image", img);
```

### cv::inRange()

创建一个二值掩码，只保留在给定颜色范围内的像素

```cpp
cv::Mat mask;
cv::inRange(src, cv::Scalar(0, 255, 0), cv::Scalar(0, 255, 0), mask);
```

### cv::morphologyEx()

执行形态学操作的函数，它可以进行膨胀（dilation）、腐蚀（erosion）、开运算（opening）、闭运算（closing）等操作。这些操作通常用于图像处理中的去噪声、分割物体、提取图像中的结构等任务。

```cpp
// 参数说明
// src：输入图像（InputArray 类型）。通常是一个 cv::Mat 对象，单通道灰度图像或多通道彩色图像。
// dst：输出图像（OutputArray 类型）。与 src 相同的类型和尺寸，用于存储操作后的图像。
// op：指定形态学操作的类型，可以是以下几种操作之一：
//   - MORPH_OPEN：开运算（先腐蚀后膨胀）
//   - MORPH_CLOSE：闭运算（先膨胀后腐蚀）
//   - MORPH_GRADIENT：形态学梯度（膨胀图像减去腐蚀图像）
//   - MORPH_TOPHAT：顶帽运算（原始图像减去开运算后的图像）
//   - MORPH_BLACKHAT：黑帽运算（闭运算后的图像减去原始图像）
// kernel：形态学操作的结构元素（InputArray 类型）。可以使用cv::getStructuringElement函数创建。
// anchor：结构元素的锚点位置（Point 类型）。默认为 -1,-1，即结构元素的中心点。
// iterations：形态学操作的迭代次数。默认为 1。
// borderType：边界处理方式。默认为 BORDER_CONSTANT。
// borderValue：当 borderType 为 BORDER_CONSTANT 时使用的边界值。
void morphologyEx(InputArray src, OutputArray dst, int op, InputArray kernel, Point anchor = Point(-1,-1), int iterations = 1, int borderType = BORDER_CONSTANT, const Scalar& borderValue = morphologyDefaultBorderValue());
```

### cv::threshold()

用于将灰度图像或单通道图像转换为二值图像，或者将图像的每个像素值按照指定的阈值进行处理。

```cpp
// 参数说明
// src：输入图像，单通道（灰度）图像，类型为 CV_8UC1 或 CV_32FC1。
// dst：输出图像，与输入图像的大小和类型相同。
// thresh：阈值，用于对像素值进行分类的阈值。
// maxval：当像素值超过（或低于，取决于类型）阈值时赋予的最大值。
// type：阈值类型，指定阈值操作的类型
//   - cv::THRESH_BINARY 大于thresh使用maxval，否则为0
//   - cv::THRESH_BINARY_INV 大于thresh使用0，否则为maxval
double threshold(InputArray src, OutputArray dst, double thresh, double maxval, int type);
```

### cv::waitKey()

用于等待键盘事件，它常用于在显示图像窗口时暂停程序，直到用户按下键盘上的某个键。

```cpp
// 参数说明
// delay：等待时间，以毫秒为单位。
//   - delay = 0：无限期等待，直到用户按下某个键。
//   - delay > 0：等待指定的毫秒数。如果在此期间按下任何键，函数会立即返回。
//   - delay < 0：行为与 delay = 0 相同，即无限期等待。
int cv::waitKey(int delay = 0);

// 示例
cv::waitKey(0);
```

## 类

### cv::Mat

### cv::Scalar

用于表示多通道的颜色值或其他类型的数据

```cpp
// 使用四个值初始化 (B, G, R, Alpha)
cv::Scalar scalar3(255, 255, 0, 255);
std::cout << "Scalar3: " << scalar3[0] << ", " << scalar3[1] << ", " << scalar3[2] << ", " << scalar3[3] << std::endl;
```

## 功能

### 掩码

```cpp
// 获取图像的尺寸
int height = img.rows;
int width = img.cols;

// 创建一个全黑的掩码
cv::Mat mask = cv::Mat::zeros(height, width, CV_8UC1);
```

## 数据类型常量

### CV_8UC1

用于定义图像矩阵的类型，它表示一个单通道（灰度）图像。