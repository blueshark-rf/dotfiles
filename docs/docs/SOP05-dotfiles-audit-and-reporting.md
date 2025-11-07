## SOP05 – Dotfiles Audit and Reporting
**Version:** 1.0  
**Date:** 2025-11-07  
**Author:** R. Field  
**Change Description:** Establishes procedure for auditing, verifying, and maintaining dotfiles consistency across client systems.

### 1. Purpose
This SOP defines a repeatable and automated process for auditing hidden configuration files (“dotfiles”) in a user's home directory.

### 2. Scope
Applies to macOS client systems managed by chezmoi.

### 3. Procedure
Create an audit script:
```bash
nano ~/bin/audit_dotfiles.py
chmod +x ~/bin/audit_dotfiles.py
```
Paste Python code to hash and report dotfiles into CSV (as detailed earlier).  
Run:
```bash
~/bin/audit_dotfiles.py
column -t -s, ~/dotfiles_audit.csv | less -S
```

### 4. Troubleshooting
| Issue | Resolution |
|-------|-------------|
| Script fails | Ensure Python 3.14 installed and executable. |
| Missing `awk` features | Install `gawk` via Homebrew. |

### 5. Validation Policy
Validated against:  
- [Python 3.14 docs](https://docs.python.org/3/library/os.html)  
- [Homebrew gawk formula](https://formulae.brew.sh/formula/gawk)  
- [Chezmoi documentation](https://www.chezmoi.io/user-guide/)  

### 6. Verification Trace Appendix
| Validation Item | Source | Verification Method | Result |
|-----------------|---------|--------------------|---------|
| Python audit | Script output | CSV verified | ✅ |
| Chezmoi tracking | chezmoi add/apply | Confirmed | ✅ |
