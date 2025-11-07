
---

#### `docs/SOP03-ssh-github-and-chezmoi-remote.md`
```markdown
# SOP03 – GitHub over SSH + chezmoi Remote

## Objective
Authenticate to GitHub using SSH keys and ensure chezmoi’s source repo pushes cleanly.

## Steps
1. **Generate key**
   ```bash
   mkdir -p ~/.ssh && chmod 700 ~/.ssh
   ssh-keygen -t ed25519 -C "blueshark-rf @ MBP M4" -f ~/.ssh/id_ed25519
