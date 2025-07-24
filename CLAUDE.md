# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## **IMPORTANT**
When analyzing large codebases or multiple files that might exceed context limits, use the Gemini CLI with its massive context window. 


Use gemini -p when:
* Analyzing entire codebases or large directories
* Comparing multiple large files
* Need to understand project-wide patterns or architecture
* Checking for the presence of certain coding patterns across the entire codebase
* If you experience any file read errors due to "exceeds maximum allowed tokens" or similar error, use gemini.

Examples:


* gemini -p "@src/main.py Explain this file's purpose and structure"
* gemini -p "@src/ Summarize the architecture of this codebase"
* gemini -p "@src/ Are there any React hooks that handle WebSocket connections? List them with file paths"

## Repository Overview

This is a Windows Developer Box Automation repository containing scripts and configuration files to automate the setup and customization of Windows development environments. The repository focuses on privacy, security, and removing unwanted Windows features.

## Key Commands

### Running Scripts
All scripts require Administrator privileges. To run any script:
```batch
# Right-click and select "Run as Administrator", or:
runas /user:Administrator "script-name.bat"
```

### Testing Registry Changes
Before applying registry modifications, test them:
```batch
# Query a registry key to verify it exists
reg query "HKLM\Software\Policies\Microsoft\Windows\DataCollection"

# Export a registry key for backup
reg export "HKLM\Software\KeyPath" backup.reg
```

## Architecture and Structure

### Script Patterns
All scripts follow these patterns:

1. **Registry Modifications**: Use `reg add` commands
   ```batch
   reg add "HKCU\Path\To\Key" /v ValueName /t REG_DWORD /d 0 /f
   ```

2. **PowerShell Integration**: For complex operations
   ```batch
   PowerShell -Command "Get-AppxPackage 'PackageName' | Remove-AppxPackage"
   ```

3. **Service Management**: Via PowerShell
   ```batch
   PowerShell -Command "$serviceName = 'ServiceName'; ..."
   ```

### Key Registry Locations
- User settings: `HKCU\` (Current User)
- System settings: `HKLM\` (Local Machine)  
- Policy settings: `\Policies\Microsoft\`
- Edge policies: `HKLM\SOFTWARE\Policies\Microsoft\Edge`

### Script Purposes
- **bloat.bat**: Removes pre-installed Windows apps (bloatware)
- **customization.bat**: Modifies UI elements and user experience
- **privacy.bat**: Disables telemetry and data collection
- **edge-privacy.bat**: Configures Microsoft Edge for privacy (includes user prompts)
- **security.bat**: Implements security hardening measures
- **fake-mdm-enrollment.reg**: Simulates MDM enrollment for feature access

## Important Considerations

1. **Administrator Rights Required**: All scripts modify system settings and require elevation
2. **No Rollback Mechanism**: Scripts don't include undo functionality - backup registry before running
3. **Personal Use Focus**: These are personal automation scripts, not enterprise-ready solutions
4. **Windows Version Compatibility**: Scripts may need adjustment for different Windows versions
5. **Testing Required**: Always test scripts in a safe environment first

## Development Guidelines

When modifying or creating new scripts:

1. Use `::` for comments explaining each section
2. Group related settings together
3. Follow existing patterns for registry modifications
4. Include descriptive comments for complex PowerShell commands
5. Consider adding user prompts for optional features (see edge-privacy.bat for example)
6. Maintain consistent formatting with existing scripts
7. Test all registry paths before implementing changes

## Common Tasks

### Adding a New Privacy Setting
```batch
:: Description of what this disables
reg add "HKLM\Path\To\Key" /v ValueName /t REG_DWORD /d 0 /f
```

### Removing a New App
```batch
:: App Name
PowerShell -Command "Get-AppxPackage 'Package.Name' | Remove-AppxPackage"
```

### Disabling a Service
```batch
PowerShell -Command "$serviceName = 'ServiceName'; Stop-Service -Name $serviceName -Force -ErrorAction SilentlyContinue; Set-Service -Name $serviceName -StartupType Disabled -ErrorAction SilentlyContinue"
```