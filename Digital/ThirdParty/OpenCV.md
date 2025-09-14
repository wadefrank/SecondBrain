
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