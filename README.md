# llm-info
记录关于LLM的学习心得

重要的LLM开源模型：

1、llama.cpp（LLAMA的C++语言实现，将模型缩小化，可在CPU运行）：

https://github.com/ggerganov/llama.cpp

代码解析：

https://blog.csdn.net/sinat_35576477/article/details/130895108

https://mp.weixin.qq.com/s?__biz=MzA4MjY4NTk0NQ==&mid=2247519554&idx=1&sn=c619e9907fb515e88b6265cf7017b726&chksm=9f8337d4a8f4bec27c755148f904668a46c85322e6ba31bc6b0286887bbb30f8a24a49d2d918&mpshare=1&scene=23&srcid=1107jzAJDvx0GcmHJO92iIcA&sharer_shareinfo=3dc56c7e8fdbf855398ab67e34dfb5c7&sharer_shareinfo_first=3dc56c7e8fdbf855398ab67e34dfb5c7#rd

2、whisper.cpp（语音大模型，可以识别语音，将语音转文字，文字转语音，依赖llama.cpp）：

https://github.com/ggerganov/whisper.cpp
https://github.com/openai/whisper

3、localGPT（本地知识库，识别PDF文档，依赖llama.cpp）：
https://github.com/PromtEngineer/localGPT

4、privateGPT（本地知识库，该工具支持多种文档格式，包括CSV、Word文档、EverNote、Email、EPub、HTML文件、Markdown、Outlook消息、Open Document Text、PDF、PowerPoint文档和UTF-8编码的文本文件。）：
https://github.com/zylon-ai/private-gpt

5、code-llama（代码大模型）：
https://github.com/Pythagora-io/gpt-pilot

6、llamafile（跨平台llama可执行程序框架，基于llama.cpp和Cosmopolitan Libc库）：

https://github.com/Mozilla-Ocho/llamafile
https://github.com/jart/cosmopolitan
https://github.com/ggerganov/llama.cpp

主要接口函数：

模型加载：llama_init_from_gpt_params，位置：llama.cpp\main\main.cpp等

模型释放：llama_free，位置：llama.cpp\main\main.cpp等

输入语言token化：llama_tokenize将字符串转换为整形，位置：llama.cpp\main\main.cpp等

token长度有限制，会进行切分，获取第一个token BOS：llama_token_bos

token转换为字符：llama_token_to_piece/llama_token_to_byte，位置：llama.cpp\main\main.cpp等，llama_token_to_str已经不支持

模型执行：老llama.cpp版本llama_eval，新版本中使用llama_decode，主要看打印日志确认：failed to eval；

token推理结果获取：llama_get_logits等相关函数，位置：llama.cpp\llama.cpp等

7、Personal Assistant框架
