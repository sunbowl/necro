# Necro - Project Archaeology for AI-Assisted Development

A Claude Code skill that prevents AI coding assistants from referencing zombie code (deprecated, unused, or legacy files).

## The Problem

When using AI coding assistants like Claude Code, they can accidentally:
- Reference deprecated files that shouldn't be used
- Suggest patterns from old code that's been replaced
- Get confused by multiple versions of the same file (`config.js`, `config-old.js`, `config-backup.js`)
- Waste time reading/understanding dead code

## The Solution

Necro teaches Claude to:
- **Auto-detect** deprecated and zombie files
- **Archive** old files instead of deleting them
- **Document** active project structure in `ARCHITECTURE.md`
- **Track** all deprecations with reasons and replacements
- **Never** reference files in the `deprecated/` folder

## Installation

### Option 1: Clone (Recommended)

```bash
# macOS/Linux
cd ~/.claude/skills
git clone https://github.com/sunbowl/necro.git

# Windows
cd %USERPROFILE%\.claude\skills
git clone https://github.com/sunbowl/necro.git
```

### Option 2: Manual Download

1. Download this repository
2. Place the `necro` folder in your Claude skills directory:
   - **macOS/Linux:** `~/.claude/skills/necro/`
   - **Windows:** `%USERPROFILE%\.claude\skills\necro\`

### Verify Installation

The skill auto-activates. In your next Claude Code session, say:
- "Learn this project" or
- "Clean up code"

Claude should respond with Necro-aware behavior.

## How It Works

### Project Structure Created by Necro

```
your-project/
├── .necro/
│   └── config.json          # Necro settings
├── deprecated/
│   ├── DEPRECATION_LOG.md   # Audit trail of all deprecations
│   └── 2025-01-04/          # Dated archive folders
│       └── src/
│           └── old-file.js  # Archived files preserve path structure
├── ARCHITECTURE.md          # Active files documentation for AI
└── [your project files]
```

### Activation Triggers

Necro automatically activates when you:
- Enter a project directory (auto-scans for `.necro/`)
- Say "clean up code" or "remove unused files"
- Say "learn this project" or "explain my project"
- Say "deprecated", "zombie files", or "archive"
- Work with files matching zombie patterns (`*-old.*`, `*.bak`, etc.)

### What Happens

1. **On Project Entry:** Claude reads `ARCHITECTURE.md` to understand active files
2. **On Cleanup Request:** Claude scans for zombie files and offers to deprecate them
3. **On File Replacement:** Claude archives old files to `deprecated/` with full logging
4. **Ongoing:** Claude never suggests edits to files in `deprecated/`

## Commands (Natural Language)

| Say This | What Happens |
|----------|--------------|
| "Learn this project" | Initializes Necro, generates ARCHITECTURE.md |
| "Explain my project" | Shows project structure and potential issues |
| "Clean up code" | Scans for zombie files, offers deprecation |
| "Remove unused files" | Same as cleanup |
| "Deprecate [file]" | Archives file to deprecated/ with logging |
| "Clean old deprecated files" | Permanently deletes files past retention |

## File Formats

### .necro/config.json

```json
{
  "version": "1.0.0",
  "initialized": "2025-01-04 09:30:00",
  "project_root": "/path/to/project",
  "deprecated_retention_days": 30,
  "inventory_settings": {
    "ignore_dirs": ["node_modules", ".git", "dist", "build", "deprecated"],
    "suspicious_patterns": ["*-old.*", "*-backup.*", "*.bak", "*-copy.*"]
  }
}
```

### DEPRECATION_LOG.md

Tracks every deprecated file with:
- Original path
- Archive location
- Reason for deprecation
- Replacement file (if any)
- Date when permanent deletion is allowed

### ARCHITECTURE.md

AI-friendly documentation of:
- Project structure (directory tree)
- Active source files by category
- Scripts and commands
- Git information
- Clear warning to ignore `deprecated/`

## Zombie Detection

Necro flags files matching these patterns:
- `*-old.*` / `*_old.*` - Old versions
- `*-backup.*` / `*_backup.*` - Backups
- `*.bak` - Backup files
- `*-copy.*` / `*_copy.*` - Copies
- `*.orig` - Git merge artifacts
- `*~` - Editor backup files

Plus files not modified in 30+ days (configurable).

## Cross-Platform Compatibility

Necro works on:
- macOS
- Linux
- Windows

Uses forward slashes in paths (Git-compatible) and ISO 8601 dates.

## Why a Skill Instead of a CLI?

Previous versions of Necro were bash scripts. The skill approach is better because:

1. **No Installation Required** - Just drop files in `~/.claude/skills/`
2. **Cross-Platform** - Works everywhere Claude Code runs
3. **AI-Native** - Claude understands the intent, not just commands
4. **Smarter Detection** - Claude can identify zombie patterns contextually
5. **Integrated Workflow** - Deprecation happens naturally during development

## Files

```
necro/
├── SKILL.md       # Main skill definition (required)
├── reference.md   # Detailed templates and procedures
├── examples.md    # Real workflow examples
└── README.md      # This file
```

## Contributing

Contributions welcome! Areas of interest:
- Additional zombie detection patterns
- Language-specific file categorization
- Integration with other tools (eslint, etc.)
- Improved architecture generation

## License

MIT License - See LICENSE file

## Credits

Developed by [Sunbowl Systems](https://github.com/sunbowl)

Inspired by the need to keep AI coding assistants focused on active, relevant code.

---

**Remember:** Dead code tells no tales, but it sure confuses AI assistants.
