# Vscode Codex Ollama

📖 阅读其他语言 / Read in other languages:
[中文](README.zh.md) | [English](README.en.md)

---

`Vscode Codex Ollama` 是一个 VS Code 插件，目标是提供接近 Codex 插件工作流的本地 Agent 体验，但模型调用完全走你本机的 Ollama 服务。

它目前支持：

- 读取本地 Ollama 已安装模型列表
- 在侧边栏里用 Codex 风格线程界面发起任务
- 通过工具调用读取、创建、修改、删除当前项目内的文件
- 严格限制文件操作范围，只允许当前打开的 VS Code 工作区
- 把文件和选区作为上下文加入当前线程

## 功能概览

- `Codex Ollama` 侧边栏
  侧边栏包含线程列表、消息流、模型选择器、上下文附件区和 Composer 输入区。
- `Ollama 模型同步`
  插件通过本地 Ollama API 获取已安装模型，并允许在界面中切换。
- `工作区沙箱`
  Agent 的文件工具只支持当前工作区内的相对路径，不能跳出项目目录。
- `文件/选区注入线程`
  可以把当前文件或编辑器选区加入线程，帮助模型先读上下文再改代码。

## 运行要求

1. 已安装并启动 [Ollama](https://ollama.com/)
2. 本机已拉取至少一个模型，例如：

```bash
ollama pull qwen2.5-coder:7b
```

3. VS Code 版本 `1.120.0` 或更高

## 默认 Ollama 接口

插件默认连接：

```text
http://127.0.0.1:11434/api
```

使用的接口包括：

- `GET /api/tags`
- `POST /api/chat`

如果你的 Ollama 地址不同，可以在设置里改。

## 安装与调试

```bash
npm install
npm run compile
```

在 VS Code 中按 `F5` 启动扩展开发宿主。

打开项目后：

1. 左侧活动栏打开 `Codex Ollama`
2. 点击“刷新模型”
3. 选择一个本地模型
4. 在输入框里描述任务并发送

## 命令

- `Open Codex Ollama Sidebar`
- `New Codex Ollama Thread`
- `Refresh Ollama Models`
- `Add File to Codex Ollama Thread`
- `Add Selection to Codex Ollama Thread`

## 设置项

- `vscodeCoderOllama.baseUrl`
  Ollama API 地址，默认 `http://127.0.0.1:11434/api`
- `vscodeCoderOllama.defaultModel`
  默认模型名，留空时自动选择第一个已安装模型
- `vscodeCoderOllama.requestTimeoutMs`
  Ollama 请求超时时间
- `vscodeCoderOllama.maxToolIterations`
  单次任务最多工具迭代次数
- `vscodeCoderOllama.showThinking`
  是否展示模型返回的 thinking 文本

## 权限边界

这个插件按你的要求做了工作区级权限限制：

- 可以读取当前工作区内文件
- 可以创建、修改、删除当前工作区内文件和目录
- 不允许访问工作区外路径
- 传入 `../`、绝对路径、或任何试图跳出当前项目的路径都会被拒绝

## 当前实现说明

这个版本已经是可运行的 MVP，但和官方 Codex 插件相比仍有差距：

- 界面风格接近 Codex，但不是 1:1 复刻
- 目前是基于 Webview 自建线程界面，不是官方 chatSessions 深度集成
- 工具集当前聚焦文件系统，没有终端执行、补丁预览、审批流等高级能力
- 是否能稳定进行工具调用，还取决于你选择的 Ollama 模型是否支持较好的 tool calling

## 已知限制

- 最适合使用支持工具调用和代码任务的 Ollama 模型
- 二进制文件目前不建议交给 Agent 修改
- `write_file` 目前按“整文件覆盖”写入，不是逐块 patch

## 开发

```bash
npm run lint
npm run compile
npm run compile-tests
npm test
```
