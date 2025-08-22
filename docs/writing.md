# ✨ Writing Agents and Commands

Best practices and guidelines for creating effective Claude Code agents and custom commands.

## Writing Agents

Agents are specialised AI workers that handle complex, multi-step tasks autonomously. Each agent
should excel at a specific domain.

### Agent Structure

Every agent requires a Markdown file with YAML frontmatter:

```yaml
---
name: your-agent-name
description: Brief description and usage examples
tools: Glob, Grep, Read, Edit, MultiEdit, Write, TodoWrite, LS
model: haiku  # or opus for complex tasks
color: blue   # visual identifier in UI
---

# Agent Instructions

Detailed instructions for the agent's behaviour and methodology...
```

### Design Principles

#### 1. Single Responsibility

Each agent should excel at ONE type of task. Don't create Swiss Army knife agents.

**Good Example:**

- `python-ci-readiness` - Prepares Python code for CI/CD
- `docstring-auditor` - Fixes Python docstrings for compliance
- `test-coverage-analyst` - Analyses test coverage gaps

**Bad Example:**

- `python-everything` - Does linting, testing, documentation, and deployment

#### 2. Clear Activation Triggers

Include examples showing when the agent should be used:

```markdown
Use this agent when:
- You've finished implementing a feature and need QA
- The test suite is failing with type errors
- You want to ensure code meets team standards

Example prompts that trigger this agent:
- "Check my code is ready for CI"
- "Fix all the type errors"
- "Make sure this follows our standards"
```

#### 3. Comprehensive Instructions

Provide detailed, step-by-step instructions:

```markdown
## Process

1. **Discover test files**
   - Search for `test_*.py` and `*_test.py` files
   - Identify pytest configuration in `pyproject.toml` or `pytest.ini`

2. **Run initial test suite**
   - Execute `pytest` with coverage
   - Capture failures and errors
   - Note coverage percentage

3. **Fix type errors**
   - Run `mypy` on all source files
   - For each error:
     - Read the file context
     - Apply appropriate type hints
     - Verify the fix doesn't break tests
```

#### 4. Error Handling

Anticipate and handle common failure scenarios:

```markdown
## Error Handling

- **Missing dependencies**: Check pyproject.toml and requirements.txt
- **Import errors**: Verify PYTHONPATH and package structure
- **Permission denied**: Skip system files, report to user
- **Syntax errors**: Report location, don't attempt to fix
```

#### 5. Tool Selection

Only include tools the agent actually needs:

```yaml
# Minimal tools for a formatting agent
tools: Read, Edit, MultiEdit

# Full tools for a refactoring agent
tools: Glob, Grep, Read, Edit, MultiEdit, Write, TodoWrite, LS
```

### Model Selection Strategy

Choose the right model for the task complexity:

#### Haiku (Fast, Lightweight)

Perfect for deterministic, well-defined tasks:

- Code formatting and linting
- Simple find-and-replace operations
- Running predefined test suites
- Checking compliance with rules

```yaml
model: haiku
```

#### Opus (Sophisticated, Complex)

Essential for tasks requiring deep reasoning:

- Architectural decisions
- Complex refactoring
- Debugging subtle issues
- Understanding business logic

```yaml
model: opus
```

#### Mixed Workflows

Let Opus orchestrate whilst Haiku handles repetitive tasks:

```markdown
1. [Opus] Analyse architecture and plan refactoring
2. [Haiku] Apply formatting to all files
3. [Haiku] Update import statements
4. [Opus] Verify logic preservation
```

### Testing Your Agent

Before deploying an agent:

1. **Test on sample projects** - Run against different codebases
2. **Verify idempotency** - Running twice shouldn't break things
3. **Check edge cases** - Empty files, missing dependencies, etc.
4. **Monitor performance** - Ensure reasonable completion times
5. **Review output quality** - Check the agent's reports are useful

## Writing Commands

Commands are slash commands that configure Claude Code's behaviour for specific workflows.

### Command Structure

Create a Markdown file in `commands/` directory:

