*2025-10-10*

# Claude code + LM Studio搭建本地模型驱动命令行Agent

## 概念

### Claude Code (AI Agent)
Claude Code 是 Anthropic 推出的命令行工具，能够让 Claude 直接在你的本地环境中执行各种开发任务。它可以读写文件、运行代码、使用开发者工具，让你通过自然语言与编码环境进行交互。

### LM Studio
LM Studio 是一个本地大语言模型运行环境，支持在本地计算机上运行各种开源大语言模型（如 llama-2、Code Llama、Mixtral 等）。它提供了友好的图形界面来管理和运行本地模型，无需依赖云端服务。

## 下载安装

### 1. Claude Code 安装
```bash
npm install -g @anthropic-ai/claude-code
```

### 2. LM Studio 安装
- 访问 [LM Studio 官网](https://lmstudio.ai/)
- 下载对应操作系统的安装包
- 按照安装向导完成安装

### 3. 模型选择和下载
在 LM Studio 中：
1. 打开 LM Studio 应用
2. 点击左侧的 "Discover" 或 "My Models"
3. 搜索并下载适合的 Code 模型，推荐：
   - **Code Llama** (专为代码生成优化)
   - **WizardCoder** (强大的代码能力)
   - **DeepSeek-Coder** (优秀的代码模型)
4. 确保选择与你硬件配置相匹配的模型大小（7B、13B、34B等）

## 配置

### 1. 启动 LM Studio 本地服务器

1. 打开 LM Studio
2. 选择已下载的模型
3. 点击 "Start Server" 或 "本地服务器"
4. 记下服务器地址和端口（通常是 `http://localhost:1234`）

### 2. 配置 Claude Code 使用本地模型

创建或编辑 Claude Code 的配置文件：

```bash
# 创建配置文件目录
mkdir -p ~/.config/claude

# 编辑配置文件
nano ~/.config/claude/config.json
```

在配置文件中添加：

```json
{
  "anthropic\": {
    \"base_url\": \"http://localhost:1234/v1\",
    \"api_key\": \"lm-studio\",
    \"model\": \"codellama\"
  }
}
```

或者使用环境变量：

```bash
export ANTHROPIC_API_KEY="lm-studio"
export ANTHROPIC_BASE_URL="http://localhost:1234/v1"
```

### 3. 验证配置

运行以下命令测试连接：

```bash
claude "hello world test"
```

## 使用

### 基本用法

配置完成后，你可以像平常一样使用 Claude Code 命令：

```bash
# 让 Claude 帮你分析代码
claude "请分析当前目录下的 main.py 文件"

# 让 Claude 帮你写测试
claude "为 utils.py 中的函数编写单元测试"

# 让 Claude 帮你重构代码
claude "重构这个函数，使其更易读"

# 让 Claude 帮你 debug
claude "为什么这个脚本运行时报错？"
```

### 进阶用法

1. **集成开发环境**
   - 在 VS Code 中集成 Claude Code
   - 使用快捷键快速调用

2. **工作流优化**
   - 创建常用命令的别名
   - 结合 git 工作流
   - 自动化重复性任务

3. **性能优化**
   - 选择合适的模型大小平衡速度和准确性
   - 使用量化模型提高运行速度
   - 合理设置 GPU 内存使用

### 注意事项

1. **模型限制**：本地模型可能没有 Claude 3 那么强大的推理能力
2. **硬件要求**：运行大模型需要足够的内存和 GPU
3. **响应速度**：本地运行可能比云端服务慢
4. **模型选择**：不同模型的代码能力差异较大，建议多尝试几个

### 常见问题

**Q: Claude Code 无法连接到 LM Studio?**
A: 请检查：
   - LM Studio 服务器是否已启动
   - 配置文件中的 URL 和端口是否正确
   - 防火墙是否阻止了连接

**Q: 本地模型效果不如 Claude 3?**
A: 这是正常现象，可以通过：
   - 选择更好的代码专用模型
   - 使用较大的模型参数
   - 优化提示词（prompt）

**Q: 运行速度很慢?**
A: 建议：
   - 使用 GPU 加速
   - 选择量化版本模型
   - 减小模型参数规模

## 参考

[https://zhuanlan.zhihu.com/p/1897253910349083711](https://zhuanlan.zhihu.com/p/1897253910349083711)

[https://zhuanlan.zhihu.com/p/1945133468855046287](https://zhuanlan.zhihu.com/p/1945133468855046287)

[https://blog.csdn.net/2402_84494441/article/details/150385229](https://blog.csdn.net/2402_84494441/article/details/150385229)