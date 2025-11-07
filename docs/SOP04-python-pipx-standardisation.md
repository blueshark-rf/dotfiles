## SOP04 – Python and Pipx Standardisation
**Version:** 1.0  
**Date:** 2025-11-07  
**Author:** R. Field  
**Change Description:** Defines standard Python and pipx configuration for client build consistency and tool isolation.

### 1. Purpose
This SOP standardises the Python environment and pipx configuration across all macOS client systems to ensure consistent and isolated tool management.

### 2. Scope
Covers Python (3.14.0), pipx, and CLI tools installed through pipx.

### 3. Procedure
1. Install and link Python:
```bash
brew install python@3.14
brew link python@3.14 --overwrite --force
```
2. Install and verify pipx:
```bash
brew install pipx
pipx --version
```
3. Define default interpreter:
```bash
export PIPX_DEFAULT_PYTHON="$(command -v python3)"
```
4. Reinstall pipx tools:
```bash
pipx reinstall-all
```
5. Verify:
```bash
pipx list
```

### 4. Troubleshooting
| Issue | Resolution |
|-------|-------------|
| pipx can't find Python | Ensure brew Python is first in PATH. |
| Reinstall errors | Run `pipx reinstall-all`. |

### 5. Validation Policy
Validated against:  
- [Python.org documentation](https://docs.python.org/3/using/mac.html)  
- [pipx docs](https://pypa.github.io/pipx/)  
- [Homebrew Python formula](https://formulae.brew.sh/formula/python@3.14)

### 6. Verification Trace Appendix
| Validation Item | Source | Verification Method | Result |
|-----------------|---------|--------------------|---------|
| Python version check | python3 --version | Verified 3.14.0 | ✅ |
| Pipx environment | pipx list | Verified | ✅ |
