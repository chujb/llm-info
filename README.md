# llm-info
记录关于LLM的学习心得

重要的LLM开源模型：

1、llama.cpp（LLAMA的C++语言实现，将模型缩小化，可在CPU运行）：

https://github.com/ggerganov/llama.cpp

代码解析：

https://blog.csdn.net/sinat_35576477/article/details/130895108

https://mp.weixin.qq.com/s?__biz=MzA4MjY4NTk0NQ==&mid=2247519554&idx=1&sn=c619e9907fb515e88b6265cf7017b726&chksm=9f8337d4a8f4bec27c755148f904668a46c85322e6ba31bc6b0286887bbb30f8a24a49d2d918&mpshare=1&scene=23&srcid=1107jzAJDvx0GcmHJO92iIcA&sharer_shareinfo=3dc56c7e8fdbf855398ab67e34dfb5c7&sharer_shareinfo_first=3dc56c7e8fdbf855398ab67e34dfb5c7#rd

llama.cpp使用C++ API开发：
https://blog.csdn.net/u012234115/article/details/134604154

2、whisper.cpp（语音大模型，可以识别语音，将语音转文字，文字转语音，依赖llama.cpp）：

https://github.com/ggerganov/whisper.cpp
https://github.com/openai/whisper

3、localGPT（本地知识库，识别PDF文档，依赖llama.cpp）：
https://github.com/PromtEngineer/localGPT

ingest.py，使用LangChain工具解析文档并使用InstructorEmbeddings在本地创建嵌入，然后，它使用Chroma vector store将结果存储在本地向量数据库中。（生成本地数据库，首次需要下载hkunlp/instructor-large 1.5G的模型）

run_localGPT.py，使用本地LLM来理解问题并创建答案。答案的上下文是从本地向量存储中提取的，使用相似性搜索从文档中定位正确的上下文片段。（问题：答案生成速度慢）

可以将此本地LLM替换为HuggingFace的任何其他LLM。确保你选择的任何LLM都是HF格式的。

4、privateGPT（本地知识库，该工具支持多种文档格式，包括CSV、Word文档、EverNote、Email、EPub、HTML文件、Markdown、Outlook消息、Open Document Text、PDF、PowerPoint文档和UTF-8编码的文本文件。）：
https://github.com/zylon-ai/private-gpt

5、code-llama（代码大模型）：
https://github.com/Pythagora-io/gpt-pilot

6、llamafile（跨平台llama可执行程序框架，基于llama.cpp和Cosmopolitan Libc库）：

https://github.com/Mozilla-Ocho/llamafile
https://github.com/jart/cosmopolitan
https://github.com/ggerganov/llama.cpp

llamafile编程示例：https://ashishware.com/2024/01/05/Llamafile/

主要接口函数：

模型加载：llama_init_from_gpt_params，位置：llama.cpp\main\main.cpp等

模型释放：llama_free，位置：llama.cpp\main\main.cpp等

输入语言token化：llama_tokenize将字符串转换为整形，位置：llama.cpp\main\main.cpp等

token长度有限制，会进行切分，获取第一个token BOS：llama_token_bos

token转换为字符：llama_token_to_piece/llama_token_to_byte，位置：llama.cpp\main\main.cpp等，llama_token_to_str已经不支持

模型执行：老llama.cpp版本llama_eval，新版本中使用llama_decode，主要看打印日志确认：failed to eval；

token推理结果获取：llama_get_logits等相关函数，位置：llama.cpp\llama.cpp等

可选参数说明：

-ngl 9999 表示模型的多少层放到 GPU 运行，其他在 CPU 运行，如果没有 GPU 则可设置为 -ngl 0 ，默认是 9999，也就是全部在 GPU 运行（需要装好驱动和 CUDA 运行环境）。

--host 0.0.0.0 web 服务的hostname，如果只需要本地访问可设置为 --host 127.0.0.1 ，如果是0.0.0.0 ，即网络内可通过 ip 访问。

--port 8080 web服务端口，默认 8080 ,可通过该参数修改。

-t 16 线程数，当 cpu 运行的时候，可根据 cpu 核数设定多少个内核并发运行。

其他参数可以通过 --help 查看。

llamafile支持以下CPU类型：

AMD64架构的微处理器必须支持SSSE3指令集。如果不支持，llamafile将显示错误信息并无法运行。这意味着，如果您使用的是Intel CPU，至少需要是Intel Core或更新系列（约2006年以后）；如果是AMD CPU，至少需要是Bulldozer或更新系列（约2011年以后）。如果您的CPU支持AVX或更高级的AVX2指令集，llamafile将利用这些特性以提升性能。目前AVX512及更高级指令集的运行时调度尚未得到支持。

ARM64架构的微处理器必须支持ARMv8a+指令集。从Apple Silicon到64位Raspberry Pis的设备都应该兼容，只要您的权重数据能够适应内存容量。

7、Personal Assistant框架

Langchain（数据解析，支持本地和网路数据接口，参考localGPT）+whisper（语音大模型：语音识别和转换，语音转文字，文字转语音）+LLM（大模型：支持各种大模型）+llamafile跨平台打包和提供前端web交互页面

支持PC、平板、手机和智能眼镜等产品，实现所见即检视识别，所拍即检索识别，所听即检视识别等能力

8、容器技术

Linux容器(Container)发展史：https://www.cnblogs.com/dgdg/p/16163687.html

容器技术及其应用白皮书：https://blog.csdn.net/wh211212/article/details/53535881

白话容器基础：https://cloud.tencent.com/developer/article/2074761
https://blog.csdn.net/u012271526/article/details/120565866

OpenGLES 与 EGL 基础概念：https://zhuanlan.zhihu.com/p/74006499

https://zhuanlan.zhihu.com/p/432215702

https://www.cnblogs.com/yongdaimi/p/11244950.html

Nvidia GPU如何在Kubernetes 里工作：https://zhuanlan.zhihu.com/p/58919502

Android进阶5：SurfaceView实现原理分析：https://juejin.cn/post/6844903968217235469

https://www.jianshu.com/p/3b5b6f2469d8

https://github.com/huanzhiyazi/articles/issues/29

虚拟化技术与文件隔离：https://demonlee.tech/archives/container-history-1

Qemu-pipe简介：https://www.cnblogs.com/edver/p/16255025.html

https://blog.csdn.net/woai110120130/article/details/94304596

GPU虚拟化的评价标准与实现策略：https://blog.csdn.net/u011364612/article/details/53967132

android容器大全：
https://paul.pub/android-on-chrome-os/

https://github.com/huataihuang/cloud-atlas-draft/blob/master/develop/android/linux/install_and_run_android_apps_on_linux_in_container.md

Minijail：https://google.github.io/minijail/

anbox分析：https://blog.csdn.net/qq_36383272/category_9934190.html
https://blog.csdn.net/qq_36383272/article/details/105680570?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-105680570-blog-105163579.235%5Ev43%5Epc_blog_bottom_relevance_base8&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-105680570-blog-105163579.235%5Ev43%5Epc_blog_bottom_relevance_base8&utm_relevant_index=9


