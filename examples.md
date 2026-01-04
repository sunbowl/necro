# Necro Examples

Real-world workflow examples for common scenarios.

---

## Example 1: First Time in a New Project

**Trigger:** User says "learn this project" or "explain my project"

### Conversation Flow

**User:** "Help me understand this project"

**Claude (with Necro skill):**
```
I'll analyze this project for you. First, let me check if Necro is set up...

I don't see a .necro/ directory. Would you like me to initialize Necro?
This helps me:
- Track the project's active files
- Avoid referencing deprecated code
- Keep architecture documentation current

Initialize Necro? (creates .necro/, deprecated/, ARCHITECTURE.md)
```

**User:** "Yes, go ahead"

**Claude:**
```
Creating Necro structure...

✓ Created .necro/config.json
✓ Created deprecated/DEPRECATION_LOG.md
✓ Scanning project structure...
✓ Generated ARCHITECTURE.md

Here's what I found:

## Project Overview

**Type:** Node.js/Express application
**Structure:**
├── src/
│   ├── controllers/     (4 files)
│   ├── models/          (3 files)
│   ├── routes/          (2 files)
│   └── utils/           (5 files)
├── tests/               (8 files)
└── config/              (2 files)

**Key Files:**
- src/index.js - Main entry point
- src/routes/api.js - API route definitions
- config/database.js - Database configuration

**Potential Issues Found:**
- src/utils/helper-old.js - suspicious filename (*-old.*)
- src/models/user.backup.js - suspicious filename (*.backup.*)

Would you like me to review these suspicious files for deprecation?
```

---

## Example 2: Refactoring a Component

**Trigger:** User replaces an existing file

### Conversation Flow

**User:** "I need to rewrite the authentication module. The current one at src/auth/auth.js is a mess."

**Claude:**
```
I'll help you rewrite the authentication module. Since we're replacing an existing
file, I'll use Necro to properly archive the old version.

Let me first read the current implementation to understand what needs to be preserved...

[reads src/auth/auth.js]

Current auth.js has:
- login() function
- logout() function
- validateToken() function
- refreshToken() function

I'll create the new implementation. When done, I'll deprecate the old file.
```

**[Claude creates new src/auth/authentication.js]**

**Claude:**
```
New authentication module created at src/auth/authentication.js.

Now deprecating the old file...

Deprecated: src/auth/auth.js
├─ Archived to: deprecated/2025-01-04/src/auth/auth.js
├─ Reason: Complete rewrite with improved security and error handling
├─ Replacement: src/auth/authentication.js
└─ Deletable after: 2025-02-03

Updated ARCHITECTURE.md to reflect the change.

The old auth.js is preserved in deprecated/ for 30 days in case you need to
reference it. I won't suggest edits to it going forward.
```

---

## Example 3: Cleanup Request

**Trigger:** User says "clean up code" or "remove unused files"

### Conversation Flow

**User:** "Can you clean up this codebase? There's a lot of old stuff."

**Claude:**
```
I'll scan the project for potential cleanup candidates. Running Necro inventory...

## Inventory Scan Results

### Suspicious Filenames (5 files)
These match patterns typically used for backups/old versions:

1. src/components/Header-old.jsx
   └─ Matches: *-old.*

2. src/utils/dateUtils.backup.js
   └─ Matches: *.backup.*

3. lib/api-copy.js
   └─ Matches: *-copy.*

4. scripts/deploy.bak
   └─ Matches: *.bak

5. config/settings_old.json
   └─ Matches: *_old.*

### Stale Files (3 files, unchanged >30 days)
These haven't been modified recently:

1. src/pages/LegacyDashboard.jsx (67 days)
2. src/hooks/useOldAuth.js (45 days)
3. tests/integration/old-api.test.js (52 days)

### Recommended Actions

I can deprecate these files, which:
- Moves them to deprecated/2025-01-04/
- Logs them in DEPRECATION_LOG.md
- Preserves them for 30 days
- Updates ARCHITECTURE.md

Options:
1. Deprecate all 8 files
2. Deprecate only suspicious files (5)
3. Let me review each one individually
4. Skip cleanup

Which would you prefer?
```

**User:** "Review each one"

**Claude:**
```
Let's review each file:

### 1. src/components/Header-old.jsx
[reads file]
This appears to be an old version of Header.jsx. The current Header.jsx exists
and has the same functionality plus additional features.
→ Recommend: DEPRECATE (replacement exists)

### 2. src/utils/dateUtils.backup.js
[reads file]
Identical to dateUtils.js except for a removed function formatLegacyDate().
→ Recommend: DEPRECATE (backup of current file)

[continues for each file...]

Summary of recommendations:
- Deprecate: 6 files (clear replacements exist)
- Keep: 2 files (still referenced in active code)

Proceed with deprecating the 6 recommended files?
```

---

## Example 4: Batch Deprecation

**Trigger:** User provides list of files to deprecate

### Conversation Flow

**User:** "Deprecate these files:
- src/old-api.js
- lib/unused-helper.js
- components/LegacyButton.tsx
They were all replaced by the new unified module."

