
---

#### `docs/SOP04-python-pipx-standardisation.md`
```markdown
# SOP04 – Python & pipx Standardisation

## Objective
Keep one active Homebrew Python (3.14) and make pipx use it for isolated CLI apps.

## Steps
1. **Retain only Python 3.14**
   ```bash
   brew uninstall python@3.12 python@3.13
   brew autoremove
   python3 -V
   which -a python3
