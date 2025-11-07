## SOP06 – Chromaterm Configuration and Colour Management
**Version:** 1.0  
**Date:** 2025-11-07  
**Author:** R. Field  
**Change Description:** Defines Chromaterm installation, configuration, and colour management.

### 1. Purpose
Provides a consistent configuration for Chromaterm colour filtering.

### 2. Scope
Applies to all macOS clients using pipx-managed Chromaterm.

### 3. Procedure
Install:
```bash
pipx install chromaterm
```
Reinstall if required:
```bash
pipx reinstall chromaterm --python "$(command -v python3)"
```
Verify:
```bash
ct --version
```
Add configuration under `~/.chromaterm.yml`:
```yaml
filters:
  - regex: '(ERROR|WARN)'
    colour: red bold
  - regex: '(OK|Success)'
    colour: green
```

### 4. Troubleshooting
| Issue | Resolution |
|-------|-------------|
| `ct: command not found` | Add `~/.local/bin` to PATH. |
| Config parse errors | Validate YAML syntax. |

### 5. Validation Policy
Validated against:  
- [Chromaterm GitHub](https://github.com/hSaria/Chromaterm)  
- [pipx documentation](https://pypa.github.io/pipx/)  
- [Chezmoi guide](https://www.chezmoi.io/user-guide/)

### 6. Verification Trace Appendix
| Validation Item | Source | Verification Method | Result |
|-----------------|---------|--------------------|---------|
| Chromaterm version | pipx list | Verified 0.10.7 | ✅ |
| YAML rules | config diff | Verified identical | ✅ |
