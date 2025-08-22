I need you to write and execute a professional git commit that balances clarity with
appropriate technical detail.

## Essential Pre-Commit Analysis

Before writing any commit message, you need to understand what's changing:

```bash
git status                    # Check staged vs unstaged files
git diff --cached            # Review exact changes being committed
git log --oneline -10        # Recent commit style reference
git log -3                   # Full recent messages for context
```

**Critical checks you should make:**

- Alert me if related files appear unstaged (they might need including)
- Understand both the mechanical change AND the real purpose
- Ask for clarification if there are blocking issues preventing commit

## Commit Message Requirements

### Style Requirements

- **UK English orthography** throughout
- Use "e.g." not "e.g.,"
- Professional tone without corporate jargon or wordy fluff
- Direct and factual language
- No emojis unless specifically requested
- No "Co-authored by" or tool attribution

### Subject Line

- Start with conventional prefix when applicable:
  `fix:`, `feat:`, `ci:`, `docs:`, `refactor:`, `test:`, `chore:`
- Otherwise start with imperative verb: Add, Fix, Update, Remove, Implement, Optimise
- Be specific about WHAT changed (the why comes in the body)
- Aim for 50 characters, hard limit at 72
- No full stop

### Body (Required for conventional commits)

- Blank line after subject
- First paragraph: explain the context and purpose in 1-2 sentences
- Keep it concise - 2-4 sentences total for most commits
- Add technical details only when necessary for understanding
- Wrap at 72 characters
- Include breaking changes, performance impacts, or important caveats

Remember: The commit message is for future developers (including yourself).
Provide enough context that someone unfamiliar with recent work can understand what changed and why.

## The Right Balance of Detail

Keep commits concise but complete. Focus on the context and purpose of changes
rather than listing mechanical file edits. Explain WHY the change was needed
and WHAT problem it solves, not just which lines changed. Be professional and
direct - no marketing language or unnecessary verbosity.

### Excellent Examples

```plain
docs(truenas): add custom app management and strengthen markdown linting

Document practical workflow for updating custom Docker Compose apps via
middleware API, including JSON/YAML conversion process and safe update steps.

Enforce consistent markdown formatting with explicit style rules for headings,
lists, and emphasis. Configure strict linting to maintain documentation quality.
```

```plain
fix: prevent race condition in connection pool cleanup

Database connections were being returned to pool before transaction commit,
causing intermittent data loss under high load. Now properly awaits commit
before releasing connection.
```

```plain
refactor: extract authentication logic into dedicated service

Consolidate scattered auth code to improve maintainability and enable future
OAuth providers. Includes proper token refresh and audit logging.

Breaking change: AuthMiddleware constructor now requires config object.
```

```plain
feat(api): add retry logic with exponential backoff

Prevent transient network failures from breaking API calls. Implements
configurable retry with jitter to avoid thundering herd problem.
```

### Poor Examples

```plain
Update files
Fix bugs
Make changes
```

Too vague - what files? what updates? why?

```plain
Add amazing new feature that revolutionises document processing

This incredible addition introduces comprehensive functionality...
```

Marketing fluff instead of technical substance

```plain
fix bug

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>
```

Uninformative and includes unnecessary attribution

## Creating the Commit

After analysing changes and writing the message, execute the commit unless there are
blocking issues requiring clarification.

## Handling Pre-Commit Hooks

Pre-commit hooks auto-fix some issues (whitespace, EOL) but not others (line
length, complex formatting). If hooks fail, fix the reported issues manually.

When hooks modify files:

1. Run `git status` to see what changed
2. Stage specific files: `git add path/to/file1 path/to/file2`
3. Never use `git add -A` - be precise about what you're staging
4. Retry the commit to see if further changes are required

If commit fails repeatedly, stop and explain the situation.

**CRITICAL**: Never modify check-related config files (e.g. `.pre-commit-config.yaml`,
`.markdownlint.yaml`) just to bypass failing checks. If linting or validation is
blocking the commit, consult the user with the specific issues and how to proceed.
