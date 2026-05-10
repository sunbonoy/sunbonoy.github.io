# 使用llama.cpp部署本地AI模型

## 下载llama.cpp
- 地址:[llam.cpp](https://github.com/ggml-org/llama.cpp/releases)
- NVIDA显卡选CUDA版；纯CPU选Vulkan版

## 下载模型
- 地址:[hf-mirror](https://hf-mirror.com), 或者魔搭社区
- 选择guff文件的模型, 比如：[Qwen3.5-9B-GGUF](https://hf-mirror.com/unsloth/Qwen3.5-9B-GGUF)

## 安装llama.cpp
- 新建llama目录，解压全部程序文件包到这个目录
- 下载的模型文件拷贝到安装目录

## 运行server(Windows下)
**示例**：
```
llama-server.exe -m model.gguf --port 8080 -c 4096
```
**实际运行指令：**
```
llama-server.exe -m Qwen3.5-9B-Q4_K_M.gguf --host 0.0.0.0 --port 11431 -ngl 30 -c 32768 -fa on -b 4096 -ub 2048 -rea off -ctk q8_0 -ctv q8_0 -a Qwen3.5-9B -mm mmproj-F16.gguf
```
## llama.cpp使用
- Web浏览器输入: http://localhost:8080
- API调用
	- 模型对话: http://localhost:8080/v1/chat/completions
	- 嵌入模型: http://localhost:8080/v1/embeddings
	- 重排模型: http://localhost:8080/v1/rerankings
- 内网电脑访问，localhost改成IP地址即可

## 参数说明
-m .gguf模型文件,需指定文件路径
-c 32768 上下文长度，默认256K，建议8k,16k,32k
-t  CPU线程数填物理核心数（4核8线程→填4或6）
-ngl  GPU卸载层数有独显才加，没显卡别写;根据显存和模型文件大小选择，可以从小往大试；99就是全卸载
--n-gpu-layers  同上
-rea off 关闭思考模式低速硬件必开，省一半时间
-fa  Flash Attention加速prompt处理，建议开('on', 'off', or 'auto', default: 'auto')
-a aliases set model name aliases, comma-separated (to be used by API)
-b  logical maximum batch size (default: 512)
-ub  physical maximum batch size (default: 512)
-ctk  KV Cache量化等级(q8_0, q4_0, q2_0)
-ctv KV Cache量化等级(q8_0, q4_0, q2_0)
-mm 视觉处理文件加载
--embedding 作为嵌入模型运行
--rerank 作为重排模型运行

[Tip]
> 运行电脑配置
> - CPU: Intel(R) Xeon(R) W-2223 CPU @ 3.60GHz   3.60 GHz
> - 内存: 64GB
> - GPU: NVIDIA Quadro RTX4000, 8GB
>
> 运行Qwen3.5-9B-Q4_K_M模型
> - 量化模型约5.6G
> - ngl设置35，可用完全卸载到显存，上下文设置32k，显存7.1GB，还有一些余量，,AI速度很快，约45-50 token/s
> - 用mm参数加载视觉处理文件，ngl改到30，显存占用7.6G，速度约为20~25 token/s，可用。
> 
> 如果需要内网调用模型，需要设置host参数为0.0.0.0
> 嵌入模型也可以运行，可再运行一个服务加载embedding模型，embedding参数设定，不用设置ngl，CPU运行即可


