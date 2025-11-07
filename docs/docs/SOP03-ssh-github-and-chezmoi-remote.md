## SOP03 – SSH, GitHub Authentication and Chezmoi Remote Setup
**Version:** 1.0  
**Date:** 2025-11-07  
**Author:** R. Field  
**Change Description:** Defines procedure for SSH key management, GitHub authentication, and linking Chezmoi to a remote repository.

### 1. Purpose
This SOP standardises the authentication process for managing GitHub repositories and remote `chezmoi` configurations securely.

### 2. Scope
Applies to all macOS client systems used for configuration management, automation, or client build replication.

### 3. Procedure
#### 3.1 Generate SSH Keys
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```
Add your public key to GitHub via https://github.com/settings/keys

#### 3.2 Test SSH Connectivity
```bash
ssh -T git@github.com
```
Expected output:
```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

#### 3.3 Link Chezmoi to GitHub Remote
```bash
chezmoi init git@github.com:<username>/dotfiles.git
chezmoi apply
```

### 4. Troubleshooting
| Issue | Resolution |
|-------|-------------|
| SSH agent not starting | Ensure macOS keychain integration is enabled. |
| GitHub key rejected | Verify correct public key and permissions. |
| Chezmoi reports permission errors | Validate SSH key ownership and permissions (600). |

### 5. Validation Policy
Validated against:  
- [GitHub SSH documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)  
- [Chezmoi official guide](https://www.chezmoi.io/user-guide/)  

### 6. Verification Trace Appendix
| Validation Item | Source | Verification Method | Result |
|-----------------|---------|--------------------|---------|
| SSH keypair creation | macOS ssh-keygen | Verified keygen output | ✅ |
| GitHub connectivity | ssh -T test | Successful auth | ✅ |
| Chezmoi remote setup | chezmoi apply | Repo sync confirmed | ✅ |
