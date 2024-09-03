> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s?__biz=MzA5MjU2NjYxNQ==&mid=2449494038&idx=1&sn=f144be7182839e22a421e0df9e9609f0&chksm=879fd52eb0e85c38c41921a05fd88bc951758d3f4e40e695ff27b12906450ff04825252ae7c9&scene=27)

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPmRdRvR1DCZP2NWFQmRfTyaQCb0aoj6UyWe9mQNS5nC4uQVFP5D3UOQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCP9tLG67zEtbwvjrDDicIrIfNJeW1T4koWiafxW3KYkv2ToosU1zNjtGibg/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCP7X40T7voa6IRn5tBIWtp3oHXSibE5w6KJfsBpI7lOKKuVDpecFc0siaw/640?wx_fmt=png&from=appmsg)

  

**十二款开源 OCR 开箱测评**

文字识别能力篇

![](https://mmbiz.qpic.cn/mmbiz_gif/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPYmuXsooCXjBjEL6qIEX6DZrcrUCJOia3dIC3LicZcWXkqjjh5Z6LMStQ/640?wx_fmt=gif&from=appmsg)

OCR（Optical Character Recognition，光学字符识别）作为信息爆炸时代的 “炼金术士”，以其高效且相对精确的性能，在海量纸质文档、扫描件、图片的文字信息提取方面发挥着举足轻重的作用。其广泛应用于教育、医疗、交通等多个行业领域，其重要性不言而喻。然而，目前开源 OCR 工具种类繁多，不同场景图像的识别效果却参差不齐，这给开发人员的选型工作带来了不小的挑战。为了尽可能全面测试 OCR 工具的识别能力，本次测评精心挑选了 12 款开源 OCR 工具，在五类不同数据集上进行横向评比，以期为用户提供更为准确、客观的选型参考。

开源 OCR 介绍与评测系列共分为三篇，本文为文字识别能力篇，评测开源 OCR 基本的文字识别能力，包括印刷中文、印刷英文、手写中文等三类基本类型，以及复杂自然场景和变形字体两类附加测评；第二篇为结构信息能力篇，对表格、票证等结构化信息的 OCR 能力进行测评；第三篇为 OCR Free 评测篇，评测开源多模态大模型对图片信息的提取和分析能力。

  

  

本次开源 OCR 文字识别能力测评选取了 12 款 OCR 工具，其中，独立工具有：PaddleOCR、RapidOCR、读光（开源版）、ChineseOCR、EasyOCR、Tesseract、OcrLiteOnnx、Surya、docTR、JavaOCR；文档分析 OCR 组件：RagFlow、Unstructured。

备注：本次测评均使用 OCR 工具自身提供的预训练模型进行测试，测试均采用工具的示例中提供的参数设置。除开源工具以外，选取百度 OCR 云服务测试结果作为参照。

**各 OCR 工具的测试版本如下：**

*   **PaddleOCR** V2.7.5
    
*   **读光 OCR**
    
*   **DocTR** V0.7.1
    
*   **Tesseract** V5.3.4
    
*   **ChineseOCR**
    
*   **OcrLiteOnnx** V1.6.1
    
*   **RapidOCR** V1.3.22
    
*   **JavaOCR** V1.0
    
*   **EasyOCR** V1.7.0
    
*   **RAGflow** V0.7.0
    
*   **Unstructured** V0.14.0
    
*   **Surya** V0.4.9
    
*   **百度 OCR** V2.0
    

为了全面评测 OCR 工具各种场景下的识别和解析能力，本次测评收集整理了多种类型文字识别的图片数据，包括印刷中英文、自然场景、手写文字和验证码等方面数据集，具体文字识别数据集分类如下：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCP8vBcHqsniaZNHOMJrOHt4AOcxPBaaRGUk2eEicCphwJhIl6I0T3IkSjw/640?wx_fmt=jpeg&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPcdEEn3wVEf07UDHH2nh34jWDkjsvcGRsUaVezchhD3ehRtP8IZUddA/640?wx_fmt=jpeg&from=appmsg)

文字识别能力主要评测 OCR 工具对文字的检测和识别能力，包括支持识别的字符集规模（生僻字），字体形变（字体、艺术字），图像旋转、形变、干扰信息、明暗、模糊等外部因素影响。

备注：文字识别能力只考察是否正确识别出字符，不考察文字结构信息（即输出结果的文字顺序）。其中，中文统计粒度为字，英文为单词（区分大小写），中英文标点符号相互区别。

*   **字符识别准确率（Precision****）：**正确识别的字符数 / 识别输出总字符数
    

*   **字符识别召回率（Recall****）：**正确识别的字符数 / 验证集总字符数
    
*   **字符识别综合评分（F-Score****）：**2*Precision*Recall/(Precision+Recal)
    
*   **平均响应时间****：**基准样本识别总时间 / 样本数量。
    

测评结果

  

  

（1）印刷中文的综合测评结果为：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPK3EH4vfcmcEICopg1hCmiaiaxRSicymhF4kjNAMmUAQ8ibJgn2290C9IHw/640?wx_fmt=jpeg&from=appmsg)

（2）印刷英文的综合测评结果为：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPmD80KWk87ZsCMUx38k5t3CSvKDPSWWtREyYVg8OMRVvdcRPM5icLSYw/640?wx_fmt=jpeg&from=appmsg)

