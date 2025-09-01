I need you to help me set up pre-commit hooks for this project. Check for an existing
`.pre-commit-config.yaml` file, analyse the codebase to suggest appropriate checks, and
configure everything using `uv` and `prek`.

## Step-by-step process

### 1. Check existing setup

First check if `.pre-commit-config.yaml` exists. If it does, show me what's already configured
and ask if I want to:

- Add more checks based on the codebase
- Reorganise it for efficiency (quick file checks first, language checks in middle, commit checks last)
- Fix any incorrect arguments or outdated hook versions
- Leave it as is

### 2. Analyse the codebase

Use Glob and Read to quickly identify what types of files are present:

- Python: `**/*.py`, `pyproject.toml`, `setup.py`
- JavaScript/TypeScript: `**/*.{js,jsx,ts,tsx}`, `package.json`
- Rust: `**/*.rs`, `Cargo.toml`
- Go: `**/*.go`, `go.mod`
- Documentation: `**/*.md`

### 3. Install prerequisites

```bash
# Check if uv is installed
command -v uv || curl -LsSf https://astral.sh/uv/install.sh | sh

# Update if already installed
uv self update

# Install prek, the fast Rust alternative to pre-commit
uv tool install prek
```

### 4. Build configuration

Based on what you find, create a `.pre-commit-config.yaml`. Here's the comprehensive template
to select from:

```yaml
default_install_hook_types:
  - pre-commit
  - commit-msg
default_stages:
  - pre-commit
  - manual

repos:
  # File integrity and format checks - include these for all projects
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v5.0.0
    hooks:
      - id: check-added-large-files
      - id: check-case-conflict
      - id: check-json
      - id: check-merge-conflict
      - id: check-symlinks
      - id: check-toml
      - id: check-xml
      - id: check-yaml
      - id: destroyed-symlinks
      - id: end-of-file-fixer
      - id: fix-byte-order-marker
      - id: mixed-line-ending
      - id: trailing-whitespace
      # Python-specific from this repo (only if Python detected)
      - id: check-ast
      - id: check-docstring-first
      - id: debug-statements

  # Python local hooks (if Python project)
  - repo: local
    hooks:
      - id: ruff-check
        name: ruff check
        entry: uvx ruff check --fix
        language: system
        types: [python]

      - id: ruff-format
        name: ruff format
        entry: uvx ruff format
        language: system
        types: [python]

      - id: mypy
        name: mypy
        entry: uv run mypy .
        language: system
        types: [python]
        require_serial: true
        pass_filenames: false

      - id: pytest
        name: pytest
        entry: uv run pytest -x --tb=short -q
        language: system
        types: [python]
        require_serial: true
        pass_filenames: false

  # JavaScript/TypeScript local hooks (if JS/TS project)
  - repo: local
    hooks:
      - id: prettier
        name: prettier
        entry: npx prettier --write
        language: system
        types_or: [javascript, jsx, typescript, tsx, json, yaml, markdown]

      - id: eslint
        name: eslint
        entry: npx eslint --fix
        language: system
        types: [javascript, jsx, typescript, tsx]

  # Rust local hooks (if Rust project)
  - repo: local
    hooks:
      - id: cargo-fmt
        name: cargo fmt
        entry: cargo fmt
        language: system
        types: [rust]
        pass_filenames: false

      - id: cargo-clippy
        name: cargo clippy
        entry: cargo clippy --fix --allow-staged
        language: system
        types: [rust]
        pass_filenames: false

  # Markdown (if .md files exist)
  - repo: https://github.com/DavidAnson/markdownlint-cli2
    rev: v0.18.1
    hooks:
      - id: markdownlint-cli2
        args: ["--fix"]

  # Conventional commits - always include
  - repo: https://github.com/compilerla/conventional-pre-commit
    rev: v4.2.0
    hooks:
      - id: conventional-pre-commit
        stages: [commit-msg]
        args: ["--strict", "--force-scope", "--verbose"]
```

### 5. Ask for confirmation

Before creating the file, tell me what hooks you'll include based on the codebase analysis:

"I found [languages/tools] in your project. I'll set up pre-commit with:

- Standard file checks (trailing whitespace, YAML/JSON validation, etc.)
- Typo checking
- Conventional commit messages
- [Language-specific tools]

Should I also include any custom commands as hooks?"

### 6. Install and test

After creating the config:

```bash
# Test for errors
prek validate-config

# Install the hooks
prek install

# Update hook versions if needed
prek autoupdate

# Test on all files
prek run --all-files
```

If any hooks make changes (they auto-fix by default), summarise what they've changed and offer
to check the `git diff` and `git status` to stage those specific changed files before committing.

## Key points to remember

- Prek is a Rust alternative to pre-commit from <https://github.com/j178/prek> (it's faster)
- Most hooks should auto-fix issues, not just check (no `--check` flags unless necessary)
- Fixed files are left unstaged so they can be reviewed before committing
- Only suggest hooks for languages/tools actually present in the codebase
- Local hooks use `language: system` and run with project tools (uv, npm, cargo)
- Don't overwhelm with every possible hook - include essentials based on what's there

Remember to actually check what's in the codebase first, don't assume.
Use Glob to find file types, then build an appropriate configuration from the template above.
