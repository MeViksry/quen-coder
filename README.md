# ⬡ QuenCoder CLI

> Multi-provider AI coding assistant — like Claude Code, but open to **any** AI.

```
  ██████╗ ██╗   ██╗███████╗███╗   ██╗
 ██╔═══██╗██║   ██║██╔════╝████╗  ██║
 ██║   ██║██║   ██║█████╗  ██╔██╗ ██║
 ██║▄▄ ██║██║   ██║██╔══╝  ██║╚██╗██║
 ╚██████╔╝╚██████╔╝███████╗██║ ╚████║
  ╚══▀▀═╝  ╚═════╝ ╚══════╝╚═╝  ╚═══╝
```

[![Go](https://img.shields.io/badge/Go-1.21+-00ADD8?logo=go)](https://go.dev)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

## Features

- 🤖 **Multi-provider** — Anthropic Claude, OpenAI GPT, Google Gemini, Groq, Ollama (local), Together AI, Mistral
- 🛠️ **Agentic tools** — Read/write files, run bash commands, search codebases, grep, find
- ⚡ **Streaming** — Real-time streaming output from all providers
- 💬 **Interactive REPL** — Full readline support with history, autocomplete, and session commands
- 🔒 **Safe** — Confirms bash commands before execution
- 📦 **Zero config** — Works immediately after setting an API key

---

## Installation

### Quick Install (recommended)

```bash
curl -fsSL https://raw.githubusercontent.com/quencoder/quen/main/install.sh | bash
```

### Manual Install

Download the latest binary for your platform from [Releases](https://github.com/quencoder/quen/releases):

```bash
# Linux (amd64)
curl -fsSL https://github.com/quencoder/quen/releases/latest/download/quen_linux_amd64.tar.gz | tar xz
sudo mv quen /usr/local/bin/

# macOS (Apple Silicon)
curl -fsSL https://github.com/quencoder/quen/releases/latest/download/quen_darwin_arm64.tar.gz | tar xz
sudo mv quen /usr/local/bin/
```

### Build from Source

```bash
git clone https://github.com/quencoder/quen.git
cd quen
make install
```

---

## Quick Start

```bash
# 1. Run setup wizard
quen init

# 2. Or set your API key directly
quen config set-key anthropic sk-ant-...

# 3. Start coding!
quen                              # interactive session
quen ask "Write a REST API in Go" # one-shot
```

---

## Supported Providers

| Provider | Models | API Key |
|----------|--------|---------|
| **Anthropic** | Claude Opus, Sonnet, Haiku | [console.anthropic.com](https://console.anthropic.com) |
| **OpenAI** | GPT-4o, O1, GPT-4 Turbo | [platform.openai.com](https://platform.openai.com) |
| **Google Gemini** | Gemini 2.0 Flash/Pro, 1.5 | [aistudio.google.com](https://aistudio.google.com) |
| **Groq** | Llama 3.3, Mixtral | [console.groq.com](https://console.groq.com) |
| **Ollama** | Any local model | No key needed |
| **Together AI** | Llama 3, CodeLlama | [api.together.xyz](https://api.together.xyz) |
| **Mistral** | Mistral Large, Codestral | [console.mistral.ai](https://console.mistral.ai) |

---

## Usage

### Interactive Mode

```bash
quen           # Start REPL with default provider
quen chat      # Same as above
```

**In-session commands:**

| Command | Description |
|---------|-------------|
| `/help` | Show help |
| `/exit` | Exit QuenCoder |
| `/clear` | Clear conversation history |
| `/model <name>` | Switch model |
| `/provider <name>` | Show providers |
| `/tools on\|off` | Toggle tool use |
| `/save <file>` | Save conversation |
| `/load <file>` | Load conversation |
| `/system <prompt>` | Set system prompt |
| `/status` | Show current config |
| `/tokens` | Show token usage |

### One-Shot Mode

```bash
# Ask a question
quen ask "How do I reverse a string in Go?"

# With stdin (pipe)
cat main.go | quen ask "Find bugs in this code"
echo "error: nil pointer" | quen ask "What does this mean?"

# Include a file
quen ask "Refactor this" --file main.go

# Override provider/model
quen ask "Write unit tests" --provider openai --model gpt-4o
```

### Configuration

```bash
# Show all settings
quen config

# Switch provider
quen config set provider openai
quen config set provider gemini
quen config set provider ollama

# Set API keys
quen config set-key anthropic sk-ant-...
quen config set-key openai sk-...
quen config set-key gemini AIza...
quen config set-key groq gsk_...

# Change model
quen config set model claude-sonnet-4-5
quen config set model gpt-4o-mini

# Tweaks
quen config set stream true
quen config set confirm-bash false
quen config set temperature 0.3
quen config set max-tokens 4096

# Show config file path
quen config path
```

### List Models

```bash
quen models             # All providers
quen models anthropic   # Specific provider
quen models openai ollama
```

---

## Tools (Agentic Capabilities)

QuenCoder can use tools to interact with your codebase:

| Tool | Description |
|------|-------------|
| `read_file` | Read file contents (with line ranges) |
| `write_file` | Write/append to files |
| `run_bash` | Execute shell commands |
| `list_directory` | List files (recursive) |
| `search_files` | Grep pattern in files |
| `find_files` | Find files by name pattern |
| `create_directory` | Create directories |
| `delete_file` | Delete files |
| `get_file_info` | File metadata |

Tools are enabled by default. Toggle with `/tools off` or `--no-tools`.

---

## Project Instructions

Create a `.queninstructions` file in your project root to give QuenCoder project-specific context:

```markdown
# My Project Instructions

This is a Go REST API using the Gin framework.
Always use `go fmt` before writing files.
Tests are in `*_test.go` files.
Run `make test` to execute tests.
```

QuenCoder will automatically load this file when you start in that directory.

---

## Config File

Located at `~/.quen/config.yaml`:

```yaml
active_provider: anthropic

providers:
  anthropic:
    api_key: "sk-ant-..."
    model: claude-opus-4-5
  openai:
    api_key: "sk-..."
    model: gpt-4o
  gemini:
    api_key: "AIza..."
    model: gemini-2.0-flash
  groq:
    api_key: "gsk_..."
    model: llama-3.3-70b-versatile
  ollama:
    base_url: http://localhost:11434
    model: llama3.2

max_tokens: 8192
temperature: 0.7
stream_output: true
confirm_bash: true
system_prompt: |
  You are QuenCoder, an expert AI coding assistant.
```

---

## Examples

```bash
# Debug an error
cat error.log | quen ask "What's causing this and how do I fix it?"

# Generate a full feature
quen ask "Create a JWT authentication middleware in Go using the Gin framework"

# Code review
quen ask "Review this PR for security issues" --file diff.patch

# Refactor
quen ask "Refactor this to use the repository pattern" --file service.go

# Tests
quen ask "Write comprehensive unit tests for all exported functions" --file math.go

# Documentation
quen ask "Generate godoc comments for all functions" --file api.go

# Use local Ollama (no API key needed)
quen ask "Explain this algorithm" --provider ollama
```

---

## Building

```bash
# Build for current platform
make build

# Build and install
make install

# Build all platforms (for release)
make release

# Run tests
make test
```

---

## License

MIT © QuenCoder Contributors
