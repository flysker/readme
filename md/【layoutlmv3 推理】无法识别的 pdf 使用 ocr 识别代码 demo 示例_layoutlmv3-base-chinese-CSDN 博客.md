> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_46398647/article/details/138133015?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-138133015-blog-137294077.235%5Ev43%5Epc_blog_bottom_relevance_base4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-138133015-blog-137294077.235%5Ev43%5Epc_blog_bottom_relevance_base4&utm_relevant_index=2)

#### 目录

*   [前情提要](#_1)
*   [一、安装依赖](#_8)
*   *   [1、直接安装的依赖](#1_9)
    *   [2、需要编译的依赖](#2_28)
    *   *   [1）Leptonica](#1Leptonica_30)
        *   [2）icu](#2icu_49)
        *   [3）Tesseract](#3Tesseract_53)
    *   [3、需要自行配置的依赖](#3_90)
*   [二、模型下载](#_96)
*   [三、更改 transformers 源码](#transformers_99)
*   [四、加载光学字符识别语言包](#_110)
*   [五、运行代码](#_116)
*   [六、结果展示](#_195)

前情提要
----

在做 pdf 转文本时，发现有些 pdf 文件中的文本无法进行识别  
使用过 pdfminer、PyMuPDF 识别为空  
OCR 的模型百度的自然最好，但是要收钱  
layoutlmv3 搜了半天找不到推理代码，全是训练代码，所以就自己研究着写了一下  
本次使用的是`layoutlmv3-base-chinese`  
多语言版本可以使用`layoutlmv3-large`

一、安装依赖
------

### 1、直接安装的依赖

[gcc](https://so.csdn.net/so/search?q=gcc&spm=1001.2101.3001.7020)、cmake 等自行安装最新版

```
pip install sentencepiece
yum install libtool
yum groupinstall "Development Tools"
yum install libjpeg-devel libpng-devel libtiff-devel
yum install poppler-utils

```

找到 pdfinfo

```
which pdfinfo
# /usr/bin/pdfinfo
# 添加该路径至环境变量
vim /etc/environment
# PATH="xxxx“这一行中

```

### 2、需要编译的依赖

#### 1）Leptonica

```
# 下载 Leptonica 的源代码
wget http://www.leptonica.org/source/leptonica-1.80.0.tar.gz

# 解压源代码包
tar -zxvf leptonica-1.80.0.tar.gz

# 进入解压后的目录
cd leptonica-1.80.0

# 配置并构建
./configure
make

# 安装
sudo make install

```

#### 2）icu

[https://github.com/unicode-org/icu/releases](https://github.com/unicode-org/icu/releases)  
下载`Source code(tar.gz)`后找到`icu-release-75-1/icu4c/source`  
和上面一样安装即可

#### 3）Tesseract

```
# 下载 tesseract-ocr 的源代码
git clone https://github.com/tesseract-ocr/tesseract.git

# 进入解压后的目录
cd tesseract

./autogen.sh

# 配置并构建
./configure

# 安装
make
sudo make install

```

```
# 检查版本
tesseract --version

```

得到以下结果则安装成功

```
tesseract 5.3.4-49-g577e8
 leptonica-1.80.0
  libjpeg 6b (libjpeg-turbo 1.2.90) : libpng 1.5.13 : libtiff 4.0.3 : zlib 1.2.7
 Found AVX512BW
 Found AVX512F
 Found AVX512VNNI
 Found AVX2
 Found AVX
 Found FMA
 Found SSE4.1
 Found OpenMP 201511
 Found libcurl/7.29.0 NSS/3.90 zlib/1.2.7 libidn/1.28 libssh2/1.8.0

```

### 3、需要自行配置的依赖

```
在这里插入代码片

```

二、模型下载
------

[https://huggingface.co/microsoft/layoutlmv3-base-chinese/tree/main](https://huggingface.co/microsoft/layoutlmv3-base-chinese/tree/main)  
![](https://i-blog.csdnimg.cn/blog_migrate/e2c7277ea7f71d24d9e7f3cd3500b288.png)

三、更改 transformers 源码
--------------------

找到自己虚拟环境的地址，实例：

```
/root/anaconda3/envs/your_envs/lib/python3.10/site-packages/transformers/models/layoutlmv3/processing_layoutlmv3.py

```

49 行更改为：

```
tokenizer_class = ("LayoutLMv3Tokenizer", "LayoutLMv3TokenizerFast",'XLMRobertaTokenizer','XLMRobertaTokenizerFast','LayoutXLMTokenizer')

```

四、加载光学字符识别语言包
-------------

源地址 [https://github.com/tesseract-ocr/tessdata](https://github.com/tesseract-ocr/tessdata)  
【chi_sim.traineddata】中文包[点击下载](https://raw.githubusercontent.com/tesseract-ocr/tessdata/main/chi_sim.traineddata)  
【eng.traineddata】英文包[点击下载](https://raw.githubusercontent.com/tesseract-ocr/tessdata/main/eng.traineddata)  
【enm.traineddata】数字包[点击下载](https://raw.githubusercontent.com/tesseract-ocr/tessdata/main/enm.traineddata)  
下载后放入`/usr/local/share/tessdata/`

五、运行代码
------

```
import os
import re
from transformers import XLMRobertaTokenizer,LayoutLMv3Tokenizer, AutoModel, AutoProcessor, LayoutLMv3ImageProcessor, LayoutLMv3Processor
from pdf2image import convert_from_path
from PIL import Image

def concatenate_text(text_list):
    list_len = len(text_list)
    result_list = []
    english_word_buffer = ''
    current_string = ''
    zh = None
    en = None
    for i,text in enumerate(text_list):
        if '\u4e00' <= text <= '\u9fff':
            zh = True
        elif i < list_len-3 and (('\u4e00' <= text_list[i+1] <= '\u9fff') or ('\u4e00' <= text_list[i+2] <= '\u9fff')):
            zh = True
        elif re.match(r'^[a-zA-Z0-9.?!-]+$', text):
            en = True
        else:
            zh = None
            en = None
        if zh:
            current_string += text
        elif en:
            english_word_buffer += text + " "
        else:
            pass
        if ('。') in text or ('.' in text):
            if current_string:
                if len(current_string)>2:
                    result_list.append(current_string.strip())  # 将当前字符串添加到结果列表中
                    current_string = ''
            if english_word_buffer:
                if len(english_word_buffer)>2:
                    result_list.append(english_word_buffer.strip())
                    english_word_buffer = ''

        zh = None
        en = None
    # 将最后一个字符串添加到结果列表中
    if current_string.strip():
        if len(current_string)>2:
            result_list.append(current_string.strip())
    if english_word_buffer.strip():
        if len(english_word_buffer)>2:
            result_list.append(english_word_buffer.strip())
    
    return result_list

model_name = "/your/model/path/layoutlmv3-base-chinese"
image_processor = LayoutLMv3ImageProcessor.from_pretrained(model_name, ocr_lang='chi_sim+eng')
tokenizer = XLMRobertaTokenizer.from_pretrained(model_name)
processor = LayoutLMv3Processor(image_processor=image_processor,tokenizer=tokenizer,apply_ocr=True)
feature_extractor = processor.feature_extractor

pdf_file = 'your/pdf_data/path/case_1.pdf'
pages = convert_from_path(pdf_file, 300)  # 300 是输出图片的 DPI（每英寸点数）
# 创建保存图片的目录
output_dir = os.path.join(os.path.dirname(pdf_file), 'images')
os.makedirs(output_dir, exist_ok=True)

for i, page in enumerate(pages):
    image_path = os.path.join(output_dir, f'page_{i + 1}.jpg')
    page.save(image_path, 'JPEG')
    image = Image.open(image_path)
    inputs = feature_extractor(image)
    print("*"*30,f"第{i}页","*"*30)
    text_list = inputs['words'][0]
    if text_list:
        print(concatenate_text(text_list))



```

六、结果展示
------

用规则函数 concatenate_text 拼接字符得到  
![](https://i-blog.csdnimg.cn/blog_migrate/e188190475fed819d1699c5f731ac73c.png)  
用 ChatGTP-3.5 整理后得到  
![](https://i-blog.csdnimg.cn/blog_migrate/42ee652f9cef1a54db435b9feda106c6.png)

2024 年 4 月 23 日，商汤科技带来全新升级的「[日日新 SenseNova 5.0](https://chat.sensetime.com/wb/home)」大模型，具备更强的知识、数学、推理及代码能力，综合性能全面对标 GPT-4 Turbo，并在主流客观评测上达到或超越 GPT-4 Turbo  
用商汤商量 - 对话大模型 5.0 整理后得到  
![](https://i-blog.csdnimg.cn/blog_migrate/f46e14c7ae30976e5fac539cf8a29fdf.png)

商量 - 文档大模型 Preview 得到  
![](https://i-blog.csdnimg.cn/blog_migrate/25c9464335bba6a611527ecf85086a47.png)