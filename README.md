# Demo Claude Code Marketplace

Reference implementation demonstrating Claude Code marketplace and plugin structure.

## Installation

Add this marketplace to Claude Code:

```bash
/plugin marketplace add <your-git-url>
```

## Available Plugins

### Code Review
Automated code review checklist for self-review before commits.

**Install:** `/plugin add code-review`
**Usage:** `/review`

### API Development
REST API scaffolding guide with standard patterns.

**Install:** `/plugin add api-dev`
**Usage:** `/api`

### Refactoring Assistant
Safe refactoring workflow with test-driven approach.

**Install:** `/plugin add refactoring`
**Usage:** `/refactor`

## Plugin Structure

Each plugin contains:
- `plugin.json` - Plugin metadata
- `commands/*.md` - Slash commands
- `skills/SKILL.md` - Workflow skills

## For Learning

This marketplace demonstrates:
- Marketplace manifest structure
- Plugin organization and metadata
- Slash command + skill integration
- Opt-in plugin activation

## License

MIT
