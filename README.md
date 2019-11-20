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

# 其它说明
- 使用`mlcpp`的原因是[lsrock1/maskrcnn_benchmark.cpp](https://github.com/lsrock1/maskrcnn_benchmark.cpp)中`ROIAlign_cuda.cu`无法编译通过
- 将`libtorch`和`opencv`的动态库拷贝到单一目录，并将路径添加到`Windows-System Properties-Advanced-System Variables-Path`中
- 工程中`train.cpp`代码未做修改，根据训练数据目录对应修改

# 补充说明
- 关于[lsrock1/maskrcnn_benchmark.cpp](https://github.com/lsrock1/maskrcnn_benchmark.cpp)中`ROIAlign_cuda.cu`无法编译通过的解决办法
- 通过`maskrcnn-benchmark/INSTALL.md`中`Windows 10`一节方法，执行`python setup.py build develop`过程中，观察执行过程中调用`nvcc`编译过程，拷贝CMD下输出语句，改现代码如下
```
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin\nvcc.exe -c D:\CodesRepositories\maskrcnn-benchmark\maskrcnn_benchmark\csrc\cuda\SigmoidFocalLoss_cuda.cu -o build\temp.win-amd64-3.7\Release\CodesRepositories\maskrcnn-benchmark\maskrcnn_benchmark\csrc\cuda\SigmoidFocalLoss_cuda.obj -ID:\CodesRepositories\maskrcnn-benchmark\maskrcnn_benchmark\csrc -ID:\Python37\lib\site-packages\torch\include -ID:\Python37\lib\site-packages\torch\include\torch\csrc\api\include -ID:\Python37\lib\site-packages\torch\include\TH -ID:\Python37\lib\site-packages\torch\include\THC "-IC:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\include" -ID:\Python37\include -ID:\Python37\include "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\ATLMFC\include" "-IC:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.17763.0\ucrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.17763.0\shared" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.17763.0\um" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.17763.0\winrt" "-IC:\Program Files (x86)\Windows Kits\10\include\10.0.17763.0\cppwinrt" -Xcompiler /EHsc -Xcompiler /wd4819 -Xcompiler /MD -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ --expt-relaxed-constexpr -DCUDA_HAS_FP16=1 -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_C -D_GLIBCXX_USE_CXX11_ABI=0 -gencode=arch=compute_52,code=sm_52
```
将最后几个`-D`的定义提出来如下
```
-D__CUDA_NO_HALF_OPERATORS__ 
-D__CUDA_NO_HALF_CONVERSIONS__ 
-D__CUDA_NO_HALF2_OPERATORS__ 
-DCUDA_HAS_FP16=1 
-D__CUDA_NO_HALF_OPERATORS__ 
-D__CUDA_NO_HALF_CONVERSIONS__ 
-D__CUDA_NO_HALF2_OPERATORS__ 
-DTORCH_API_INCLUDE_EXTENSION_H 
-DTORCH_EXTENSION_NAME=_C 
-D_GLIBCXX_USE_CXX11_ABI=0 
```
去掉`-D`，添加下工程的`Property Pages-CUDA C/C++ - Host - Preprocessor Definitions`中，编译`.cu`文件成功
