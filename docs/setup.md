# 📦 Setup guide

Complete setup instructions for Claude Code Files repository.

## Prerequisites

You'll need Git installed on your system, a Git hosting account (GitHub, GitLab, etc.), and Claude
Code CLI installed.

## New users

If you're setting up Claude Code for the first time, you have two options:

**Option 1: Fork to your own repository** (recommended for ongoing customisation)

Clone the template from the [claude-code-files repository](https://git.tomfos.tr/tom/claude-code-files)
and push it to your own Git hosting:

```bash
# Clone the template
git clone https://git.tomfos.tr/tom/claude-code-files.git ~/.claude
cd ~/.claude

# Add your own repository as origin
git remote remove origin
git remote add origin https://github.com/your-username/claude-code-files.git
git push -u origin main
```

This lets you track your customisations, share configurations across machines, and pull updates
from the original template when needed.

**Option 2: Direct clone** (simpler, but no version control of your changes)

```bash
git clone https://git.tomfos.tr/tom/claude-code-files.git ~/.claude
```

With either option, edit `~/.claude/CLAUDE.md` to include your personal preferences, coding
standards, and any project-specific instructions. Once that's done, you're ready to go! Claude Code
will automatically detect and use your configurations.

## Existing users

If you're already using Claude Code with existing configurations, you can migrate to this system
whilst preserving your setup and gaining version control.

First, make sure Claude Code is completely closed, then backup your existing configuration:

```bash
mv ~/.claude ~/backup_claude
```

This preserves all your existing agents, commands, and settings. Now clone your fork:

```bash
git clone https://your-git-host.com/your-username/claude-code-files.git ~/.claude
```

The repository comes with a template CLAUDE.md that you might want to reference later, so save it:

```bash
cd ~/.claude
mv CLAUDE.md backup_CLAUDE.md
```

Copy all your existing configurations back:

```bash
cp -r ~/backup_claude/* ~/.claude/
```

Now you have two CLAUDE.md files - your existing personalised version and the repository's template
(saved as `backup_CLAUDE.md`). Open both files in your editor and incorporate any useful sections
from the template into your version. When you're done, remove the backup with `rm backup_CLAUDE.md`.

Check what needs to be committed:

```bash
git status
```

You'll see your modified `CLAUDE.md`, new files in `agents/` and `commands/` directories, and other
Claude Code files that are ignored by `.gitignore`. Commit your configuration:

```bash
git add agents/*.md commands/*.md CLAUDE.md
git commit -m "Add my existing Claude Code configurations"
git push origin main
```

Once you've verified everything works, clean up the backup:

```bash
rm -rf ~/backup_claude
```

Your configurations are now version controlled and ready to use!

## Syncing across machines

Once your configurations are in Git, you can easily sync them across multiple machines. On a new
machine, simply clone your fork:

```bash
git clone https://your-git-host.com/your-username/claude-code-files.git ~/.claude
```

To pull updates from your other machines:

```bash
cd ~/.claude
git pull
```

After modifying configurations on any machine:

```bash
cd ~/.claude
git add -A
git commit -m "Update configurations"
git push
```

## Updating from upstream

If the original repository has useful updates, you can pull them into your fork. Add the upstream
remote (you only need to do this once):

```bash
cd ~/.claude
git remote add upstream https://git.tomfos.tr/tom/claude-code-files.git
```

Then pull and merge updates:

```bash
git fetch upstream
git merge upstream/main
```

Resolve any conflicts, then push to your fork:

```bash
git push origin main
```

## Troubleshooting

**Permission issues**: If you get permission denied errors, make sure you're using the correct Git
URL (SSH vs HTTPS), check your SSH keys are properly configured, and verify you have push access to
your fork.

**Missing configurations**: If Claude Code doesn't detect your configurations, verify files are in
the correct directories (`~/.claude/agents/`, `~/.claude/commands/`), check file extensions are
`.md`, and ensure YAML frontmatter is properly formatted in agent files.

**Git conflicts**: When merging from upstream, review conflicts carefully, keep your customisations
when in doubt, test configurations after resolving, and commit the resolution.

## Usage examples

Once your configurations are set up, here are some common usage patterns.

When you want to use an agent:

```plain
User: "I've finished implementing the new authentication module. Can you make sure it's ready for CI?"
Assistant: "I'll use the python-ci-readiness agent to run through the complete quality assurance process."
[Agent runs pytest, mypy, ruff check, and ruff format, fixing issues along the way]
```

When you want to activate a command:

```plain
User: /write-docs
Assistant: Documentation mode activated. I'll focus on creating comprehensive, well-structured documentation.
```
