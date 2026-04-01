# Unlimited Local Vibe Coding on Linux

Relying solely on a paid LLM with token quotas means accepting that you can be **interrupted** in the middle of a critical project change.  
Some developers have already been blocked with a broken build or an incomplete critical modification because of quota limits.

The power of cloud LLMs is no longer in question — they deliver the best results available today.  
However, having a local fallback strategy provides continuity and prevents these kinds of blocking situations.

Beyond resilience, a hybrid Cloud LLM / Local LLM approach also enables smarter FinOps optimization:  
reserve premium models for high-value tasks (reasoning, architecture), and use local models for repetitive tasks, iterations, or testing phases.

<em>This method can be adapted to Windows with minimal effort (manual installation of the same components).</em>

---

# TL;DR

Run a free local model using Ollama with Python 3.12, and install **Aider** to get a workflow similar to Claude Code.

---

## Introduction

**Ollama** allows you to run LLMs locally on your machine.  
In this guide, we use the open-weight model **Qwen**, developed by Alibaba.

**Aider** is a CLI-based AI coding assistant that lets you work directly inside your terminal with your codebase.

You can choose different model sizes depending on your hardware:
- 7B → laptop-friendly
- 14B / 32B → requires a more powerful GPU

For a smooth coding experience, we will use **qwen2.5-coder:7b**.

---

## Prerequisites

Make sure you have the required build tools installed.

Python 3.12 is widely compatible with current AI tooling at the time of writing.

If you're running a newer version (e.g., Fedora 43 with Python 3.14), it is recommended to isolate Python 3.12 to avoid compatibility issues.

```bash
sudo dnf install python3 python3-pip
python3 --version
sudo dnf groupinstall "Development Tools"
sudo dnf install python3.12 python3.12-devel gcc gcc-c++
```

## Install Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```
## Install Aider
On Fedora 43+, Python 3.14 may cause build issues with older numpy versions.
The most reliable approach is to isolate the installation using pipx.

This avoids managing multiple .venv environments.
```bash
sudo dnf install pipx
pipx ensurepath
pipx install aider-chat --python /usr/bin/python3.12
```
This is where things get clean: by targeting Python 3.12 explicitly, pipx handles isolation automatically.

## Git Repository
If not already done, initialize Git in your project:
```bash
git init
git config --global init.defaultBranch main
git config user.name "your name"
git config user.email "you@example.com"
```
If you skip this step, Aider will initialize a repository automatically.
However, for publishing your work, you will need proper Git configuration anyway.

Aider is a Git-native agent: without a repository, you lose key features (undo, auto-commits).

## Configure Ollama
1. Run your model
```bash
ollama run qwen2.5-coder:7b
```
2. Set API access
Aider needs access to the Ollama API. Add this to your ~/.bashrc or ~/.zshrc:
```bash
export OLLAMA_API_BASE=http://localhost:11434
```
## Run Aider with Qwen
In a new terminal, inside your Git project:
```bash
aider --model ollama_chat/qwen2.5-coder:7b
```
Evolution
For better performance, you can use Codestral by Mistral AI (22B model optimized for code):
```bash
ollama pull codestral && ollama run codestral 
aider --model ollama_chat/codestral
```

# Troubleshooting
## Invalid model name
If during the inférence, you see `litellm.APIConnectionError: Ollama_chatException - {"error":"invalid model name"}` 

Retrieve the name that Ollama understand, to run it :
`ollama list`

Example
```bash
ollama list | grep qwen
qwen2.5-coder:latest      dae161e27b0e    4.7 GB    About an hour ago    
qwen2.5:7b                845dbda0ea48    4.7 GB    2 days ago

aider --model ollama_chat/qwen2.5-coder:latest
```

## Model not responding
Check if Ollama is running:
`curl http://localhost:11434`
If not:
`sudo systemctl start ollama`
## Python 3.12 not found
Check path:
`which python3.12`
If different:
`pipx install aider-chat --python /path/to/python3.12 --force`
## ImpImporter error
Error:
`AttributeError: module 'pkgutil' has no attribute 'ImpImporter'`
Root cause: Python 3.14 removed deprecated APIs used by older packages.
Fix: 
`pipx inject aider-chat "numpy>=1.26.0"`
# License
MIT License — free to use and modify for the open source community.

# References
My LinkedIn : [Julien P.](https://www.linkedin.com/in/julien-p-68834731/?locale=en)

[Ollama](https://ollama.com/)

[Qwen2.5 coder](https://ollama.com/library/qwen2.5-coder)

[Mistral AI Codestral](https://mistral.ai/news/codestral)

[Aider](https://aider.chat/#getting-started)
