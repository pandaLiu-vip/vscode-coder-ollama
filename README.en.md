# Vscode Codex Ollama

📖 Read in other languages / 阅读其他语言:
[中文](README.zh.md) | [English](README.en.md)

`Vscode Codex Ollama` is a VS Code extension designed to provide a local agent experience close to the Codex extension workflow, while all model calls are routed entirely through your local Ollama service.

It currently supports:

- Reading the list of locally installed Ollama models
- Starting tasks from a Codex-style threaded interface in the sidebar
- Reading, creating, modifying, and deleting files in the current project through tool calls
- Strictly limiting file operations to the currently opened VS Code workspace
- Adding files and selections as context to the current thread

## Feature Overview

- `Codex Ollama` Sidebar
  The sidebar includes a thread list, message stream, model selector, context attachments area, and Composer input area.
- `Ollama Model Sync`
  The extension retrieves installed models through the local Ollama API and lets you switch between them in the UI.
- `Workspace Sandbox`
  The agent's file tools only support relative paths within the current workspace and cannot escape the project directory.
- `Inject Files/Selections into Thread`
  You can add the current file or the editor selection to the thread so the model can read the context before modifying code.

## Requirements

1. Ollama is installed and running
2. At least one model has been pulled locally, for example:

```bash
ollama pull qwen2.5-coder:7b
```

3. VS Code version `1.120.0` or later

## Default Ollama Endpoint

The extension connects to the following endpoint by default:

```text
http://127.0.0.1:11434/api
```

The following API routes are used:

- `GET /api/tags`
- `POST /api/chat`

If your Ollama address is different, you can change it in the settings.

## Installation and Debugging

```bash
npm install
npm run compile
```

Press `F5` in VS Code to launch the Extension Development Host.

After opening the project:

1. Open `Codex Ollama` from the activity bar on the left
2. Click "Refresh Models"
3. Select a local model
4. Describe your task in the input box and send it

## Commands

- `Open Codex Ollama Sidebar`
- `New Codex Ollama Thread`
- `Refresh Ollama Models`
- `Add File to Codex Ollama Thread`
- `Add Selection to Codex Ollama Thread`

## Settings

- `vscodeCoderOllama.baseUrl`
  Ollama API address, default: `http://127.0.0.1:11434/api`
- `vscodeCoderOllama.defaultModel`
  Default model name. If left empty, the first installed model is selected automatically.
- `vscodeCoderOllama.requestTimeoutMs`
  Ollama request timeout
- `vscodeCoderOllama.maxToolIterations`
  Maximum number of tool iterations for a single task
- `vscodeCoderOllama.showThinking`
  Whether to display the thinking text returned by the model

## Permission Boundaries

This extension enforces workspace-level permission limits as requested:

- It can read files inside the current workspace
- It can create, modify, and delete files and directories inside the current workspace
- It cannot access paths outside the workspace
- Any `../`, absolute path, or other path that attempts to escape the current project will be rejected

## Notes on the Current Implementation

This version is already a runnable MVP, but it still differs from the official Codex extension in several ways:

- The UI style is close to Codex, but it is not a 1:1 replica
- The current thread UI is built with Webview rather than a deep integration with the official `chatSessions`
- The current toolset focuses on the file system and does not yet include advanced capabilities such as terminal execution, patch previews, or approval flows
- Whether tool calls work reliably still depends on whether the Ollama model you choose supports tool calling well

## Known Limitations

- It works best with Ollama models that support tool calling and coding tasks
- Binary files are currently not recommended for agent modification
- `write_file` currently writes by fully overwriting the file instead of applying chunked patches

## Development

```bash
npm run lint
npm run compile
npm run compile-tests
npm test
```
