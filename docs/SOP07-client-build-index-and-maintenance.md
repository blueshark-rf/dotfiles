## SOP07 – Client Build Index and Maintenance
**Version:** 1.0  
**Date:** 2025-11-07  
**Author:** Robert Field  
**Change Description:** Establishes maintenance and validation processes for client build systems managed under Chezmoi and GitHub.

### 1. Purpose
Defines lifecycle management and verification of all client configuration artefacts managed under `chezmoi`, ensuring reproducibility and integrity.

### 2. Scope
Applies to macOS client systems using Homebrew, pipx, chezmoi, and GitHub for configuration storage and synchronisation.

### 3. Procedure

#### 3.1 Validate Environment
```bash
brew doctor
chezmoi doctor
pipx list
git status
```
Confirm all outputs are clean and no symbolic link or permission errors occur.

#### 3.2 Update Components
```bash
brew update && brew upgrade
pipx upgrade-all
chezmoi update
chezmoi add ~/.zshrc ~/.gitconfig ~/.chromaterm.yml
chezmoi git -- add .
chezmoi git -- commit -m "Periodic maintenance: update dotfiles and tooling"
chezmoi git -- push
```

#### 3.3 Validate Core Configuration
| SOP | Task |
|-----|------|
| SOP01 | Confirm PATH and Homebrew config |
| SOP02 | Validate Git and delta output |
| SOP03 | Test SSH authentication |
| SOP04 | Confirm Python and pipx environment |
| SOP05 | Run dotfile audit |
| SOP06 | Validate Chromaterm configuration |

#### 3.4 Maintenance Schedule
| Frequency | Task |
|------------|------|
| Weekly | Run brew & pipx upgrades |
| Monthly | Run dotfile audit |
| Quarterly | Review SOPs and update GitHub repo |

#### 3.5 Rebuild Readiness
Before rebuild:
```bash
chezmoi init git@github.com:blueshark-rf/dotfiles.git
chezmoi apply
```

#### 3.6 Backup Repository
```bash
git clone --mirror git@github.com:blueshark-rf/dotfiles.git ~/Backups/dotfiles-backup-$(date +%Y%m%d)
```

### 4. Troubleshooting
| Issue | Resolution |
|-------|-------------|
| chezmoi changes not applied | Run `chezmoi diff` and confirm templates |
| brew/pipx errors | Re-run `brew doctor` or reinstall dependencies |
| SSH auth failures | Verify keys and permissions |

### 5. Validation Policy
Validated against:
- https://www.chezmoi.io/user-guide/
- https://docs.brew.sh/
- https://pypa.github.io/pipx/
- https://docs.github.com/en/authentication/connecting-to-github-with-ssh

### 6. Verification Trace Appendix
| Validation Item | Source | Verification Method | Result |
|-----------------|---------|--------------------|---------|
| Chezmoi validation | chezmoi doctor | OK | ✅ |
| Brew upgrade test | brew doctor | Clean | ✅ |
| Pipx environment | pipx list | OK | ✅ |
| Dotfile sync | chezmoi push | Verified | ✅ |
