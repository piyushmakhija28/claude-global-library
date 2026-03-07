---
name: powershell-scripting
version: 1.0.0
last-updated: 2026-03-07
description: PowerShell scripting for Windows/cross-platform automation - cmdlets, modules, pipeline, remote management, registry, services, Active Directory, scheduled tasks, and production-grade script patterns.
allowed-tools: Read,Glob,Grep,Bash,Edit,Write
user-invocable: true
---

# SKILL.md
## PowerShell Scripting Skill Instructions

### Skill Name
powershell-scripting

### Description
Comprehensive skill for writing production-grade PowerShell scripts. Covers PowerShell 5.1 (Windows) and PowerShell 7+ (cross-platform), cmdlet design, pipeline processing, module creation, error handling, remote management, registry operations, service management, Active Directory, scheduled tasks, and security best practices.

---

## Core Rules (NON-NEGOTIABLE)

### Rule 1: Use Approved Verbs
```powershell
# PowerShell enforces verb-noun naming for functions/cmdlets
# CORRECT
function Get-UserData { }
function Set-Configuration { }
function New-BackupArchive { }
function Remove-TempFiles { }

# WRONG
function FetchUserData { }
function UpdateConfig { }

# List all approved verbs
Get-Verb | Sort-Object Verb
```

### Rule 2: CmdletBinding and Parameter Validation (MANDATORY)
```powershell
function Invoke-MyTask {
    [CmdletBinding(SupportsShouldProcess)]
    param(
        [Parameter(Mandatory, Position = 0, ValueFromPipeline)]
        [ValidateNotNullOrEmpty()]
        [string]$Path,

        [Parameter()]
        [ValidateRange(1, 100)]
        [int]$RetryCount = 3,

        [Parameter()]
        [ValidateSet('Debug', 'Info', 'Warn', 'Error')]
        [string]$LogLevel = 'Info',

        [switch]$Force
    )

    process {
        if ($PSCmdlet.ShouldProcess($Path, "Process file")) {
            # ... logic ...
        }
    }
}
```

### Rule 3: Strict Mode (MANDATORY for all scripts)
```powershell
Set-StrictMode -Version Latest
$ErrorActionPreference = 'Stop'

# This catches:
# - Uninitialized variables
# - Non-existent properties
# - Function calls using () instead of spaces
# - Other common mistakes
```

### Rule 4: UTF-8 Output (Windows Compatibility)
```powershell
# Set UTF-8 encoding for console and file output
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8

# File operations - always specify encoding
Get-Content -Path $file -Encoding UTF8
Set-Content -Path $file -Value $content -Encoding UTF8
Out-File -FilePath $file -Encoding UTF8
```

---

## Script Template (Production-Grade)

```powershell
#Requires -Version 5.1
#Requires -RunAsAdministrator  # Only if needed

<#
.SYNOPSIS
    Brief one-line description of the script.

.DESCRIPTION
    Detailed description of what the script does,
    when to use it, and any prerequisites.

.PARAMETER Path
    The target path to process.

.PARAMETER LogLevel
    Logging verbosity level.

.EXAMPLE
    .\Script-Name.ps1 -Path "C:\Data" -LogLevel Info

.EXAMPLE
    Get-ChildItem *.txt | .\Script-Name.ps1 -Verbose

.NOTES
    Version: 1.0.0
    Author: Auto-generated
    Date: YYYY-MM-DD
#>

[CmdletBinding(SupportsShouldProcess)]
param(
    [Parameter(Mandatory, Position = 0)]
    [ValidateScript({ Test-Path $_ })]
    [string]$Path,

    [ValidateSet('Debug', 'Info', 'Warn', 'Error')]
    [string]$LogLevel = 'Info'
)

# ─── Strict Mode ────────────────────────────────────────────
Set-StrictMode -Version Latest
$ErrorActionPreference = 'Stop'

# ─── Constants ──────────────────────────────────────────────
$Script:ScriptDir = $PSScriptRoot
$Script:ScriptName = $MyInvocation.MyCommand.Name
$Script:LogFile = Join-Path $env:TEMP "$($Script:ScriptName).log"

# ─── Logging ────────────────────────────────────────────────

function Write-Log {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Message,

        [ValidateSet('DEBUG', 'INFO', 'WARN', 'ERROR')]
        [string]$Level = 'INFO'
    )

    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    $entry = "[$timestamp] [$Level] $Message"
    Add-Content -Path $Script:LogFile -Value $entry -Encoding UTF8 -ErrorAction SilentlyContinue
    switch ($Level) {
        'DEBUG' { Write-Verbose $entry }
        'INFO'  { Write-Host $entry }
        'WARN'  { Write-Warning $Message }
        'ERROR' { Write-Error $Message }
    }
}

# ─── Functions ──────────────────────────────────────────────

function Invoke-MainLogic {
    [CmdletBinding()]
    param()

    Write-Log "Starting $Script:ScriptName" -Level INFO

    # ... script logic here ...

    Write-Log "Completed successfully" -Level INFO
}

# ─── Entry Point ────────────────────────────────────────────

try {
    Invoke-MainLogic
}
catch {
    Write-Log "Fatal error: $_" -Level ERROR
    Write-Log $_.ScriptStackTrace -Level ERROR
    exit 1
}
finally {
    # Cleanup code here (temp files, connections, etc.)
}
```

