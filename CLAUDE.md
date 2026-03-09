# pambrose-claude-plugins

Personal Claude Code plugin marketplace.

## Structure

This repo is a Claude Code marketplace. Each top-level directory is an independent plugin, and
`.claude-plugin/marketplace.json` is the marketplace manifest.

```
pambrose-claude-plugins/
├── .claude-plugin/
│   └── marketplace.json   # Marketplace manifest (lists all plugins)
├── linear-integration/  # Each plugin is a top-level directory
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── commands/
│   ├── agents/
│   ├── skills/
│   └── hooks/
└── CLAUDE.md
```

## Conventions

- Plugin directory names use kebab-case and match the `name` field in `plugin.json`
- Each plugin follows the standard Claude Code plugin structure (see plugin-dev:plugin-structure skill)
- When adding a new plugin, also add its entry to `.claude-plugin/marketplace.json`
- Use `${CLAUDE_PLUGIN_ROOT}` for all intra-plugin path references in hooks and scripts
