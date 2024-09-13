> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/anywhereyouare/article/details/139823848?spm=1001.2014.3001.5501)

具体编译步骤见上一篇文章，这里不再说  
[C++ Windows 编译部署 PaddleOCR GPU 版本 (一）](https://blog.csdn.net/anywhereyouare/article/details/138346353?spm=1001.2014.3001.5501)  
本篇文章主要讲踩的坑：

### 1、use_gpu=true 运行失败，false 运行成功

不知道当时脑子欠抽还是什么，没看清就直接下载相关库去了，开始下载的 windows 推理库打开 version.txt 文件  
![](https://img-blog.csdnimg.cn/direct/726e839ac39445079901e9f5a2af6e87.png)  
[cuDNN 下载](https://developer.nvidia.com/rdp/cudnn-archive#a-collapse824-114)没有看到 for CUDA11.2, 且是 win x64 的，于是下载了一个 cuda11.4  
![](https://img-blog.csdnimg.cn/direct/b15ca1f35cbc47e0987069876fde2174.png)  
TensorRT 8.2 下载：[添加链接描述](https://developer.nvidia.com/nvidia-tensorrt-8x-download)

![](https://img-blog.csdnimg.cn/direct/381b7a96fb9942848bf13f1e4dc61c63.png)  
这个配置下，生成都没有问题，但是只要把`use_gpu=true`, 运行就会报错  
所以本人直接下载最新的，按 version.txt 里面的来  
推理库链接：[C++ 推理库](https://www.paddlepaddle.org.cn/inference/master/guides/install/download_lib.html#windows)  
![](https://img-blog.csdnimg.cn/direct/78d11c43d82448878f1d29c2b820a10a.png)

### 2、启动目录改变后，运行失败

教程里面是在`xx\PaddleOCR\deploy\cpp_infer`这一级目录下运行的，但是我最终是要在我自己的项目里面启动生成的`ppocr.exe`  
先看一下报错：  
![](https://img-blog.csdnimg.cn/direct/f535e5e6499a418e8386db8eb058f188.png)  
所以全局搜索一下`ppocr_keys_v1.txt`  
![](https://img-blog.csdnimg.cn/direct/5dba1144be8a4a1ab69512a8809ec47b.png)  
所以启动目录应该和`rec_char_dict_path`满足你写的这种关系，比如我把`rec_char_dict_path`=`ppocr_keys_v1.txt`, 那么假设我在`xx\PaddleOCR\deploy\cpp_infer`启动，就得把`ppocr_keys_v1.txt`放在该目录下，  
如果我在`xx\PaddleOCR\deploy\cpp_infer\build\Release`下启动，我就得把`ppocr_keys_v1.txt`放在该级目录，具体看自己需求定  
![](https://img-blog.csdnimg.cn/direct/b8126878e5b34435b00d43694f76c1bf.png)

### 3、把识别到的结果写入 txt 文本

添加一下代码：  
![](https://img-blog.csdnimg.cn/direct/94786a64de5a4355ac8ab70f9b592c46.png)  
结果：  
![](https://img-blog.csdnimg.cn/direct/8d78dd0de2b74e0a88a348f83ff0c5ea.png)

### 4、相关文件

下载的`dirent.h`拷贝到`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\VS\include`  
下载链接：[dirent.h 下载](https://paddleocr.bj.bcebos.com/deploy/cpp_infer/cpp_files/dirent.h)  
![](https://img-blog.csdnimg.cn/direct/aaba142d93fa4be1960f9e8f00a0603b.png)  
缺少 `zlibwapi.dll` [下载链接](https://docs.nvidia.com/deeplearning/cudnn/latest/release-notes.html)  
`zlibwapi.lib`文件放到`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.5\lib`  
`zlibwapi.dll`文件放到`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\12.0\bin`

![](https://img-blog.csdnimg.cn/direct/5e1f5a2753654d91b54f8969e7737df7.png)