---

## Essential Patterns

### Pattern 1: Pipeline Processing
```powershell
# Process objects through the pipeline
function ConvertTo-CleanData {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory, ValueFromPipeline)]
        [psobject]$InputObject
    )

    begin {
        # Initialize (runs once before pipeline)
        $count = 0
    }

    process {
        # Process each pipeline object
        $count++
        [PSCustomObject]@{
            Index     = $count
            Name      = $InputObject.Name.Trim()
            Size      = $InputObject.Length
            Modified  = $InputObject.LastWriteTime
        }
    }

    end {
        # Finalize (runs once after pipeline)
        Write-Verbose "Processed $count items"
    }
}

# Usage
Get-ChildItem -File | ConvertTo-CleanData | Export-Csv -Path output.csv -NoTypeInformation
```

### Pattern 2: Error Handling
```powershell
# Try-Catch-Finally
try {
    $result = Invoke-WebRequest -Uri $url -ErrorAction Stop
}
catch [System.Net.WebException] {
    Write-Log "Network error: $($_.Exception.Message)" -Level ERROR
    # Handle specific exception type
}
catch {
    Write-Log "Unexpected error: $_" -Level ERROR
    throw  # Re-throw if can't handle
}
finally {
    # Always executes - cleanup here
}

# Retry with backoff
function Invoke-WithRetry {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [scriptblock]$ScriptBlock,

        [int]$MaxRetries = 3,

        [int]$DelaySeconds = 2
    )

    $attempt = 0
    while ($true) {
        $attempt++
        try {
            return & $ScriptBlock
        }
        catch {
            if ($attempt -ge $MaxRetries) {
                Write-Log "Failed after $MaxRetries attempts: $_" -Level ERROR
                throw
            }
            $wait = $DelaySeconds * $attempt
            Write-Log "Attempt $attempt/$MaxRetries failed, retrying in ${wait}s..." -Level WARN
            Start-Sleep -Seconds $wait
        }
    }
}

# Usage
$data = Invoke-WithRetry -MaxRetries 5 -ScriptBlock {
    Invoke-RestMethod -Uri 'https://api.example.com/data'
}
```

### Pattern 3: File and Directory Operations
```powershell
# Safe file operations
function Invoke-FileProcessing {
    [CmdletBinding()]
    param([string]$SourceDir, [string]$DestDir)

    # Ensure destination exists
    if (-not (Test-Path $DestDir)) {
        New-Item -ItemType Directory -Path $DestDir -Force | Out-Null
    }

    # Process files with progress
    $files = Get-ChildItem -Path $SourceDir -Recurse -File
    $total = $files.Count
    $current = 0

    foreach ($file in $files) {
        $current++
        $pct = [math]::Round(($current / $total) * 100)
        Write-Progress -Activity "Processing files" -Status "$file" -PercentComplete $pct

        $destPath = Join-Path $DestDir $file.Name
        Copy-Item -Path $file.FullName -Destination $destPath -Force
    }

    Write-Progress -Activity "Processing files" -Completed
}

# CSV processing
Import-Csv -Path data.csv | Where-Object { $_.Status -eq 'Active' } |
    ForEach-Object { $_.Name.ToUpper() } |
    Export-Csv -Path filtered.csv -NoTypeInformation

# JSON processing
$config = Get-Content -Path config.json -Raw | ConvertFrom-Json
$config.setting = 'new-value'
$config | ConvertTo-Json -Depth 10 | Set-Content -Path config.json -Encoding UTF8

# XML processing
[xml]$xml = Get-Content -Path data.xml
$nodes = $xml.SelectNodes('//item[@status="active"]')
foreach ($node in $nodes) {
    $node.SetAttribute('processed', 'true')
}
$xml.Save("$PWD\data.xml")
```

