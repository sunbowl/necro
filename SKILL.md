---
name: necro
description: Project archaeology and technical debt management. Activates automatically when entering a project directory, when user mentions cleanup, deprecated files, zombies, architecture, or project structure. Also triggers on "clean up code", "remove unused files", "learn this project", "explain my project". Manages deprecated/ folders, ARCHITECTURE.md files, and zombie file detection. Auto-scans projects proactively.
---

# Necro - Project Archaeology for AI-Assisted Development

Necro prevents AI coding tools from referencing zombie code (deprecated, unused, or legacy files). It maintains clear boundaries between active and archived code.

## Auto-Activation Triggers

Activate Necro behaviors when:
- Entering any project directory (auto-scan for `.necro/`)
- User says: "clean up", "cleanup", "deprecated", "zombie", "unused files"
- User says: "learn this project", "explain my project", "project structure"
- User says: "remove unused", "archive old", "technical debt"
- Working with files that match suspicious patterns
- Before major refactoring work

## Quick Start

### First Time in a Project
```
1. Check if .necro/ exists → If yes, read config and ARCHITECTURE.md
2. If no .necro/, offer to initialize: "I notice this project doesn't have Necro set up. Want me to initialize it?"
3. After init, auto-generate ARCHITECTURE.md
```

### Proactive Scanning
When entering a project with Necro initialized:
1. Read `.necro/config.json` for settings
2. Read `ARCHITECTURE.md` to understand active files
3. Silently note any files in `deprecated/` to avoid referencing them
4. **NEVER** suggest edits to files in `deprecated/`

## Core Operations

### 1. Initialize Project (`necro init` equivalent)

When user wants to set up Necro or you detect a project without it:

```
Create these files/directories:
├── .necro/
│   └── config.json
├── deprecated/
│   └── DEPRECATION_LOG.md
└── ARCHITECTURE.md
```

**config.json template:**
```json
{
  "version": "1.0.0",
  "initialized": "[CURRENT_DATETIME]",
  "project_root": "[CURRENT_DIRECTORY]",
  "deprecated_retention_days": 30,
  "inventory_settings": {
    "ignore_dirs": ["node_modules", ".git", "dist", "build", "deprecated", ".necro", "__pycache__", "venv", ".venv", "vendor", "target"],
    "suspicious_patterns": ["*-old.*", "*-backup.*", "*.bak", "*-copy.*", "*.old", "*_old.*", "*_backup.*", "*-deprecated.*", "*.orig", "*~"]
  }
}
```

**DEPRECATION_LOG.md template:**
```markdown
# Deprecation Log

Track of all deprecated files. Files are retained for 30 days before permanent deletion.

| Date | Original Path | Archive Location | Reason | Replacement | Delete After |
|------|---------------|------------------|--------|-------------|--------------|
```

### 2. Deprecate Files (`necro deprecate` equivalent)

When removing/replacing files, NEVER delete. Always deprecate:

**Procedure:**
1. Create dated folder: `deprecated/YYYY-MM-DD/`
2. Preserve original path structure inside dated folder
3. Move file (not copy) to archive
4. Add entry to `DEPRECATION_LOG.md`
5. Update `ARCHITECTURE.md` to remove the file

**Example:**
```
Original: src/utils/old-helper.js
Archive:  deprecated/2025-01-04/src/utils/old-helper.js

Log entry:
| 2025-01-04 | src/utils/old-helper.js | deprecated/2025-01-04/src/utils/old-helper.js | Replaced with new implementation | src/utils/helper.js | 2025-02-03 |
```

### 3. Generate Architecture (`necro arch` equivalent)

Create/update `ARCHITECTURE.md` with:

1. **AI Warning Header** - Critical instruction to ignore deprecated/
2. **Project Structure** - Directory tree (3 levels, excluding ignored dirs)
3. **Active Source Files** - Grouped by directory/type
4. **Scripts** - From package.json, Makefile, or shell scripts
5. **Documentation** - Markdown files
6. **Git Info** - Branch, remote URL

See [reference.md](reference.md) for full template.

### 4. Inventory Scan (`necro inventory` equivalent)

Proactively scan for zombie files:

**What to scan for:**
- Files matching suspicious patterns from config
- Files not modified in 30+ days (configurable)
- Duplicate-looking filenames (file.js and file-old.js)
- Orphaned test files (tests for deleted code)
- Commented-out large code blocks

**When to scan:**
- On project entry (if Necro initialized)
- When user asks to "clean up" or find "unused files"
- Before major refactoring

**Output format:**
```markdown
## Potential Zombie Files Found

### Suspicious Filenames
- `src/utils/helper-old.js` - matches pattern *-old.*
- `lib/backup-config.json` - matches pattern *backup*

### Stale Files (>30 days unchanged)
- `scripts/unused-script.sh` - last modified 45 days ago

### Recommendation
Run deprecation on these files? I can move them to deprecated/ with proper logging.
```

### 5. Clean Deprecated (`necro deprecate clean` equivalent)

When user asks to clean old deprecated files:

1. Read `DEPRECATION_LOG.md`
2. Find entries where "Delete After" date has passed
3. List files eligible for permanent deletion
4. **Ask for confirmation** before deleting
5. Remove files and update log with "[DELETED]" marker

## Critical Rules

### NEVER Do These
1. **NEVER** reference files in `deprecated/` directory
2. **NEVER** delete files without deprecating first (except during clean)
3. **NEVER** modify `DEPRECATION_LOG.md` entries retroactively
4. **NEVER** skip the dated folder structure (always `deprecated/YYYY-MM-DD/`)

### ALWAYS Do These
1. **ALWAYS** read `ARCHITECTURE.md` when entering a Necro project
2. **ALWAYS** update `ARCHITECTURE.md` after deprecating files
3. **ALWAYS** preserve original directory structure in archives
4. **ALWAYS** log deprecations with reason and replacement info
5. **ALWAYS** ask before permanent deletion

## Project Detection

Check for Necro in this order:
1. `.necro/config.json` exists → Necro initialized
2. `deprecated/` exists but no `.necro/` → Partial setup, offer to complete
3. `ARCHITECTURE.md` exists but no `.necro/` → Manual setup, offer Necro
4. None exist → Offer to initialize

## Cross-Platform Notes

- Use forward slashes `/` in all paths (works on all platforms)
- Date format: `YYYY-MM-DD` (ISO 8601)
- Datetime format: `YYYY-MM-DD HH:MM:SS`
- Line endings: Use platform-appropriate (Git handles this)
- File operations: Use Claude's native Read/Write/Edit tools

## Integration with Other Tools

### Git Integration
- Add to `.gitignore` (optional): Nothing - deprecated/ SHOULD be tracked
- Commit message format: `chore(necro): deprecate [filename] - [reason]`

### AI Assistant Integration
When another AI or Claude session enters this project:
1. Load `ARCHITECTURE.md` first
2. Only reference files in "Active" sections
3. Check `deprecated/DEPRECATION_LOG.md` if unsure about a file
4. When replacing files, use Necro deprecation workflow

## See Also

- [reference.md](reference.md) - Detailed templates and procedures
- [examples.md](examples.md) - Real workflow examples
