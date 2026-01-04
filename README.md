# CTF AutoRe Bench

本项目旨在构建一个**探索性的综合评估框架**，用于测试和分析大语言模型（LLM）在全自动解决 CTF 逆向工程（Reverse Engineering）赛题方面的能力。

## 🎯 研究目标

本项目致力于通过实验回答以下核心问题：

1. **基准测试**：目前主流 LLM 实现全自动逆向的能力边界在哪里？（涵盖解题耗时、成功率及 API 成本评估）。
2. **上下文策略对比**：
   - **Tool Use 模式**：通过调度 IDA MCP (Model Context Protocol) 工具按需获取信息。
   - **Context Dump 模式**：一次性将所有反编译结果、内存快照、符号表注入上下文。
   - *对比两者在解题效果上的优劣。*
3. **反编译质量影响**：
   - 反编译代码（C/C++伪代码）相比纯汇编代码对 LLM 的辅助作用有多大？
   - 不同的反编译引擎对模型理解是否有显著差异？
4. **动静态分析对比**：
   - 动态调试相比静态分析能否为 LLM 提供有效增益？
   - LLM 目前是否具备有效控制动态调试流程的能力？

## 🛠️ 环境依赖

本项目已在以下环境中测试通过：

- **OS**: Windows 10
- **Python**: 3.12
- **IDA Pro**: 9.0 SP1
- **API**: DeepSeek-V3.2 (API model: deepseek-reasoner) 

## 🚀 快速开始

### 1. 预配置

在运行之前，请确保已配置环境变量 `DEEPSEEK_API_KEY`：

PowerShell

```
# Windows PowerShell 示例
$env:DEEPSEEK_API_KEY="sk-your-key-here"
```

> **提示**：请确保您的 DeepSeek API 账户余额充足，以免测试中断。

### 2. 安装依赖

Bash

```
pip install -r requirements.txt
```

### 3. 配置 idalib

请参考 Hex-Rays 官方文档配置 `idalib`： 🔗 [IDA Lib User Guide](https://docs.hex-rays.com/user-guide/idalib)

> **故障排除**：如果安装 `idalib` 后遇到问题，请优先检查报错信息，并核对对应路径下的 `ida-config.json` 配置文件是否正确。

### 4. 启动测试

测试需要启动两个终端窗口配合运行。

**终端 A (启动 MCP Server)**： 启动 IDA 的 headless 模式作为服务端。

Bash

```
uv run idalib-mcp --host 127.0.0.1 --port 8745 misc/EasyVM.exe
```

**终端 B (启动 Client/Agent)**： 运行测试脚本。

Bash

```
python ida_agent_test.py
```

## ⚠️ 安全与免责声明

> [!WARNING] **高风险警告：无沙盒环境** 本项目中的 Python 执行接口**目前没有接入任何沙盒环境**。 `ida_agent_test.py` 会直接在您的宿主机上执行代码。请**绝对不要**在生产环境或含有敏感数据的机器上运行不可信的二进制文件或模型生成的代码。

- `ida_agent_test.py` 目前主要用于测试静态分析工具的调用。
- 本项目仅供安全研究与学术交流使用。

## 📂 测试用例

- **入门测试**：`misc/main.exe` (较为简单)
  - Flag: `flag{f2e6c420-5d8f-4a9e-8ecd-c08c1a5f8238}`
- **进阶测试**：`misc/EasyVM.exe`

------

*该 README 由 Gemini 3 pro润色整理*