### Pattern 4: Registry Operations (Windows)
```powershell
# Read registry
$value = Get-ItemPropertyValue -Path 'HKLM:\SOFTWARE\MyApp' -Name 'InstallPath' -ErrorAction SilentlyContinue

# Write registry
function Set-RegistryValue {
    [CmdletBinding(SupportsShouldProcess)]
    param(
        [string]$Path,
        [string]$Name,
        [object]$Value,
        [string]$Type = 'String'
    )

    if (-not (Test-Path $Path)) {
        New-Item -Path $Path -Force | Out-Null
    }

    if ($PSCmdlet.ShouldProcess("$Path\$Name", "Set registry value")) {
        Set-ItemProperty -Path $Path -Name $Name -Value $Value -Type $Type
    }
}

# Enumerate registry
Get-ChildItem -Path 'HKLM:\SOFTWARE' | ForEach-Object { $_.Name }
```

### Pattern 5: Service Management
```powershell
# Check service status
function Test-ServiceHealth {
    [CmdletBinding()]
    param([string[]]$ServiceName)

    foreach ($svc in $ServiceName) {
        $service = Get-Service -Name $svc -ErrorAction SilentlyContinue
        if (-not $service) {
            Write-Log "Service not found: $svc" -Level WARN
            continue
        }

        [PSCustomObject]@{
            Name   = $service.Name
            Status = $service.Status
            Startup = $service.StartType
            Healthy = ($service.Status -eq 'Running')
        }
    }
}

# Restart service with timeout
function Restart-ServiceSafe {
    [CmdletBinding(SupportsShouldProcess)]
    param(
        [Parameter(Mandatory)]
        [string]$ServiceName,
        [int]$TimeoutSeconds = 60
    )

    if ($PSCmdlet.ShouldProcess($ServiceName, "Restart service")) {
        Restart-Service -Name $ServiceName -Force
        $sw = [System.Diagnostics.Stopwatch]::StartNew()
        while ((Get-Service $ServiceName).Status -ne 'Running') {
            if ($sw.Elapsed.TotalSeconds -gt $TimeoutSeconds) {
                throw "Service $ServiceName did not start within ${TimeoutSeconds}s"
            }
            Start-Sleep -Milliseconds 500
        }
        Write-Log "Service $ServiceName restarted successfully" -Level INFO
    }
}
```

### Pattern 6: Remote Management
```powershell
# Remote execution via PSRemoting
$cred = Get-Credential
$session = New-PSSession -ComputerName 'server01' -Credential $cred

# Run command on remote machine
Invoke-Command -Session $session -ScriptBlock {
    Get-Process | Where-Object CPU -gt 50
}

# Copy files to remote
Copy-Item -Path .\deploy.zip -Destination 'C:\Deploy\' -ToSession $session

# Multi-machine parallel execution
$servers = @('web01', 'web02', 'web03')
Invoke-Command -ComputerName $servers -ScriptBlock {
    Restart-Service -Name 'W3SVC' -Force
    Write-Output "$env:COMPUTERNAME: IIS restarted"
} -Credential $cred -ThrottleLimit 5

# Cleanup
Remove-PSSession $session
```

### Pattern 7: Scheduled Tasks
```powershell
# Create a scheduled task
function Register-MaintenanceTask {
    [CmdletBinding(SupportsShouldProcess)]
    param(
        [string]$TaskName = 'DailyCleanup',
        [string]$ScriptPath,
        [string]$Schedule = 'Daily',
        [string]$Time = '02:00'
    )

    $action = New-ScheduledTaskAction -Execute 'pwsh.exe' -Argument "-NoProfile -File `"$ScriptPath`""
    $trigger = switch ($Schedule) {
        'Daily'   { New-ScheduledTaskTrigger -Daily -At $Time }
        'Weekly'  { New-ScheduledTaskTrigger -Weekly -DaysOfWeek Monday -At $Time }
        'Hourly'  { New-ScheduledTaskTrigger -Once -At $Time -RepetitionInterval (New-TimeSpan -Hours 1) }
    }
    $settings = New-ScheduledTaskSettingsSet -StartWhenAvailable -DontStopOnIdleEnd

    if ($PSCmdlet.ShouldProcess($TaskName, "Register scheduled task")) {
        Register-ScheduledTask -TaskName $TaskName -Action $action -Trigger $trigger -Settings $settings -User 'SYSTEM' -Force
    }
}

