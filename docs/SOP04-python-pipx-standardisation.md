
---

#### `docs/SOP05-dotfiles-audit-and-reporting.md`
```markdown
# SOP05 – Dotfiles Audit & Reporting

## Objective
Generate a CSV inventory of dotfiles, highlight large files, broken symlinks, and errors, and provide quick gawk queries.

## Generate report
I keep an audit script at `~/bin/audit_dotfiles.py` producing `~/dotfiles_audit.csv`. After running it:

```bash
ls -la ~/dotfiles_audit.csv
