> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [lulutongtong.blog.csdn.net](https://lulutongtong.blog.csdn.net/article/details/138034137?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-138034137-blog-139608695.235%5Ev43%5Epc_blog_bottom_relevance_base4&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-138034137-blog-139608695.235%5Ev43%5Epc_blog_bottom_relevance_base4&utm_relevant_index=1)

> 文章浏览阅读 1.7w 次，点赞 13 次，收藏 103 次。

#### 目的

你知道国内大模型多少是基于 Llama2 改造的，你就知道 Llama 模型有多厉害了，那么现在 Llama3 刚出来，搞自媒体的赶紧蹭 Llama3 的流量，搞技术的[程序员](https://so.csdn.net/so/search?q=%E7%A8%8B%E5%BA%8F%E5%91%98&spm=1001.2101.3001.7020)赶紧抓住研究一下，看能否技术变现一下，尤其搞技术的，再不动到时年龄大了被裁员了不要怨天怨地的！

#### 操作难度等级

非程序员的普通人及产品经理都可以按此文章说来本地部署

#### 15 分钟本地 Windows 电脑部署 Llama3 开源大模型

##### 1、下载安装 Ollama

访问 https://ollama.com，下载 Ollama [客户端](https://so.csdn.net/so/search?q=%E5%AE%A2%E6%88%B7%E7%AB%AF&spm=1001.2101.3001.7020)，下载 Windows 版本，如果你的电脑是 MacOs, 下载对应的版本即可。

安装完成后，打开 Windows 命令窗口，输入 ollama，出现如下提示，说明安装成功，可以使用了：  
![](https://i-blog.csdnimg.cn/blog_migrate/f29a4fc11b567b9b0951aa8625aa1480.jpeg)

##### 2、使用 Ollama 的命令下载 Llama3 模型文件

在 ollama 命令窗口输入：

ollama run llama3

按回车后就开始自动下载 Llama3 模型文件，默认下载最新的 8B 版本，模型文件大小 4.7G。

等待 5 到 10 分钟即可下载完成（下载文件途中可以去上个厕所或者关注一下我的账号）  
![](https://i-blog.csdnimg.cn/blog_migrate/71a3fc3e0ca3ec2e11a3fdd33b066687.jpeg)

##### 3、使用 Llama3 聊天对话功能

下载完成后出现 send a message 提示时，就可以直接与它进行对话了。看看我和它的对话：  
![](https://i-blog.csdnimg.cn/blog_migrate/ab3f0e9160d1ef014a9079113a987bf1.jpeg)

##### 4、本地 Llama3 API 接口调用

ollama 会自动生成 Llama3 的 API 接口访问地址，API 文档地址：https://github.com/ollama/ollama/blob/main/docs/api.md

请求地址：

```
curl http://localhost:11434/api/chat -d '{
  "model": "llama3",
  "messages": [
    {
      "role": "user",
      "content": "why is the sky blue?"
    }
  ],
  "stream": false
}'

```

不会写代码的，使用 Postman 去调试：

![](https://i-blog.csdnimg.cn/blog_migrate/281f4e1a8f500f3bf42f8ae6b6513c88.jpeg)