# Query and manage tasks
Get-ScheduledTask -TaskName 'DailyCleanup' | Get-ScheduledTaskInfo
Disable-ScheduledTask -TaskName 'DailyCleanup'
Unregister-ScheduledTask -TaskName 'DailyCleanup' -Confirm:$false
```

### Pattern 8: Module Creation
```powershell
# Module manifest (MyModule.psd1)
# Generate with: New-ModuleManifest -Path MyModule.psd1

# Module script (MyModule.psm1)
# Private helper (not exported)
function Resolve-InternalPath {
    param([string]$Path)
    [System.IO.Path]::GetFullPath($Path)
}

# Public functions (exported)
function Get-ModuleData {
    [CmdletBinding()]
    param([string]$Name)
    # ... implementation ...
}

function Set-ModuleData {
    [CmdletBinding(SupportsShouldProcess)]
    param([string]$Name, [object]$Value)
    # ... implementation ...
}

# Export only public functions
Export-ModuleMember -Function 'Get-ModuleData', 'Set-ModuleData'
```

---

## Objects and Data Structures

```powershell
# Custom objects
$obj = [PSCustomObject]@{
    Name    = 'Server01'
    IP      = '192.168.1.10'
    Status  = 'Online'
    CPU     = 45.2
}

# Hashtable (dictionary)
$config = @{
    Host    = 'localhost'
    Port    = 8080
    Debug   = $true
}
$config['Timeout'] = 30

# Ordered hashtable (preserves insertion order)
$ordered = [ordered]@{
    First  = 1
    Second = 2
    Third  = 3
}

# Array operations
$arr = @(1, 2, 3, 4, 5)
$arr += 6                          # Append (creates new array)
$filtered = $arr | Where-Object { $_ -gt 3 }
$transformed = $arr | ForEach-Object { $_ * 2 }

# ArrayList (better for frequent adds)
$list = [System.Collections.ArrayList]::new()
[void]$list.Add('item1')
[void]$list.Add('item2')

# Generic List (typed, best performance)
$typedList = [System.Collections.Generic.List[string]]::new()
$typedList.Add('item1')
```

---

## String Operations

```powershell
# String interpolation
$name = "World"
"Hello, $name!"                                    # Variable expansion
"Count: $($items.Count)"                           # Expression expansion
"Path: $(Join-Path $root 'sub')"                   # Command expansion

# Here-strings (multiline)
$multiline = @"
Line 1: $name
Line 2: $(Get-Date)
"@

# Literal here-string (no expansion)
$literal = @'
No $expansion here
Just literal text
'@

# String methods
'Hello'.ToUpper()                                  # HELLO
'Hello'.ToLower()                                  # hello
'  spaces  '.Trim()                                # spaces
'hello world'.Replace('world', 'powershell')       # hello powershell
'a,b,c'.Split(',')                                 # @('a','b','c')
'one two three' -split '\s+'                       # @('one','two','three')
@('a','b','c') -join ', '                          # a, b, c

# Regex
'Hello123' -match '(\d+)'                          # True, $Matches[1] = '123'
'Hello123' -replace '\d+', 'NUM'                   # HelloNUM
[regex]::Matches('a1b2c3', '\d') | ForEach-Object { $_.Value }  # 1, 2, 3

# Format strings
'{0:N2}' -f 1234.5                                 # 1,234.50
'{0:yyyy-MM-dd}' -f (Get-Date)                     # 2026-03-07
```

---

## Security Best Practices

1. **Never use `Invoke-Expression` with user input** - code injection risk
2. **Use `SecureString` for passwords** - never store plaintext credentials
3. **Validate all parameters** with `[Validate*]` attributes
4. **Use `SupportsShouldProcess`** for destructive operations (-WhatIf/-Confirm)
5. **Set execution policy** appropriately - never use `Bypass` in production
6. **Use credential objects** - never pass passwords as plain strings
7. **Sign scripts** in production - use code signing certificates
8. **Constrained Language Mode** for untrusted environments
9. **Use `[SecureString]` and `Export-Clixml`** for stored credentials
10. **Audit with transcript logging** - `Start-Transcript`

```powershell
# Secure credential handling
$credential = Get-Credential

