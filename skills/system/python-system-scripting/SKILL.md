---
name: python-system-scripting
version: 1.0.0
last-updated: 2026-02-23
description: Python system scripting for Claude memory system - hooks, session management, automation, daemon scripts, and enforcement. Windows-safe, ASCII-only, cp1252 compatible.
allowed-tools: Read,Glob,Grep,Bash,Edit,Write
user-invocable: true
---

# SKILL.md
## Python System Scripting Skill Instructions

### Skill Name
python-system-scripting

### Description
This skill provides patterns, rules, and guidance for writing Python scripts that power the Claude memory system. Covers hooks (UserPromptSubmit, PreToolUse, PostToolUse, Stop), session management, enforcement scripts, daemon/automation, and file-based IPC.

---

## Core Rules (NON-NEGOTIABLE)

### Rule 1: Windows-Safe Output (CRITICAL)
```python
# MANDATORY at top of EVERY script
import sys
if sys.platform == 'win32':
    import io
    sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8', errors='replace')
    sys.stderr = io.TextIOWrapper(sys.stderr.buffer, encoding='utf-8', errors='replace')
```

### Rule 2: NO Unicode Characters
- FORBIDDEN: Emojis, arrows, checkmarks, bullets, any non-ASCII
- ALLOWED: `[OK]`, `[ERROR]`, `[WARN]`, `[INFO]`, `->`, `*`, `-`, `+`, `#`
- This applies to ALL strings: print(), logging, file writes, comments

### Rule 3: UTF-8 File I/O
```python
# ALWAYS specify encoding for file operations
with open(path, 'r', encoding='utf-8', errors='replace') as f:
    data = f.read()

with open(path, 'w', encoding='utf-8') as f:
    f.write(content)

# Path objects
content = path.read_text(encoding='utf-8', errors='replace')
path.write_text(content, encoding='utf-8')
```

### Rule 4: Graceful Error Handling
```python
# Scripts are called by hooks - they MUST NOT crash
try:
    main_logic()
except Exception as e:
    log_event(f"[ERROR] {e}")
    sys.exit(0)  # Exit 0 = don't block hook pipeline
    # Exit 1 = BLOCK (only for enforcement scripts that intentionally block)
```

---

## Script Architecture Patterns

### Pattern 1: Hook Script (UserPromptSubmit / PreToolUse / PostToolUse / Stop)
```python
#!/usr/bin/env python3
"""
Script Name: example-hook.py
Version: 1.0.0
Last Modified: 2026-02-23
Description: [What this hook does]
Hook Type: [UserPromptSubmit | PreToolUse | PostToolUse | Stop]
Windows-Safe: No Unicode chars (ASCII only, cp1252 compatible)
"""

import sys
import os
import json
from pathlib import Path
from datetime import datetime

# Windows encoding fix
if sys.platform == 'win32':
    import io
    sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8', errors='replace')
    sys.stderr = io.TextIOWrapper(sys.stderr.buffer, encoding='utf-8', errors='replace')

# Standard paths
MEMORY_BASE = Path.home() / '.claude' / 'memory'
CURRENT_DIR = MEMORY_BASE / 'current'
LOGS_DIR = MEMORY_BASE / 'logs'
FLAG_DIR = Path.home() / '.claude'

def read_hook_stdin():
    """Read JSON from Claude Code hook stdin"""
    try:
        if not sys.stdin.isatty():
            raw = sys.stdin.read()
            if raw and raw.strip():
                return json.loads(raw.strip())
    except Exception:
        pass
    return {}

def log_event(msg):
    """Log to script-specific log file"""
    log_file = LOGS_DIR / 'hook-events.log'
    log_file.parent.mkdir(parents=True, exist_ok=True)
    ts = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    try:
        with open(log_file, 'a', encoding='utf-8') as f:
            f.write(f"{ts} | {msg}\n")
    except Exception:
        pass

def main():
    hook_data = read_hook_stdin()
    # ... script logic ...
    sys.exit(0)

if __name__ == '__main__':
    main()
```

### Pattern 2: Flag-Based IPC (Inter-Process Communication)
```python
# Scripts communicate via flag files in ~/.claude/
# Flag = JSON file with metadata + auto-expiry

def write_flag(name, data, session_id=''):
    """Write enforcement flag (session-specific for Loophole #11)"""
    suffix = f"-{session_id}" if session_id else ""
    flag_path = FLAG_DIR / f'.{name}{suffix}.json'
    payload = {
        **data,
        'created_at': datetime.now().isoformat(),
        'session_id': session_id,
    }
    flag_path.write_text(json.dumps(payload, indent=2), encoding='utf-8')

def read_flag(name, session_id='', max_age_minutes=60):
    """Read flag with auto-expiry (Loophole #10)"""
    suffix = f"-{session_id}" if session_id else ""
    flag_path = FLAG_DIR / f'.{name}{suffix}.json'
    if not flag_path.exists():
        return None
    try:
        data = json.loads(flag_path.read_text(encoding='utf-8'))
        created = datetime.fromisoformat(data.get('created_at', ''))
        age = (datetime.now() - created).total_seconds() / 60
        if age > max_age_minutes:
            flag_path.unlink(missing_ok=True)
            return None
        return data
    except Exception:
        return None

def clear_flag(name, session_id=''):
    """Remove flag file"""
    suffix = f"-{session_id}" if session_id else ""
    flag_path = FLAG_DIR / f'.{name}{suffix}.json'
    flag_path.unlink(missing_ok=True)
```

