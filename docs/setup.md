# 📦 Setup Guide

Complete setup instructions for Claude Code Files repository.

## Prerequisites

- Git installed on your system
- A Git hosting account (GitHub, GitLab, etc.)
- Claude Code CLI installed

## For New Users

If you're setting up Claude Code for the first time:

### 1. Fork the Repository

First, fork the [claude-code-files repository](https://github.com/your-org/claude-code-files) to
your own Git account. This allows you to:

- Customise configurations without affecting the original
- Pull updates from upstream when needed
- Push your personal preferences

### 2. Clone to ~/.claude

```bash
git clone ssh://git@github.com/your-username/claude-code-files.git ~/.claude
```

Or if using HTTPS:

```bash
git clone https://github.com/your-username/claude-code-files.git ~/.claude
```

### 3. Customise CLAUDE.md

Edit `~/.claude/CLAUDE.md` to include your personal preferences, coding standards, and any
project-specific instructions.

### 4. Start Claude Code

You're ready to go! Claude Code will automatically detect and use your configurations.

## For Existing Claude Code Users

If you're already using Claude Code with existing configurations, follow these migration steps to
preserve your setup whilst gaining version control:

### 1. Quit Claude Code

Ensure Claude Code is completely closed before starting the migration.

### 2. Backup Existing Configuration

```bash
mv ~/.claude ~/backup_claude
```

This preserves all your existing agents, commands, and settings.

### 3. Clone Your Fork

```bash
git clone ssh://git@github.com/your-username/claude-code-files.git ~/.claude
```

### 4. Preserve Repository's CLAUDE.md

The repository comes with a template CLAUDE.md. Save it for reference:

```bash
cd ~/.claude
mv CLAUDE.md backup_CLAUDE.md
```

### 5. Restore Your Configurations

Copy all your existing configurations back:

```bash
cp -r ~/backup_claude/* ~/.claude/
```

### 6. Merge CLAUDE.md Files

You now have two CLAUDE.md files:

- `CLAUDE.md` - Your existing personalised version
- `backup_CLAUDE.md` - The repository's template

Review both and merge the best parts:

1. Open both files in your editor
2. Incorporate useful sections from the template into your version
3. Remove the backup: `rm backup_CLAUDE.md`

### 7. Review Changes

Check what needs to be committed:

```bash
git status
```

You'll see:

- Modified `CLAUDE.md` (your personalised version)
- New files in `agents/` (your custom agents)
- New files in `commands/` (your custom commands)
- Other Claude Code files (ignored by `.gitignore`)

### 8. Commit Your Configuration

```bash
git add agents/*.md commands/*.md CLAUDE.md
git commit -m "Add my existing Claude Code configurations"
git push origin main
```

### 9. Clean Up

Once you've verified everything works:

```bash
rm -rf ~/backup_claude
```

### 10. Start Claude Code

Your configurations are now version controlled and ready to use!

## Syncing Across Machines

Once your configurations are in Git, you can easily sync them across multiple machines:

### On a New Machine

```bash
git clone ssh://git@github.com/your-username/claude-code-files.git ~/.claude
```

### Pulling Updates

```bash
cd ~/.claude
git pull
```

### Pushing Changes

After modifying configurations:

```bash
cd ~/.claude
git add -A
git commit -m "Update configurations"
git push
```

## Updating from Upstream

If the original repository has useful updates:

### Add Upstream Remote (once)

```bash
cd ~/.claude
git remote add upstream https://github.com/original-org/claude-code-files.git
```

### Pull Updates

```bash
git fetch upstream
git merge upstream/main
```

Resolve any conflicts, then push to your fork:

```bash
git push origin main
```

## Troubleshooting

### Permission Issues

If you get permission denied errors:

- Ensure you're using the correct Git URL (SSH vs HTTPS)
- Check your SSH keys are properly configured
- Verify you have push access to your fork

### Missing Configurations

If Claude Code doesn't detect your configurations:

- Verify files are in the correct directories (`~/.claude/agents/`, `~/.claude/commands/`)
- Check file extensions are `.md`
- Ensure YAML frontmatter is properly formatted in agent files

### Git Conflicts

When merging from upstream:

1. Review conflicts carefully
2. Keep your customisations when in doubt
3. Test configurations after resolving
4. Commit the resolution

## Advanced Setup

### Using Different Branches

Maintain different configurations for different contexts:

```bash
git checkout -b work-config
# Make work-specific changes
git commit -am "Add work configurations"

git checkout -b personal-config
# Make personal changes
git commit -am "Add personal configurations"
```

Switch between them:

```bash
git checkout work-config  # When at work
git checkout personal-config  # When on personal projects
```

### Sharing Team Configurations

Teams can maintain a shared fork:

1. Create a team organisation on your Git host
2. Fork the repository to the team org
3. Add team members as collaborators
4. Maintain team standards in the shared fork
5. Individual members can fork from the team fork for personal customisations
