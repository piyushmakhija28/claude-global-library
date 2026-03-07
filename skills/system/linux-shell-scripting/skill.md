---
name: linux-shell-scripting
version: 1.0.0
last-updated: 2026-03-07
description: Linux/Unix shell scripting (Bash, sh, zsh) - system automation, file processing, process management, networking, cron jobs, text manipulation, and production-grade script patterns.
allowed-tools: Read,Glob,Grep,Bash,Edit,Write
user-invocable: true
---

# SKILL.md
## Linux Shell Scripting Skill Instructions

### Skill Name
linux-shell-scripting

### Description
Comprehensive skill for writing production-grade Linux/Unix shell scripts. Covers Bash scripting fundamentals, POSIX compliance, system automation, file processing, process management, networking utilities, cron scheduling, text manipulation (awk/sed/grep), error handling, and security best practices.

---

## Core Rules (NON-NEGOTIABLE)

### Rule 1: Shebang and Shell Selection
```bash
#!/usr/bin/env bash
# For maximum portability use env lookup
# Use #!/bin/bash only when bash-specific features are required
# Use #!/bin/sh for POSIX-only scripts (maximum compatibility)
```

### Rule 2: Strict Mode (MANDATORY for all scripts)
```bash
set -euo pipefail
# -e: Exit on error
# -u: Treat unset variables as errors
# -o pipefail: Pipe fails if any command in pipeline fails

# Optional: Debug mode
# set -x  # Print each command before execution (debug only)
```

### Rule 3: Quoting (ALWAYS quote variables)
```bash
# CORRECT
echo "$variable"
cp "$source" "$destination"
if [[ -f "$file" ]]; then

# WRONG - never do this
echo $variable
cp $source $destination
if [ -f $file ]; then
```

### Rule 4: Use [[ ]] Over [ ]
```bash
# CORRECT (bash)
[[ -f "$file" ]]
[[ "$str" == "value" ]]
[[ "$num" -gt 5 ]]
[[ "$str" =~ ^[0-9]+$ ]]  # regex support

# AVOID (POSIX [ ] has quirks)
[ -f "$file" ]
```

---

## Script Template (Production-Grade)

```bash
#!/usr/bin/env bash
#
# Script: script-name.sh
# Version: 1.0.0
# Description: Brief description of what this script does
# Usage: ./script-name.sh [options] <arguments>
# Author: Auto-generated
# Date: YYYY-MM-DD
#

set -euo pipefail

# ─── Constants ───────────────────────────────────────────────
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "${BASH_SOURCE[0]}")"
readonly LOG_FILE="/var/log/${SCRIPT_NAME%.sh}.log"

# ─── Default Configuration ──────────────────────────────────
VERBOSE=false
DRY_RUN=false

# ─── Functions ───────────────────────────────────────────────

log() {
    local level="$1"
    shift
    local msg="$*"
    local ts
    ts="$(date '+%Y-%m-%d %H:%M:%S')"
    echo "[$ts] [$level] $msg" | tee -a "$LOG_FILE" 2>/dev/null || true
}

log_info()  { log "INFO"  "$@"; }
log_warn()  { log "WARN"  "$@"; }
log_error() { log "ERROR" "$@"; }

die() {
    log_error "$@"
    exit 1
}

usage() {
    cat <<EOF
Usage: $SCRIPT_NAME [OPTIONS] <args>

Options:
  -h, --help       Show this help message
  -v, --verbose    Enable verbose output
  -n, --dry-run    Show what would be done without executing

Examples:
  $SCRIPT_NAME -v /path/to/dir
  $SCRIPT_NAME --dry-run file.txt
EOF
    exit 0
}

cleanup() {
    # Called on EXIT - clean up temp files, processes, etc.
    local exit_code=$?
    if [[ -n "${TEMP_DIR:-}" && -d "$TEMP_DIR" ]]; then
        rm -rf "$TEMP_DIR"
    fi
    exit "$exit_code"
}

# ─── Argument Parsing ───────────────────────────────────────

parse_args() {
    while [[ $# -gt 0 ]]; do
        case "$1" in
            -h|--help)    usage ;;
            -v|--verbose) VERBOSE=true; shift ;;
            -n|--dry-run) DRY_RUN=true; shift ;;
            --)           shift; break ;;
            -*)           die "Unknown option: $1" ;;
            *)            break ;;
        esac
    done

    # Remaining positional args
    ARGS=("$@")
}

# ─── Main Logic ─────────────────────────────────────────────

main() {
    parse_args "$@"
    trap cleanup EXIT

    log_info "Starting $SCRIPT_NAME"

    # ... script logic here ...

    log_info "Completed successfully"
}

# ─── Entry Point ─────────────────────────────────────────────
main "$@"
```

