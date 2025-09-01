# 🚀 Claude Code Files

**Centralised configuration for Claude Code CLI - agents, commands, and global instructions.**

Keep your Claude Code setup consistent across all your machines by storing your personal
configuration in Git.

## 📁 What's included

### Agents (`agents/`)

Specialised agents that handle specific tasks autonomously. You might have one for getting code
ready for CI/CD, another for auditing docstrings, or one that hunts down test coverage gaps. Each
agent knows exactly what it's good at and has the right tools for the job.

You can pick which model each agent uses - Opus when you need serious thinking power for
architectural work, or Haiku when you just need something done quickly like formatting or linting.

### Commands (`commands/`)

Slash commands that change how Claude Code behaves. Type something like `/write-docs` or
`/debug-mode` to switch into a specific working mode with its own rules and priorities.

### CLAUDE.md

Your personal instruction manual for Claude Code. This is where you tell it your preferences - how
you like your code formatted, what your documentation should look like, whether you prefer quick
hacks or proper solutions. It applies to every Claude Code session on that machine.

The `.gitignore` only tracks these three things, so this repo plays nicely with your other Claude
Code files.

## 🔧 Compatibility

Works with Claude Code and any CLI coding assistant that understands YAML-formatted agent files,
Markdown slash commands, and global instruction files.

The Gemini CLI doesn't support agents or slash commands yet.

## 📖 Documentation

- **[Setup guide](docs/setup.md)** - How to get this working on your machine
- **[Writing guide](docs/writing.md)** - How to create your own agents and commands

## 📄 License

This project is licensed under the [Apache License 2.0](LICENSE).
