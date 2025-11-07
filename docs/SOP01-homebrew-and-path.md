# SOP01 – Homebrew Install & PATH Ordering

## Objective
Install Homebrew packages and ensure modern GNU tools (coreutils, gawk, etc.) and Homebrew binaries are prioritised in `$PATH` while remaining safe for macOS.

## Steps
1. **Ensure Homebrew is installed (Apple Silicon default path)**  
   https://brew.sh  
   Homebrew prefix: `/opt/homebrew`

2. **Install required formulae (subset relevant to this workstation)**  
   ```bash
   brew install \
     bash coreutils gawk gettext colordiff git git-delta grc glow \
     openssh gh chezmoi gmp mpfr libunistring oniguruma pcre2 \
     python@3.14 pipx nano readline zstd xz lz4 sqlite nmap ldns libssh2 \
     libgit2 libfido2 mpdecimal ncurses openssl@3 duti