---

## Essential Patterns

### Pattern 1: File Processing
```bash
# Process file line by line (safe - handles spaces, special chars)
while IFS= read -r line; do
    echo "Processing: $line"
done < "$input_file"

# Process with line numbers
local line_num=0
while IFS= read -r line; do
    ((line_num++))
    echo "$line_num: $line"
done < "$input_file"

# Find and process files (safe for filenames with spaces)
find "$dir" -type f -name "*.log" -print0 | while IFS= read -r -d '' file; do
    echo "Found: $file"
done

# CSV parsing
while IFS=',' read -r col1 col2 col3; do
    echo "Column 1: $col1, Column 2: $col2"
done < "$csv_file"
```

### Pattern 2: Text Processing (awk/sed/grep)
```bash
# awk - columnar data extraction
awk '{print $1, $3}' file.txt                    # Print columns 1 and 3
awk -F',' '{sum+=$2} END{print sum}' data.csv     # Sum column 2
awk '/pattern/{print NR": "$0}' file.txt          # Lines matching pattern

# sed - stream editing
sed -i 's/old/new/g' file.txt                     # In-place replacement
sed -n '10,20p' file.txt                          # Print lines 10-20
sed '/^#/d' config.file                           # Remove comment lines
sed -i.bak 's/foo/bar/g' file.txt                 # Replace with backup

# grep - pattern searching
grep -r "pattern" /path/to/dir                    # Recursive search
grep -n "ERROR" log.txt                           # With line numbers
grep -c "pattern" file.txt                        # Count matches
grep -l "TODO" *.py                               # Files containing match
grep -E "^[0-9]{3}-[0-9]{4}$" file.txt            # Extended regex
grep -v "^#" config.file | grep -v "^$"           # Non-comment, non-empty
```

### Pattern 3: Process Management
```bash
# Background process with PID tracking
long_running_command &
local pid=$!
echo "$pid" > /var/run/myapp.pid

# Wait for process
wait "$pid" || die "Process $pid failed"

# Kill process by PID file
kill_by_pidfile() {
    local pidfile="$1"
    if [[ -f "$pidfile" ]]; then
        local pid
        pid="$(cat "$pidfile")"
        if kill -0 "$pid" 2>/dev/null; then
            kill "$pid"
            wait "$pid" 2>/dev/null || true
        fi
        rm -f "$pidfile"
    fi
}

# Timeout execution
timeout 30 some_command || die "Command timed out after 30s"

# Check if command exists
command -v docker &>/dev/null || die "docker is not installed"
```

### Pattern 4: Temporary Files and Directories
```bash
# Safe temp file creation
TEMP_FILE="$(mktemp)"
TEMP_DIR="$(mktemp -d)"

# Ensure cleanup on exit
trap 'rm -rf "$TEMP_FILE" "$TEMP_DIR"' EXIT

# Named pipe (FIFO) for IPC
FIFO="$(mktemp -u)"
mkfifo "$FIFO"
trap 'rm -f "$FIFO"' EXIT
```

### Pattern 5: Locking (Prevent Concurrent Execution)
```bash
LOCK_FILE="/var/lock/${SCRIPT_NAME}.lock"

acquire_lock() {
    if ! mkdir "$LOCK_FILE" 2>/dev/null; then
        die "Another instance is running (lock: $LOCK_FILE)"
    fi
    trap 'rm -rf "$LOCK_FILE"' EXIT
}

# Using flock (preferred on Linux)
exec 200>"$LOCK_FILE"
flock -n 200 || die "Another instance is running"
```

