Custom DOOM Emacs config

## Requirements
- Emacs 29.1+ (native Windows build or GNU Emacs on macOS)
- Git, `ripgrep`, and `fd` available in `$PATH`
- Optional but recommended: GitHub CLI (`gh`) for the Copilot GPT backend

macOS (Homebrew):
```bash
brew install emacs ripgrep fd git gh
```

Windows 11 (winget):
```powershell
winget install GNU.Emacs ripgrep.ripgrep sharkdp.fd Git.Git GitHub.cli
```

## Install Doom Emacs
1. Clone Doom:
   - macOS: `git clone https://github.com/doomemacs/doomemacs ~/.emacs.d`
   - Windows 11: `git clone https://github.com/doomemacs/doomemacs $env:USERPROFILE\.emacs.d`
2. Clone this config repo to the matching Doom directory (`~/.doom.d` on macOS, `%USERPROFILE%\.doom.d` on Windows).
3. Run Doom’s installer (`~/.emacs.d/bin/doom install` on macOS, `~\.emacs.d\bin\doom install` in PowerShell).
4. Whenever you change `packages.el`, run `doom sync`.

## Post-install steps
- Install the fonts used by the theme:
  - JetBrains Mono (main code font)  
    - macOS: `brew install --cask font-jetbrains-mono`
    - Windows 11: download from <https://www.jetbrains.com/lp/mono/> and double-click to install.
  - Noto Sans Symbols (Org heading bullets)  
    - macOS: `brew tap homebrew/cask-fonts && brew install --cask font-noto-sans-symbols-2`
    - Windows 11: download from <https://fonts.google.com/noto/specimen/Noto+Sans+Symbols> and install via Settings ▸ Personalization ▸ Fonts.
- Restart Emacs or run `M-x doom/reload-font` after installing fonts.

## Optional integrations
- **GitHub Copilot chat via gptel**: install GitHub CLI (`gh`), authenticate with `gh auth login --scopes \"copilot\"`, and ensure `gh` is on your `PATH`. Set `GITHUB_TOKEN` or rely on the CLI auth cache before launching Emacs.
- **Org directory**: the config expects `~/org/`. Create it or adjust the path in `config.org`.

## Post-install steps

After running `doom sync`, make sure the following are in place:

- Install the `Noto Sans Symbols` font so Org heading bullets render correctly.
  - **macOS:** `brew tap homebrew/cask-fonts && brew install --cask font-noto-sans-symbols-2`
  - **Windows 11:** download the OTF from <https://fonts.google.com/noto/specimen/Noto+Sans+Symbols> and install it via Settings ▸ Personalization ▸ Fonts.
- Restart Emacs (or run `M-x doom/reload-font`) after installing the font so Doom picks it up.