### Pattern 3: Session Data Access
```python
# Standard session file operations

SESSIONS_DIR = MEMORY_BASE / 'sessions'
CURRENT_SESSION_FILE = MEMORY_BASE / '.current-session.json'

def get_current_session_id():
    """Get active session ID"""
    if not CURRENT_SESSION_FILE.exists():
        return None
    try:
        data = json.loads(CURRENT_SESSION_FILE.read_text(encoding='utf-8'))
        return data.get('current_session_id')
    except Exception:
        return None

def read_session(session_id):
    """Read session JSON file"""
    path = SESSIONS_DIR / f'{session_id}.json'
    if not path.exists():
        return None
    try:
        return json.loads(path.read_text(encoding='utf-8'))
    except Exception:
        return None

def update_session(session_id, updates):
    """Merge updates into session JSON"""
    path = SESSIONS_DIR / f'{session_id}.json'
    data = read_session(session_id) or {}
    data.update(updates)
    data['last_updated'] = datetime.now().isoformat()
    path.write_text(json.dumps(data, indent=2), encoding='utf-8')
```

### Pattern 4: File Locking (Concurrent Access Safety)
```python
# Windows file locking for shared JSON files (Loophole #19)
import msvcrt

def locked_json_update(filepath, updater_fn):
    """
    Read-modify-write JSON with file lock.
    updater_fn receives current data dict, returns updated dict.
    """
    filepath = Path(filepath)
    filepath.parent.mkdir(parents=True, exist_ok=True)

    if not filepath.exists():
        filepath.write_text('{}', encoding='utf-8')

    with open(filepath, 'r+', encoding='utf-8') as f:
        try:
            msvcrt.locking(f.fileno(), msvcrt.LK_NBLCK, 1)
        except IOError:
            return None  # Another process holds the lock

        try:
            content = f.read()
            data = json.loads(content) if content.strip() else {}
            updated = updater_fn(data)
            f.seek(0)
            f.truncate()
            json.dump(updated, f, indent=2)
            return updated
        finally:
            f.seek(0)
            msvcrt.locking(f.fileno(), msvcrt.LK_UNLCK, 1)
```

---

## Standard Paths Reference

| Path | Purpose |
|------|---------|
| `~/.claude/memory/` | Memory system root |
| `~/.claude/memory/current/` | Active versioned scripts |
| `~/.claude/memory/sessions/` | Session JSON files |
| `~/.claude/memory/logs/` | All log files |
| `~/.claude/memory/logs/sessions/{SESSION_ID}/` | Per-session logs |
| `~/.claude/memory/.current-session.json` | Active session pointer |
| `~/.claude/.hook-state.json` | Hook state tracking |
| `~/.claude/` | Flag files for IPC |

---

## Exit Code Convention

| Code | Meaning | When |
|------|---------|------|
| `0` | Success / Allow | Script completed, tool call allowed |
| `1` | Block / Deny | Enforcement script blocks action (PreToolUse) |
| `2` | Warning | Non-fatal issue detected |

---

## Logging Standards

```python
# Log levels via prefix
log_event("[OK] Task completed successfully")
log_event("[ERROR] Failed to read session file: FileNotFoundError")
log_event("[WARN] Session file missing chain data")
log_event("[INFO] Processing session SESSION-20260223-133518-2DRM")
log_event("[SKIP] No action needed for this hook event")
```

---

## Testing Scripts

```bash
# Test hook script with mock stdin
echo '{"prompt":"test message","session_id":"SESSION-TEST"}' | python script.py

# Test with no stdin (should handle gracefully)
python script.py < /dev/null

# Verify no Unicode in output
python script.py 2>&1 | python -c "import sys; [print(f'Line {i}: OK') if all(ord(c)<128 for c in line) else print(f'Line {i}: UNICODE DETECTED: {line}') for i,line in enumerate(sys.stdin,1)]"
```

---

## Checklist Before Committing Any Python Script

1. [ ] Windows encoding wrapper at top
2. [ ] NO Unicode/emoji characters anywhere
3. [ ] All file I/O uses encoding='utf-8'
4. [ ] Graceful error handling (try/except around main)
5. [ ] Correct exit codes (0=allow, 1=block)
6. [ ] Standard path constants used (not hardcoded)
7. [ ] Logging uses log_event() pattern
8. [ ] Hook stdin read handles empty/missing data
9. [ ] Flag files use session-specific naming
10. [ ] File locking for shared JSON files

---

## Version History

- v1.0.0 (2026-02-23): Initial skill creation
  - Core rules (Windows-safe, no Unicode, UTF-8 I/O)
  - 4 architecture patterns (Hook, Flag IPC, Session, File Locking)
  - Standard paths, exit codes, logging, testing, checklist
