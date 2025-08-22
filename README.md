# 🚀 Claude Code Files

**Centralised configuration for Claude Code CLI - agents, commands, and global instructions.**

A unified repository for managing Claude Code customisations across machines.

## 📁 What's Included

### Agents (`agents/`)

Specialised AI workers that handle complex, multi-step tasks autonomously. Each has a specific
focus area and toolset—quality assurance pipelines, codebase searches, refactoring operations.

**Strategic Model Selection**: Configure different models per agent based on complexity. Use Opus
for sophisticated orchestration and architecture, Haiku for rapid tasks like linting and
formatting.

### Commands (`commands/`)

Slash commands that trigger specific behaviours in your CLI coding assistant. Type `/` followed by
a command name to execute pre-configured instructions.

### CLAUDE.md

Global instructions that apply to all Claude Code sessions:

- User preferences and context
- Code quality standards
- Documentation requirements
- Git workflow preferences
- Language and style guidelines
- Problem-solving approaches

The `.gitignore` is configured to track ONLY these three items, allowing this repo to coexist with
other Claude Code files without interference.

## 🔧 Compatibility

Designed for Claude Code and CLI coding assistants that support:

- YAML-formatted Markdown agent definitions
- Markdown-based slash commands
- Global instruction files

**Note:** As of the time of writing, the Gemini CLI does not have similar agent or slash command
features.

## 📦 Getting Started

### Quick Start

1. Fork this repository
2. Clone to `~/.claude/`
3. Customise your configurations
4. Commit and push your changes

**→ [Full setup guide](docs/setup.md)** - Detailed instructions for new and existing users

## 📚 Usage Examples

### Using an Agent

```plain
User: "I've finished implementing the new authentication module. Can you make sure it's ready for CI?"
Assistant: "I'll use the python-ci-readiness agent to run through the complete quality assurance process."
[Agent runs pytest, mypy, ruff check, and ruff format, fixing issues along the way]
```

### Using a Command

```plain
User: /write-docs
Assistant: Documentation mode activated. I'll focus on creating comprehensive, well-structured documentation.
```

### Creating a New Agent

```yaml
# agents/my-agent.md
---
name: my-agent
description: What this agent does
tools: Read, Edit, MultiEdit
model: haiku
---

# Instructions
Detailed behaviour...
```

### Creating a New Command

```markdown
# commands/my-command.md
You are an expert at [task]. When invoked:
1. [Behaviour changes]
2. [Workflow steps]
```

## 📖 Documentation

- **[Setup Guide](docs/setup.md)** - Installation and migration for new and existing users
- **[Writing Guide](docs/writing.md)** - Best practices for creating effective agents and commands

## 📄 License

This project is licensed under the [Apache License 2.0](LICENSE).