### Pattern 6: Networking
```bash
# Check port availability
check_port() {
    local host="$1" port="$2"
    if nc -z "$host" "$port" 2>/dev/null; then
        echo "Port $port is open on $host"
        return 0
    fi
    return 1
}

# Wait for service to be ready
wait_for_service() {
    local host="$1" port="$2" timeout="${3:-30}"
    local elapsed=0
    while ! nc -z "$host" "$port" 2>/dev/null; do
        ((elapsed++))
        if [[ $elapsed -ge $timeout ]]; then
            die "Service $host:$port not ready after ${timeout}s"
        fi
        sleep 1
    done
    log_info "Service $host:$port is ready"
}

# Download with retry
download_with_retry() {
    local url="$1" dest="$2" max_retries="${3:-3}"
    local attempt=0
    while ((attempt < max_retries)); do
        ((attempt++))
        if curl -fsSL -o "$dest" "$url"; then
            return 0
        fi
        log_warn "Download attempt $attempt/$max_retries failed"
        sleep $((attempt * 2))
    done
    return 1
}
```

### Pattern 7: Cron Job Management
```bash
# Add a cron job (idempotent)
add_cron_job() {
    local schedule="$1" command="$2"
    local marker="# managed:${SCRIPT_NAME}"
    (crontab -l 2>/dev/null | grep -v "$marker"; echo "$schedule $command $marker") | crontab -
}

# Remove a cron job
remove_cron_job() {
    local marker="# managed:${SCRIPT_NAME}"
    crontab -l 2>/dev/null | grep -v "$marker" | crontab -
}

# Common cron schedules
# ┌──── minute (0-59)
# │ ┌──── hour (0-23)
# │ │ ┌──── day of month (1-31)
# │ │ │ ┌──── month (1-12)
# │ │ │ │ ┌──── day of week (0-7, 0=7=Sunday)
# * * * * *
# 0 * * * *     Every hour
# */5 * * * *   Every 5 minutes
# 0 2 * * *     Daily at 2 AM
# 0 0 * * 0     Weekly on Sunday
# 0 0 1 * *     Monthly on the 1st
```

### Pattern 8: Service/Daemon Script
```bash
# systemd-compatible service script pattern
start_service() {
    log_info "Starting service..."
    nohup "$SCRIPT_DIR/app" >> "$LOG_FILE" 2>&1 &
    echo $! > "$PID_FILE"
    log_info "Service started (PID: $(cat "$PID_FILE"))"
}

stop_service() {
    if [[ -f "$PID_FILE" ]]; then
        local pid
        pid="$(cat "$PID_FILE")"
        if kill -0 "$pid" 2>/dev/null; then
            log_info "Stopping service (PID: $pid)..."
            kill "$pid"
            local count=0
            while kill -0 "$pid" 2>/dev/null && ((count < 10)); do
                sleep 1
                ((count++))
            done
            if kill -0 "$pid" 2>/dev/null; then
                log_warn "Force killing PID $pid"
                kill -9 "$pid"
            fi
        fi
        rm -f "$PID_FILE"
    fi
}

status_service() {
    if [[ -f "$PID_FILE" ]] && kill -0 "$(cat "$PID_FILE")" 2>/dev/null; then
        echo "Running (PID: $(cat "$PID_FILE"))"
    else
        echo "Stopped"
    fi
}

case "${1:-}" in
    start)   start_service ;;
    stop)    stop_service ;;
    restart) stop_service; start_service ;;
    status)  status_service ;;
    *)       echo "Usage: $0 {start|stop|restart|status}" ;;
esac
```

---

## Arrays and Data Structures

```bash
# Indexed array
declare -a files=("file1.txt" "file2.txt" "file3.txt")
files+=("file4.txt")                    # Append
echo "${files[0]}"                      # Access by index
echo "${#files[@]}"                     # Length
echo "${files[@]}"                      # All elements

# Associative array (bash 4+)
declare -A config
config[host]="localhost"
config[port]="8080"
config[debug]="true"

for key in "${!config[@]}"; do
    echo "$key = ${config[$key]}"
done

# Array iteration
for item in "${files[@]}"; do
    echo "Processing: $item"
done
```

---

## String Operations

