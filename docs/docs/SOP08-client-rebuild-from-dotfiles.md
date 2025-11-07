# SOP08 – Client Rebuild from Dotfiles Repository
Author: Richard Field (Italik Ltd)

## Objective
I describe a repeatable, low-touch process to rebuild a client machine from my dotfiles repository using `chezmoi`. This includes first-run bootstrap, applying config, validating the result, and handling common errors.

## Scope
- macOS client endpoints.
- My public dotfiles repo: https://github.com/blueshark-rf/dotfiles
- Assumes Homebrew and the core tooling from SOP01–SOP07 are present, or will be bootstrapped as part of this SOP.

## Prerequisites
- Network access to GitHub (https://github.com).
- One of the following for Git operations:
  - **SSH**: working key loaded in ssh-agent and authorised on GitHub.
  - **HTTPS**: GitHub CLI (`gh`) authenticated with HTTPS.
- Homebrew and `chezmoi` available locally (see SOP01, SOP03).
  - Homebrew: https://brew.sh
  - Chezmoi install reference: https://www.chezmoi.io/install/

## High‑Level Steps
1. Verify core tools.
2. Authenticate to GitHub (SSH or HTTPS).
3. Initialise `chezmoi` against the dotfiles repo.
4. Apply configuration to the working `$HOME`.
5. Validate and remediate any drift.
6. Record and push SOP changes, if required.

## Step 1 – Verify Core Tools
Run the following and confirm versions print without errors:
```bash
which -a brew git chezmoi gh || true
brew --version
git --version
chezmoi --version
gh --version
```

## Step 2 – Authenticate to GitHub
### Option A: SSH (preferred)
1. Ensure the key exists and is loaded:
```bash
ls -l ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub
ssh -T git@github.com
```
Expected output includes: `Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.`

2. If unauthorised:
   - Add the public key to GitHub: https://github.com/settings/keys
   - Ensure `ssh-agent` is running and the key is added:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Option B: HTTPS (via GitHub CLI)
```bash
gh auth login
# Choose: GitHub.com → HTTPS → Authenticate Git with your credentials → Login with a web browser
gh auth status
```

## Step 3 – Initialise Chezmoi from the Repo
### Using SSH
```bash
chezmoi init git@github.com:blueshark-rf/dotfiles.git
chezmoi apply
```

### Using HTTPS
```bash
chezmoi init https://github.com/blueshark-rf/dotfiles.git
chezmoi apply
```

Notes:
- `chezmoi init` sets the source to the repo but does not modify files until `apply` runs.
- If re-pointing from another remote, update the origin inside chezmoi’s source:
```bash
cd "$(chezmoi source-path)"
chezmoi git -- remote set-url origin git@github.com:blueshark-rf/dotfiles.git
```

## Step 4 – Validate the Applied Configuration
Run a dry‑run diff to ensure there’s no unexpected drift:
```bash
chezmoi diff
```
If the diff is expected, apply again:
```bash
chezmoi apply
```

Confirm key items:
```bash
# PATH ordering (Homebrew first, etc.)
print -l $path

# Git and delta
type -a git
git --version
git config --list | grep -E 'delta|pager'

# Chromaterm
command -v ct && ct -h || true

# Python & pipx standardisation
python3 -V
pipx list
```

## Step 5 – Resolve Common Errors
- **“.gitconfig has changed since chezmoi last wrote it?”**
  - The file was locally modified. Re‑import it to chezmoi’s source then re‑apply:
```bash
chezmoi re-add ~/.gitconfig
chezmoi apply
```
- **Permission or keychain issues with SSH**
  - See SOP03 for macOS ssh-agent and keychain guidance.
- **Brew PATH precedence not in effect**
  - Source the shell and rebuild zcompdump:
```bash
source ~/.zshrc
rm -f ~/.zcompdump*
exec $SHELL -l
```

## Step 6 – Commit Any Source Changes and Push
If you have updated the dotfiles source (e.g., re‑added `.gitconfig`):
```bash
cd "$(chezmoi source-path)"
chezmoi git -- add .
chezmoi git -- commit -m "Sync updated dotfiles"
chezmoi git -- push
```

## Optional – Fresh Machine Bootstrap
For a brand‑new client, the following bootstraps Homebrew and chezmoi in one go:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install chezmoi gh git-delta gawk grc coreutils nano colordiff gettext openssh
gh auth login
chezmoi init https://github.com/blueshark-rf/dotfiles.git
chezmoi apply
```

## Verification Checklist
- `chezmoi diff` returns no unexpected changes.
- `git --version` shows Homebrew Git.
- `git d` produces coloured diff via `delta`.
- `python3 -V` shows the expected interpreter and `pipx list` shows tools.
- Chromaterm (`ct`) is available and reads the managed configuration.
- SSH to GitHub confirms authenticated status (SSH) or `gh auth status` shows logged in (HTTPS).

## Rollback
If a change breaks the environment, restore from the last known good commit in the chezmoi source repository and re‑apply:
```bash
cd "$(chezmoi source-path)"
chezmoi git -- log --oneline -n 5
chezmoi git -- checkout <good_commit>
chezmoi apply
```

## Validation Policy
All steps in this SOP have been verified against the following vendor sources. Where behaviour may vary across OS releases or tool versions, I have noted the canonical references. Any statement not listed below is **Not validated**.

## References (Vendor Documentation – Full URLs)
- Chezmoi – Quick Start: https://www.chezmoi.io/quick-start/
- Chezmoi – Commands: https://www.chezmoi.io/reference/commands/
- Chezmoi – Working with Git: https://www.chezmoi.io/user-guide/advanced/working-with-git/
- GitHub – Generating SSH Keys: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
- GitHub – Testing SSH Connection: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection
- GitHub CLI – Authentication: https://cli.github.com/manual/gh_auth_login
- Homebrew – Install: https://brew.sh

## Verification Trace Appendix
- **Chezmoi init/apply behaviour** – Verified by running `chezmoi init` (SSH/HTTPS) followed by `chezmoi apply` on macOS 15; confirmed file creation and idempotence as per docs.
- **SSH authentication to GitHub** – Verified with `ssh -T git@github.com` returning success message; matches GitHub docs wording.
- **HTTPS auth via gh** – Verified `gh auth login` and `gh auth status` steps; aligns with official CLI manual.
- **Git delta integration** – Verified `git d` alias renders coloured diffs; `git config --list` contains `delta` keys from SOP02.
- **PATH precedence** – Verified `print -l $path` shows Homebrew paths before system.
- **Python/pipx standardisation** – Verified `python3 -V` and `pipx list` reflect the configured interpreter per SOP04.
