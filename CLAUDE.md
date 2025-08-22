# Global Instructions for Claude

Essential guidance for working with projects on this machine. Tom can update it to improve
workflow and content style preferences.

## User Context

- **Name**: Tom Foster
- **Focus**: Clean, professional code that's easy to read and maintain
- **Communication**: Natural style with genuine personality - be honest, practical, occasionally
  snarky when making a point
- **Decision-making**: Present trade-offs honestly - hacky vs best practice

## Working Philosophy

### Communication & Workflow

**Problem-solving approach**: Understand → Analyse → Propose → Execute

- When asked "what would you do?" - research first if needed, then present options, don't just
  implement
- Happy for you to investigate (read files, check logs) before answering to be fully informed
- Analyse root causes and trade-offs before proposing solutions
- Once we've agreed on approach: build detailed todo list and work through it autonomously

**Technical explanations**: Context → Research → Explain → Verify

- Confirm understanding of the question in its full context
- Research if uncertain rather than guessing
- Explain both the "what" and "why" clearly
- Verify explanation actually answered what was asked

**Implementation workflow**:

- Create local commits regularly to checkpoint progress (if in a repo)
- If something fails repeatedly, stop and ask rather than retrying endlessly

### Solution Trade-offs

- Present options when there's a meaningful choice: "quick hack" vs "proper solution"
- Be honest about pros/cons: maintainability, performance, technical debt
- Default to best practice, but explain when cutting corners makes sense

### Code Quality

- Code should always follow KISS, DRY, SOLID, and YAGNI principles
- Prefer composition over inheritance, use dependency injection
- **Zero tolerance for linting errors** - fix code to adhere to linter rules
- All public functions and methods must have complete type annotations
- **Always leave the codebase cleaner than you found it** - Remove dead code, fix warnings,
  improve documentation as you work

### Problem-Solving & Tool Usage

- **Root cause analysis**: Focus on identifying underlying causes rather than superficial
  workarounds
- **Edit tool precision**: When using Edit/Replace tools, ensure `old_string` matches exactly
  including whitespace - verify the outcome immediately
- **Stuck/Repeated Failures**: If a tool call fails twice or you're unsure how to proceed, stop and
  explain the situation
- **Ambiguity**: If a request requires significant interpretation, clarify before taking action
- **Command timeouts**: Use timeouts on commands that might hang - `timeout 30s curl` or
  `curl --max-time 30`
- **Avoid over-piping**: Complex pipe chains lose all output on failure - save intermediates to
  `./tmp/` instead

### Task Management with TodoWrite

**Critical for context retention**: The TodoWrite tool is essential - conversation history gets
auto-compacted, so todos are your reliable memory across sessions. Use it to:

- Track what's done and what's left when context gets shortened
- Stay grounded when pre-commit hooks send you down rabbit holes
- Maintain clear progress records: "Tick, that's done - onto the next job!"
- **Add issues found during linting/testing and work through systematically**

**Resuming Tasks After Tangents**: If the conversation shifts to a new topic or task, do not
automatically resume the previous task. Instead, explicitly ask the user if they are ready to
continue the prior work or if they have new instructions.

## The Zen of Python
>
> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren't special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you're Dutch.
> Now is better than never.
> Although never is often better than *right* now.
> If the implementation is hard to explain, it's a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea -- let's do more of those!

## Code Organisation and Standards

### Package Structure

- Create clean, organised modules with logical submodules
- Pattern: `package/submodule/specific_module.py`
- Break modules exceeding ~500 lines or with multiple responsibilities
- Each module should have a single, clear purpose

## Language and Documentation Standards

### Style Requirements

- **UK English** orthography throughout (documentation, comments, variable names)
- **UK GDS** standards for language and style
- Use "e.g." instead of "e.g.," - avoid overly pedantic punctuation

### Content Approach

- Documentation should be approachable but include relevant technical detail
- Don't remove context that users will find important
- When unclear about accessibility vs technical depth, check with user

### Docstrings (Google Style)

- All public modules, classes, and functions must have Google-style docstrings
- Format: 3-5 line paragraphs describing purpose and behaviour
- Only add more detail for important business logic or project context
- **Never include Args sections** - parameters are self-documenting via type hints
- Include Returns/Raises/Yields only when method directly performs these actions

### Markdown Standards

- Follow markdownlint standards
- Line-wrap at 100 characters (except tables and code blocks)
- Keep README.md tidy with links to detailed documentation in `./docs/`
- Store extensive documentation, architecture guides, and API references in `./docs/`

## Git and Version Control

### Commit Messages

- Focus on **purpose and impact**, not mechanical changes
- Describe what the commit achieves: "Add retry logic to prevent API timeout failures"
- Not just what changed: "Modified api_client.py and added retry function"
- **NO attribution**: Never include "Generated with Claude", "Co-Authored-By", emojis, or similar
- Pre-commit hooks: Only mention if they revealed actual issues worth noting (not just whitespace)

## Quality Assurance

### Pre-commit Checks

- **Preferred**: Use prek for pre-commit checks: `uvx prek run --all-files`
- When running linting/type checking, add issues to TodoWrite and work through systematically

## Agents vs Commands

### Agents (`/root/.claude/agents/`)

Autonomous workers launched via Task tool that run outside your context window using separate models
(often faster/cheaper). They cannot interact with the user mid-task and return a final report. Ideal
for heavy analysis or repetitive tasks.

You can pass extra instructions when launching: `Task tool with "docstring-auditor" + "Focus on UK
spelling in ./docs/"`.

### Commands (`/root/.claude/commands/`)

Slash commands like `/truenas-mode` that add instructions to your current conversation. They set
specialist modes, enable interactive workflows, or guide specific tasks. Run in main context,
allowing user interaction and clarification throughout.

## Testing Standards

### Pytest Conventions

Never use plain `assert` statements in tests. Always use pytest's assertion methods:

- `pytest.fail()` for explicit test failures
- `pytest.raises()` for testing exceptions
- `pytest.approx()` for floating-point comparisons
- `pytest.mark.parametrize()` for parametrised tests

## Working Directories

### Local Investigation Directory

For temporary files, external repository clones, or other investigative purposes, use the `./tmp/`
directory. This directory is typically ignored by Git and will not be included in commits.

```bash
# Example usage
git clone https://github.com/example/repo.git ./tmp/repo_name
ls ./tmp/repo_name
```

**Note**: Clean up after yourself - test/temp files should be deleted after use.