```markdown
# command-name.md

You are an expert at [specific domain]. When this command is active:

## Behaviour

- [Specific behaviour modification]
- [Another behaviour modification]

## Process

1. [First step in the workflow]
2. [Second step]
3. [Third step]

## Examples

When the user says: "Write tests for this function"
You should: [Specific action]
```

### Command Design Principles

#### 1. Single Purpose

Each command should enable ONE specific workflow:

**Good Examples:**

- `/write-docs` - Documentation writing mode
- `/review-security` - Security audit mode
- `/refactor-clean` - Clean code refactoring mode

**Bad Example:**

- `/do-everything` - Unclear, too broad

#### 2. Clear Instructions

Be explicit about behaviour changes:

```markdown
When this command is active:

## DO
- Write comprehensive docstrings for every public function
- Include usage examples in class docstrings
- Add type hints to all parameters

## DON'T
- Don't remove existing documentation
- Don't change code logic
- Don't add unnecessary comments
```

#### 3. Include Examples

Show typical usage scenarios:

```markdown
## Example Interactions

User: "Document this class"
Assistant: I'll add comprehensive Google-style docstrings with:
- Class purpose and usage
- Method documentation
- Usage examples
- Type hints

User: "This needs better docs"
Assistant: I'll enhance the existing documentation by:
- Expanding descriptions
- Adding parameter details
- Including return value information
```

#### 4. Handle Edge Cases

Anticipate what might go wrong:

```markdown
## Edge Cases

- **No existing docstrings**: Create from scratch
- **Conflicting styles**: Follow Google style
- **Generated code**: Add minimal documentation
- **Test files**: Use single-line docstrings
```

### Command Categories

Organise commands by their purpose:

#### Development Modes

- `/test-driven` - TDD workflow
- `/debug-mode` - Systematic debugging
- `/performance-opt` - Performance optimisation

#### Code Quality

- `/clean-code` - Refactoring for cleanliness
- `/security-review` - Security audit mode
- `/accessibility` - A11y improvements

#### Documentation

- `/write-docs` - Documentation mode
- `/api-docs` - API documentation
- `/user-guide` - User-facing docs

#### Specialised Workflows

- `/migration` - Code migration assistance
- `/review-pr` - Pull request review mode
- `/architecture` - Architectural decisions

## Naming Conventions

### Agents

Use descriptive, action-oriented names:

- Format: `{language/domain}-{action}-{target}`
- Examples: `python-ci-readiness`, `javascript-test-generator`, `sql-query-optimiser`

### Commands

Use concise, memorable names:

- Format: `{action}-{target}` or `{mode}`
- Examples: `/write-docs`, `/debug-mode`, `/clean-code`

## Documentation Standards

### For Agents

Document:

1. **Purpose** - What the agent does
2. **When to use** - Clear triggers
3. **Process** - Step-by-step workflow
4. **Limitations** - What it won't do
5. **Output** - What to expect

### For Commands

Document:

1. **Mode** - How behaviour changes
2. **Workflow** - The process to follow
3. **Examples** - Common interactions
4. **Defaults** - Standard assumptions

## Contributing

When contributing new agents or commands:

1. **Check existing tools** - Avoid duplication
2. **Follow conventions** - Match the existing style
3. **Test thoroughly** - Verify on multiple scenarios
4. **Document clearly** - Help others understand
5. **Consider maintenance** - Keep it simple

## Advanced Patterns

### Composable Agents

Design agents that work well together:

```markdown
This agent works well with:
- `test-coverage-analyst` - Run after to verify coverage
- `docstring-auditor` - Run before to document code
- `performance-profiler` - Run after optimisations
```

### Progressive Enhancement

Start simple, add complexity gradually:

```markdown
## Basic Mode
- Format code
- Fix obvious issues

## Advanced Mode (--thorough flag)
- Deep refactoring
- Architecture improvements
- Performance optimisation
```

### Feedback Loops

Include verification steps:

```markdown
After each change:
1. Run tests to verify
2. Check type hints still valid
3. Ensure imports resolve
4. Report any regressions
```
