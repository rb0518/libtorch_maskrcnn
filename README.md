# 环境
  - Window 10
  - Visual Studio 2017
  - CUDA cuda_10.1.168_425.25_win10 
  - cudnn-10.1-windows10-x64-v7.6.0.64
  - libtorch-win-shared-with-deps-1.3.0
  - OPENCV 4.1.0 and OPENCV-contrib
  - [mask_rcnn_pytorch](https://github.com/Kolkir/mlcpp)
  
# 准备工作
- `Visual Studio 2017` 编译 `opencv` , 拷到`d:\thirdparty\opencv` 目录下
- 从[pytorch.org](https://pytorch.org/)下载`libtorch-win-shared-with-deps-1.3.0`解压目录为`d:\libtorch`

# 创建VS工程

- 在`Visual Studio 2017`中通过`New Prject`-`NVIDIA`-`CUDA 10.1`创建工程
- 将mask_rcnn_pytorch中源文件拷贝到工程目录下
- 添加`Opencv`和`libtorch`路径到工程中
- 添加`Opencv`和`libtorch`的`.lib`文件到工程中
- 在工程`Property Pages`页面`C/C++`-`Command Line`中`Additional Options`中添加`/std:c++17`

其它说明
- 使用`mlcpp`的原因是[lsrock1/maskrcnn_benchmark.cpp](https://github.com/lsrock1/maskrcnn_benchmark.cpp)中`ROIAlign_cuda.cu`无法编译通过
- 将`libtorch`和`opencv`的动态库拷贝到单一目录，并将路径添加到`Windows-System Properties-Advanced-System Variables-Path`中
- 工程中`train.cpp`代码未做修改，根据训练数据目录对应修改