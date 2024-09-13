> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/anywhereyouare/article/details/138346353?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-138346353-blog-127005276.235^v43^pc_blog_bottom_relevance_base4&spm=1001.2101.3001.4242.1&utm_relevant_index=4)

起因是用祖传的 ppocr.exe 识别图片中的文字计算时间在 1600ms~2400ms 不等，且 use_gpu=True/False 在时间上没有区别，差不多可以推断之前用的应该不是 [GPU](https://so.csdn.net/so/search?q=GPU&spm=1001.2101.3001.7020) 的 OCR 推理库。于是自己编译一个 GPU 的 ppocr.exe。

本篇文章主要讲如何编译  
![](https://i-blog.csdnimg.cn/blog_migrate/9cd485a32a8487e585f272b604d5d889.png)

### 一、下载 PaddleOCR 代码

```
git clone https://github.com/PaddlePaddle/PaddleOCR

```

到本地是这样的，默认是 main 分支，我直接用了 (release/2.7)  
![](https://i-blog.csdnimg.cn/blog_migrate/1e97c8880bb67ce2b5cb350665a78b90.png)  
待会我们编译的时候只需要`deploy/cpp_infer`, 且编译指南在`windows_vs2019_build.md`，按步骤来基本没有问题, 所以本文重要记录环境配置![](https://i-blog.csdnimg.cn/blog_migrate/180901df4031deeb4a0c1402ab892f14.png)

### 二、下载 Windows 推理库

[C++ 推理库下载](https://www.paddlepaddle.org.cn/inference/master/guides/install/download_lib.html#windows)  
![](https://i-blog.csdnimg.cn/blog_migrate/6f56adb395fd00aa0df6719b59ad2ad4.png)本人需要 GPU 版本的，所以直接下载的最后一个，所以 win 也要配置相应的 cuda、cuDNN、TensorRT  
**注意版本，本人开始看错了，在后面调用 GPU 的时候，报了错误**  
解压后打开 version.txt，版本一定要按这个来！！！  
![](https://i-blog.csdnimg.cn/blog_migrate/3c2f3be9973731c9151446e78aca2a69.png)

报错如下：大抵就是 cuda、cuDNN、TensorRT 版本不匹配  
![](https://i-blog.csdnimg.cn/blog_migrate/9a5fe78a4f78972bbc70b796e5a6f0b8.png)

### 三、环境准备

cmake 下载：[https://cmake.org/download/](https://cmake.org/download/)  
OpenCV 下载：[https://opencv.org/releases/](https://opencv.org/releases/)  
opencv 环境变量：![](https://i-blog.csdnimg.cn/blog_migrate/fd8e4cca9950ac4d3eaa931f57ec2044.png)  
默认有以下环境，我们只要配置：CUDA、cuDNN、TensorRT 安装  
![](https://i-blog.csdnimg.cn/blog_migrate/17ec0f9bf44de2d038a60a28d113287f.png)

##### 3.1 CUDA 下载

下载地址：[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)  
![](https://i-blog.csdnimg.cn/blog_migrate/08034747caf89b130ae05a4e8d0540df.png)  
CUDA 安装直接默认，安装好后检查一下环境变量  
![](https://i-blog.csdnimg.cn/blog_migrate/370e29f0cd45c520eb066f4b06864a58.png)

##### 3.2 cuDNN 下载

下载地址：[https://developer.nvidia.com/rdp/cudnn-download](https://developer.nvidia.com/rdp/cudnn-download)  
下载对应的版本即可，本人下载的是 8.9.1（要与 cuda 版本匹配）  
![](https://i-blog.csdnimg.cn/blog_migrate/eb64748a4f2d05350aa1975588e3b135.png)  
下载好后解压，分别将 cudnn 三个文件夹的内容分别复制到 cuda 对应的文件夹里面![](https://i-blog.csdnimg.cn/blog_migrate/bdd4317298f8b9159e41a232b24e8f31.png)

##### 3.3 tensorRT

进入官方链接下载对应版本的 tensorRT（需要开发者账号）：[https://developer.nvidia.com/tensorrt/download](https://developer.nvidia.com/tensorrt/download)  
下载好后解压 TensorRT-8.6.1.6.Windows10.x86_64.cuda-12.0.zip(也要与 cuda 版本相匹配）

*   将下载好的 TensorRT-8.6.1.6 文件夹中的 include 文件移动到 CUDA 文件夹下的 include 文件夹
*   将下载好的 TensorRT-8.6.1.6 文件夹中的 lib 中的 dll 文件移动到 CUDA 下的 bin 文件夹
*   将下载好的 TensorRT-8.6.1.6 文件夹中的 lib 中的 lib 文件移动到 CUDA 下的 lib\x64 文件夹  
    ![](https://i-blog.csdnimg.cn/blog_migrate/bc8c12acbb0264dbdcc5da767c24e846.png)  
    到这里可以重启一下电脑，防止个别环境变量没有生效

### 四、构建 Visual Studio 项目

在 PaddleOCR/deploy/cpp_infer 里面新建一个 cmakeBuild 文件夹，名字任意，打开 cmake-gui, 后续步骤在 PaddleOCR/deploy/cpp_infer/docs/windows_vs2019_build.md，以下是要注意的点

*   GPU 版本，在 cpu 版本的基础上，还需填写以下变量

*   `CUDA_LIB`: CUDA 地址，如 `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\lib\x64`
*   `CUDNN_LIB`: 和 CUDA_LIB 一致
*   `TENSORRT_DIR`：TRT 下载后解压缩的位置，如 `D:\TensorRT-8.6.1.6`
*   `WITH_GPU`: 打勾
*   `PADDLE_LIB`: 下载的推理库路径
*   `WITH_TENSORRT`：打勾  
    ![](https://i-blog.csdnimg.cn/blog_migrate/ca42ad465ba1dd3251b5c3d6be21f06a.png)  
    构建成功后，剩下的步骤可以在下载的`PaddleOCR\deploy\cpp_infer\docs\windows_vs2019_build.md`里面查看  
    注意如果按源码编译的话运行的目录只能在教程里面的`cd /d D:\projects\cpp\PaddleOCR\deploy\cpp_infer`，如果不是的话会有以下报错：  
    ![](https://i-blog.csdnimg.cn/blog_migrate/b97a2cb488f5aa53b381d1942e9ce768.png)  
    [C++ Windows 编译部署 PaddleOCR GPU 版本 (二）](https://blog.csdn.net/anywhereyouare/article/details/139823848?spm=1001.2014.3001.5501)