# Store credential securely (per-user, per-machine encryption)
$credential | Export-Clixml -Path "$env:USERPROFILE\cred.xml"

# Retrieve stored credential
$savedCred = Import-Clixml -Path "$env:USERPROFILE\cred.xml"

# SecureString conversion (when API requires plain text)
$ptr = [Runtime.InteropServices.Marshal]::SecureStringToBSTR($savedCred.Password)
try {
    $plainText = [Runtime.InteropServices.Marshal]::PtrToStringBSTR($ptr)
}
finally {
    [Runtime.InteropServices.Marshal]::ZeroFreeBSTR($ptr)
}

# Transcript for audit
Start-Transcript -Path "C:\Logs\session-$(Get-Date -Format 'yyyyMMdd-HHmmss').log"
# ... operations ...
Stop-Transcript
```

---

## Performance Tips

```powershell
# Use pipeline filtering early (reduces objects in pipeline)
# SLOW:
Get-ChildItem -Recurse | Where-Object { $_.Extension -eq '.log' }
# FAST:
Get-ChildItem -Recurse -Filter '*.log'

# Use -Filter over -Include (provider-level filtering)
Get-ChildItem -Path $dir -Filter '*.txt'       # Fast (provider)
Get-ChildItem -Path $dir -Include '*.txt'       # Slow (PowerShell)

# StringBuilder for string concatenation
$sb = [System.Text.StringBuilder]::new()
foreach ($item in $largeCollection) {
    [void]$sb.AppendLine($item)
}
$result = $sb.ToString()

# Avoid repeated array concatenation
# SLOW: $arr += $item (creates new array each time)
# FAST:
$list = [System.Collections.Generic.List[object]]::new()
foreach ($item in $source) {
    $list.Add($item)
}

# Parallel processing (PowerShell 7+)
1..10 | ForEach-Object -Parallel {
    Start-Sleep -Seconds 1
    "Processed $_"
} -ThrottleLimit 5

# Measure performance
Measure-Command { Get-ChildItem -Recurse }

# Suppress output for void operations
[void]$list.Add($item)     # Best
$null = $list.Add($item)   # Also good
$list.Add($item) | Out-Null  # Slowest - avoid
```

---

## Cross-Platform Considerations (PowerShell 7+)

```powershell
# Detect platform
if ($IsWindows) { }
elseif ($IsLinux) { }
elseif ($IsMacOS) { }

# Cross-platform paths
$configDir = if ($IsWindows) {
    $env:APPDATA
} else {
    Join-Path $HOME '.config'
}

# Line endings
$content = Get-Content -Path $file -Raw
$content = $content -replace "`r`n", "`n"   # Normalize to Unix
$content = $content -replace "(?<!\r)`n", "`r`n"  # Normalize to Windows

# Cross-platform temp directory
$tempDir = [System.IO.Path]::GetTempPath()
```

---

## Checklist Before Committing Any PowerShell Script

1. [ ] `Set-StrictMode -Version Latest` present
2. [ ] `$ErrorActionPreference = 'Stop'` set
3. [ ] All functions use `[CmdletBinding()]`
4. [ ] Approved verbs used (Get-, Set-, New-, Remove-, etc.)
5. [ ] Parameter validation attributes used (`[Validate*]`)
6. [ ] `SupportsShouldProcess` on destructive functions
7. [ ] Comment-based help (`.SYNOPSIS`, `.DESCRIPTION`, `.EXAMPLE`)
8. [ ] Error handling with try-catch-finally
9. [ ] No plain-text credentials
10. [ ] PSScriptAnalyzer passes (`Invoke-ScriptAnalyzer -Path script.ps1`)

---

## Version History

- v1.0.0 (2026-03-07): Initial skill creation
  - Core rules (approved verbs, CmdletBinding, strict mode, UTF-8)
  - Production script template
  - 8 architecture patterns (pipeline, error handling, files, registry, services, remote, tasks, modules)
  - Objects, strings, security, performance, cross-platform