**Claude:**
```
I'll deprecate these 3 files. They'll be archived with today's date.

Processing...

✓ Deprecated: src/old-api.js
  └─ Archived to: deprecated/2025-01-04/src/old-api.js

✓ Deprecated: lib/unused-helper.js
  └─ Archived to: deprecated/2025-01-04/lib/unused-helper.js

✓ Deprecated: components/LegacyButton.tsx
  └─ Archived to: deprecated/2025-01-04/components/LegacyButton.tsx

Updated DEPRECATION_LOG.md:

| Date | Original Path | Archive Location | Reason | Replacement | Delete After |
|------|---------------|------------------|--------|-------------|--------------|
| 2025-01-04 | src/old-api.js | deprecated/2025-01-04/src/old-api.js | Replaced by unified module | src/unified/api.js | 2025-02-03 |
| 2025-01-04 | lib/unused-helper.js | deprecated/2025-01-04/lib/unused-helper.js | Replaced by unified module | src/unified/helpers.js | 2025-02-03 |
| 2025-01-04 | components/LegacyButton.tsx | deprecated/2025-01-04/components/LegacyButton.tsx | Replaced by unified module | src/unified/components.tsx | 2025-02-03 |

Updated ARCHITECTURE.md to remove these files from active listings.

All 3 files archived. They'll be retained until 2025-02-03.
```

---

## Example 5: Cleaning Old Deprecated Files

**Trigger:** User asks to clean deprecated files or maintenance task

### Conversation Flow

**User:** "Clean up old deprecated files"

**Claude:**
```
I'll check DEPRECATION_LOG.md for files past their retention period...

## Files Eligible for Permanent Deletion

These files have passed their 30-day retention period:

| File | Deprecated | Delete After | Age |
|------|------------|--------------|-----|
| deprecated/2024-11-15/src/legacy-auth.js | 2024-11-15 | 2024-12-15 | 50 days |
| deprecated/2024-11-20/lib/old-utils.js | 2024-11-20 | 2024-12-20 | 45 days |
| deprecated/2024-12-01/config/old-db.json | 2024-12-01 | 2024-12-31 | 34 days |

⚠️  **Warning:** This will PERMANENTLY delete these files. This cannot be undone.

Proceed with permanent deletion of 3 files?
```

**User:** "Yes, delete them"

**Claude:**
```
Permanently deleting deprecated files...

✓ Deleted: deprecated/2024-11-15/src/legacy-auth.js
✓ Deleted: deprecated/2024-11-20/lib/old-utils.js
✓ Deleted: deprecated/2024-12-01/config/old-db.json
✓ Removed empty folder: deprecated/2024-11-15/
✓ Removed empty folder: deprecated/2024-11-20/
✓ Removed empty folder: deprecated/2024-12-01/

Updated DEPRECATION_LOG.md - moved entries to "Deleted Records" section.

Cleanup complete. 3 files permanently removed, reclaimed ~45KB.
```

---

## Example 6: Entering Existing Necro Project

**Trigger:** Automatic when entering project with .necro/

### Conversation Flow

**User:** "I need to add a new feature to this project"

**Claude (automatically on entry):**
```
[Necro detected - loading project context...]

I've loaded this project's architecture:

## Project: ecommerce-api
- Type: Express.js API
- Active files: 47
- Deprecated files: 12 (in deprecated/)

Key areas:
- src/controllers/ - API endpoints
- src/services/ - Business logic
- src/models/ - Database models

Note: I found 2 files matching zombie patterns that aren't deprecated yet:
- src/services/payment-old.js
- src/utils/validation.backup.js

Want me to review these before we start on your new feature?
```

---

## Example 7: Explain Project Structure

**Trigger:** User says "explain my project"

### Conversation Flow

**User:** "Explain my project"

**Claude:**
```
Let me analyze this project's structure...

[If Necro not initialized: offers to initialize first]

## Project Analysis: my-web-app

### Overview
This is a **Next.js application** with TypeScript, using:
- React 18 for UI components
- Tailwind CSS for styling
- Prisma for database access
- NextAuth for authentication

### Directory Structure

```
my-web-app/
├── src/
│   ├── app/              # Next.js 13+ app router pages
│   │   ├── api/          # API routes
│   │   ├── dashboard/    # Dashboard pages
│   │   └── auth/         # Auth pages
│   ├── components/       # React components (23 files)
│   │   ├── ui/           # Base UI components
│   │   └── features/     # Feature-specific components
│   ├── lib/              # Utility libraries
│   └── hooks/            # Custom React hooks
├── prisma/               # Database schema & migrations
├── public/               # Static assets
└── tests/                # Test files
```

### Entry Points
- `src/app/layout.tsx` - Root layout
- `src/app/page.tsx` - Home page
- `src/app/api/` - API endpoints

### Configuration
- `next.config.js` - Next.js config
- `tailwind.config.js` - Tailwind config
- `prisma/schema.prisma` - Database schema

### Potential Issues
- Found 3 files with zombie patterns (not yet deprecated)
- 2 test files appear orphaned (no matching source)

Would you like me to:
1. Generate a detailed ARCHITECTURE.md?
2. Review the potential zombie files?
3. Start working on something specific?
```

---

## Anti-Pattern Examples

### Wrong: Deleting Instead of Deprecating

```
❌ User: "Delete the old config file"
❌ Claude: *deletes config-old.json permanently*

✓ User: "Delete the old config file"
✓ Claude: "I'll deprecate config-old.json instead of permanently deleting it.
           This preserves it for 30 days in case you need to reference it.

           Deprecated: config-old.json
           └─ Archived to: deprecated/2025-01-04/config-old.json

           Want to permanently delete it now instead?"
```

### Wrong: Referencing Deprecated Files

```
❌ Claude: "I see there's a helper function in deprecated/2024-12-01/utils.js
           that we could use..."

✓ Claude: "I found a similar pattern was used in a deprecated file. Let me
           check what replaced it... The new implementation is in src/utils/helpers.js.
           We should use that instead."
```

### Wrong: Forgetting to Update Architecture

```
❌ Claude: *deprecates file*
           "Done, file deprecated."

✓ Claude: *deprecates file*
           *updates ARCHITECTURE.md*
           *updates DEPRECATION_LOG.md*
           "Done. Deprecated file, updated architecture docs, and logged the change."
```