```bash
# Substring
str="Hello, World!"
echo "${str:0:5}"           # Hello
echo "${str:7}"             # World!

# Replacement
echo "${str/World/Bash}"    # Hello, Bash!
echo "${str//l/L}"          # HeLLo, WorLd! (all occurrences)

# Length
echo "${#str}"              # 13

# Default values
echo "${VAR:-default}"      # Use default if VAR is unset/empty
echo "${VAR:=default}"      # Set AND use default if unset/empty

# Trim
echo "${str#Hello, }"       # World! (remove prefix)
echo "${str%!}"             # Hello, World (remove suffix)

# Pattern-based path manipulation
filepath="/home/user/docs/report.tar.gz"
echo "${filepath##*/}"      # report.tar.gz (basename)
echo "${filepath%/*}"       # /home/user/docs (dirname)
echo "${filepath%%.*}"      # /home/user/docs/report (remove all extensions)
echo "${filepath##*.}"      # gz (last extension)
```

---

## Error Handling Patterns

```bash
# Trap for cleanup
trap cleanup EXIT
trap 'die "Interrupted"' INT TERM

# Retry with backoff
retry() {
    local max_attempts="$1"
    shift
    local attempt=1
    while true; do
        if "$@"; then
            return 0
        fi
        if ((attempt >= max_attempts)); then
            log_error "Failed after $max_attempts attempts: $*"
            return 1
        fi
        local delay=$((attempt * 2))
        log_warn "Attempt $attempt/$max_attempts failed, retrying in ${delay}s..."
        sleep "$delay"
        ((attempt++))
    done
}

# Usage: retry 3 curl -fsSL http://example.com

# Assert function
assert() {
    local condition="$1" message="${2:-Assertion failed}"
    if ! eval "$condition"; then
        die "$message"
    fi
}

# Usage: assert '[[ -f "$config_file" ]]' "Config file not found: $config_file"
```

---

## Security Best Practices

1. **Never use eval with user input** - command injection risk
2. **Quote all variables** - prevents word splitting and globbing
3. **Use `--` to end option parsing** - prevents flag injection: `rm -- "$file"`
4. **Validate all inputs** before using them
5. **Restrict PATH** at script start: `export PATH="/usr/local/bin:/usr/bin:/bin"`
6. **Use `mktemp`** for temp files - never hardcode temp paths
7. **Set umask** for file creation: `umask 077`
8. **Avoid storing secrets** in scripts - use env vars or secret managers
9. **Use `readonly`** for constants to prevent accidental modification
10. **Check return codes** - never ignore command failures

```bash
# Input validation
validate_input() {
    local input="$1"
    # Reject dangerous characters
    if [[ "$input" =~ [\'\"\\$\`\;] ]]; then
        die "Invalid characters in input: $input"
    fi
}
```

---

## Performance Tips

```bash
# Prefer built-in over external commands
# SLOW: result=$(echo "$str" | tr '[:upper:]' '[:lower:]')
# FAST:
result="${str,,}"

# SLOW: count=$(echo "$str" | wc -c)
# FAST:
count="${#str}"

# Use printf over echo for portability
printf '%s\n' "$message"

# Parallel execution
parallel_exec() {
    local max_jobs="${1:-4}"
    shift
    local pids=()
    for cmd in "$@"; do
        eval "$cmd" &
        pids+=($!)
        if ((${#pids[@]} >= max_jobs)); then
            wait "${pids[0]}"
            pids=("${pids[@]:1}")
        fi
    done
    wait
}

# Process substitution (avoid temp files)
diff <(sort file1.txt) <(sort file2.txt)
```

---

## Checklist Before Committing Any Shell Script

1. [ ] Shebang line present (`#!/usr/bin/env bash`)
2. [ ] `set -euo pipefail` at the top
3. [ ] All variables quoted (`"$var"`)
4. [ ] `[[ ]]` used instead of `[ ]` (for bash scripts)
5. [ ] Temp files created with `mktemp` and cleaned up via trap
6. [ ] Input validation for user-provided arguments
7. [ ] Meaningful exit codes (0=success, 1=error)
8. [ ] Logging with timestamps
9. [ ] Usage/help function present
10. [ ] ShellCheck passes (`shellcheck script.sh`)

---

## Version History

- v1.0.0 (2026-03-07): Initial skill creation
  - Core rules (strict mode, quoting, test syntax)
  - Production script template
  - 8 architecture patterns (file processing, text, process, temp, lock, network, cron, service)
  - Arrays, strings, error handling, security, performance