（3）变形字体的艺术字测评结果为：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPQVFSwvKzhD7opG7dG0FCbRs5mH0PEzy2ahWLCGIZGVPYTGSWfyQe7Q/640?wx_fmt=jpeg&from=appmsg)

（4）自然场景的街景图片测评结果为：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPMpocIdIhibQ6Tnc6TzM37W7Teib9VsLWcPk6tt7j3NRVd7paC04eYwdw/640?wx_fmt=jpeg&from=appmsg)

（5）手写中文的综合测评结果为：

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPAlSAOmmBYOaSAz5GzD7gLNKmrdEUnngGH1uEqWm8e86dO136YX73wQ/640?wx_fmt=jpeg&from=appmsg)

**【关注公众号在后台回复 “OCR” 即可下载完整版报告。】**

测评总结

  

  

印刷中文识别准确度测试中，综合前三分别是 RapidOCR、RagFlow 和 Surya。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPibqhOuiaPSYnbOoC92a6etp8E9b7x2ZdgqB66UMz4pkFibHdYRVIaOYAg/640?wx_fmt=png&from=appmsg)

在印刷英文识别准确度测试环节，综合前三分别是 Surya、Unstructured 和读光 OCR，还是国外开源软件领先。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPyjH5ribcvqD2YaeicBE8ZGJZryrLfoBmErXgky9ia9fLRA5v4THkaWe1w/640?wx_fmt=png&from=appmsg)

在各种变形字体（艺术字、验证码等非标准字体）场景下，由于本次测评仅采用各 OCR 工具自身提供的预训练模型进行测试，识别准确度均较低，如需提高变形字体的准确率需要针对变形字体进行专项训练。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPXoBvqEgocAHm0AzbcHW6tqjzibpaRibF5kTtJCPRVAkEiaNuXicBibTmWWA/640?wx_fmt=png&from=appmsg)

在复杂多行文字的街景场景中，前三名分别是 RagFlow、RapidOCR 和 PaddleOCR，它们的综合评分相当接近，均略高于 70%。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPUI3V3hJktd7Wt2IjZDjCG23xvhpWwnCrjpvFOpQ7MKBwf3MuVPAdxA/640?wx_fmt=png&from=appmsg)

在手写中文识别场景下，综合前三分别是 RapidOCR、ChineseOCR 和 RagFlow。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPh4mOnlFQiaQUNMtHbpne6LNicMQCumB1M2cJHaBPpJNh6vqHzY2nwyicA/640?wx_fmt=png&from=appmsg)

在响应时间方面，表现优异的有 OcrLiteOnnx（0.01 秒级）、RagFlow（0.1 秒级），响应非常快。另外，ChineseOCR、EasyOCR 和 RapidOCR 表现也不错，平均时间小于 1 秒。

![](https://mmbiz.qpic.cn/mmbiz_png/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPK0ibN2BUKdbjseazf7MJVia0v42QqibcWZVYyt3bPj6bT1lvIDjsejMQQ/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_gif/8xx85faZV8tAvwX0PUZYkGjl0geNNMCPYmuXsooCXjBjEL6qIEX6DZrcrUCJOia3dIC3LicZcWXkqjjh5Z6LMStQ/640?wx_fmt=gif&from=appmsg)

随着大语言模型的快速发展和应用，我们对 OCR 识别的需求不再局限于字的识别，对于结构化信息抽取的需求越来越大。我们将在下一篇将对开源 OCR 工具的结构分析能力进行评测。同时，针对 OCR Free 类的大模型，如 TextMoneky、DocPedia、UReader、Pix2struct、Donut，以及国内研究的 InterVL 等，我们计划开展一次 **OCR Free 类评测**，敬请期待。

技术支持

  

  

开源 OCR 介绍与评测系列，由广州软件应用技术研究院（简称：广州软件院）提供技术指导和资源支持。

广州软件院成立于 2011 年 5 月 27 日（原广州中国科学院软件应用技术研究所，是由广州南沙开发区管委会与中国科学院软件研究所共建的事业法人单位），为广州市政府创新发展模式的试点单位之一，广东省首批新型研发机构。广州软件院秉承立足南沙、服务粤港澳大湾区、辐射 “一带一路” 国家的总体定位，聚焦于智慧城市领域的应用技术研究，重点在政务大数据、智能物联网、区块链、人工智能、智慧食药监管、智能交通、智能视频分析、电子数据取证、软件测评等方向开展技术研究及成果转移转化工作。

广州软件院先进软件测评实验室是专业从事软件和信息安全测评的第三方检验检测实验室，主要研究区块链、物联网、智能网联汽车、人工智能等新技术的测试和测评。实验室具备国家市场监管总局许可的国家级检验检测机构资质认定 (CMA)、中国合格评定认可技术委员会认可检验检测实验室(CNAS) 和中国国防科技工业实验室认可委员会认可检验检测实验室(DIAC)。

**关于《十二款开源 OCR 开箱测评 - 文字识别能力篇》，想要获取下载链接，点击右下角 在看 ，后台回复 OCR 即可。**

  

**END**

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/8xx85faZV8veLjOIEaALvq2SLUoxicmgX61lP7oXXlWBbhBkeOKswicA5YndMUpe0DDO7G4skZDrPfPeeicHTJUOQ/640?wx_fmt=jpeg&from=appmsg)

**官网：https://gzis.ac.cn**

**联系部门：科技处**

**联系电话：020-26912281**