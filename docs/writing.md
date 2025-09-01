# ✨ Writing agents and commands

Best practices and guidelines for creating effective Claude Code agents and custom commands.

## Agent fundamentals

Agents are specialised AI workers that handle complex, multi-step tasks on their own. Think of them
as expert consultants who can work independently on specific problems whilst you focus on other
things. Each agent should be really good at one particular area rather than being a Swiss Army knife
that tries to do everything poorly.

### Agent file structure

Every agent requires a Markdown file with YAML frontmatter that defines its capabilities and
configuration:

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

### Model selection

Choosing the right model matters for both speed and cost. Haiku is great at clear-cut, well-defined
tasks like code formatting, simple find-and-replace operations, running test suites, and checking if
code follows the rules. It's fast and lightweight, making it perfect for repetitive work.

Opus, on the other hand, is what you need when the work requires real thinking. Use it for
architectural decisions, complex refactoring that needs understanding of business logic, debugging
tricky issues, and any task that involves making judgement calls about code quality or design
patterns.

For complex workflows, try a mixed approach where Opus handles the overall strategy whilst Haiku
does the repetitive work. For example, Opus might look at the architecture and plan a refactoring
approach, then Haiku applies formatting to all files and updates import statements, before Opus
checks that the logic has been kept intact.

### Tool selection

Think carefully about which tools your agent actually needs. A simple formatting agent might only
need Read, Edit, and MultiEdit tools, whilst a big refactoring agent needs the full toolkit
including Glob, Grep, and TodoWrite for complex searches and keeping track of progress. Including
extra tools doesn't break anything, but it can make the agent's purpose less clear to users.

## Writing effective agents

### Core principles

A good agent does one thing well. Each agent should be really good at one specific type of task
rather than trying to be everything to everyone. Instead of creating a "python-everything" agent
that handles linting, testing, documentation, and deployment, build focused agents like
"python-ci-readiness", "docstring-auditor", and "test-coverage-analyst" that can work together but
each have clear, different purposes.

Users need to understand when to use your agent, so give clear examples of when it makes sense.
Explain scenarios like "You've finished building a feature and need quality checks" or "The test
suite is failing with type errors", and include example prompts that would naturally trigger the
agent, such as "Check my code is ready for CI" or "Fix all the type errors".

Documentation matters for complex agents. Give detailed, step-by-step instructions that explain not
just what the agent will do, but how it goes about the work. Break down the approach into clear
steps, like finding test files by searching for specific patterns, running the test suite to catch
failures, then working through type errors by reading file context and applying the right fixes.

Good error handling makes the difference between great agents and average ones. Think about common
failure scenarios and document how the agent should respond. When dependencies are missing, it
should check config files. When import errors happen, it should look at the project structure. For
permission issues, it should skip problem files and report back rather than crashing entirely.

### Testing and validation

Before using any agent in the wild, test it thoroughly across different scenarios. Try it on sample
projects with different structures and complexity levels. Make sure it can run multiple times
without breaking things. Check edge cases like empty files, missing dependencies, and weird project
setups. Keep an eye on performance to make sure it finishes in reasonable time, and always check
that the agent's final reports are actually useful.

### Naming convention

Agent names should be descriptive and action-focused, following the pattern of
`{language/domain}-{action}-{target}`. Good examples include "python-ci-readiness",
"javascript-test-generator", and "sql-query-optimiser". The name should immediately tell you what
the agent does and what area it works in.

## Command fundamentals

Commands are slash commands that change how Claude Code behaves for specific workflows. Unlike
agents that work on their own, commands run in the main context and let you interact throughout the
process. Think of them as modes that shape how the assistant approaches and responds to requests.

### Command file structure

Commands are simpler than agents, needing only a Markdown file in the `commands/` directory that
explains how behaviour changes:

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

### Command categories

Commands usually fall into a few broad categories. Development modes like `/test-driven`,
`/debug-mode`, and `/performance-opt` change how the assistant approaches coding tasks. Code quality
commands such as `/clean-code`, `/security-review`, and `/accessibility` focus the assistant on
specific parts of code improvement. Documentation commands like `/write-docs`, `/api-docs`, and
`/user-guide` tune the assistant for different types of writing tasks. Specialised workflows
including `/migration`, `/review-pr`, and `/architecture` give focused expertise for particular
scenarios.

## Writing effective commands

### Command principles

Like agents, commands work best when they have one clear purpose. A good command helps with one
specific workflow rather than trying to cover multiple unrelated things. Commands like
`/write-docs`, `/review-security`, and `/refactor-clean` work well because their scope is
immediately obvious. Avoid broad, vague commands like `/do-everything` that don't give meaningful
guidance.

Be clear about how the command changes behaviour. Users should understand exactly what they're
getting when they turn on a command. Say what the assistant will do differently, what it will focus
on, and what it will avoid. For documentation commands, you might say that the assistant will write
thorough docstrings for every public function, include usage examples in class docstrings, and add
type hints to all parameters, whilst avoiding removing existing documentation or making unnecessary
code changes.

Real examples help users understand how the command affects interactions. Show typical scenarios
and how the assistant's responses change when the command is on. This helps users figure out
whether a command will be useful for their current task and understand the assistant's thinking
when it behaves differently than usual.

Think about edge cases and document how the command handles weird situations. What happens when
there's no existing documentation to work with? How does the command handle conflicting coding
styles? What assumptions does it make about generated code or test files? Clear guidance prevents
confusion and keeps behaviour consistent.

### Command naming

Command names should be short and memorable, following patterns like `{action}-{target}` or
`{mode}`. Examples include `/write-docs`, `/debug-mode`, and `/clean-code`. The name should be easy
to type and remember whilst clearly showing what the command does.

## Documentation standards

### For agents

Agent documentation should cover five key areas that help users understand when and how to use them
well. Clearly explain what the agent does and what problems it solves. Give specific triggers and
example prompts that would naturally lead to using this agent. Document the step-by-step workflow
the agent follows, including any tools it uses and how it makes decisions. Be honest about what the
agent can't do, and describe what users can expect in the final report or output.

### For commands

Command documentation needs to explain how the assistant's behaviour changes when the command is
on, describe the workflow or process the assistant will follow, give examples of how common
interactions will work differently, and clarify any default assumptions or fallback behaviours the
command relies on.

## Advanced patterns

### Composable agents

Well-designed agents can work together to handle complex, multi-step workflows. Think about how
your agent fits into broader development processes and document which other agents work well with
it. A test coverage analyst might work well after a CI readiness agent, whilst a docstring auditor
might be most useful before running code quality checks.

### Progressive enhancement

Some agents benefit from multiple modes that users can choose based on their needs and time limits.
A basic mode might focus on formatting code and fixing obvious issues, whilst an advanced mode with
a `--thorough` flag could include deep refactoring, architecture improvements, and performance
tweaks. This approach lets users balance thoroughness against time constraints.

### Feedback loops

The best agents include checking steps that catch problems early and give confidence in their work.
After making changes, an agent might run tests to check functionality, make sure type hints still
work correctly, ensure imports still resolve properly, and report any regressions straight away.
These feedback loops make agents more reliable and help users trust their output.
