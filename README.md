Custom DOOM Emacs config

## Requirements
- Emacs 29.1+ (native Windows build or GNU Emacs on macOS)
- Git, `ripgrep`, and `fd` available in `$PATH`
- Python 3 with `pip`/`pip3` for the CMake language server (Python 3.13 is required on Linux; see the post-install steps)
- Optional but recommended: GitHub CLI (`gh`) for the Copilot GPT backend
- `clang` or a package that provides `clangd`

macOS (Homebrew):
```bash
brew install emacs ripgrep fd git gh
```

Windows 11 (winget):
```powershell
winget install GNU.Emacs ripgrep.ripgrep sharkdp.fd Git.Git GitHub.cli
```

Linux/Omarchy:

``` bash
sudo pacman -S emacs git ripgrep fd python python-pip clang
```

## Install Doom Emacs
1. Clone Doom:
   - macOS: `git clone https://github.com/doomemacs/doomemacs ~/.emacs.d`
   - Windows 11: `git clone https://github.com/doomemacs/doomemacs $env:USERPROFILE\.emacs.d`
2. Clone this config repo to the matching Doom directory (`~/.doom.d` on macOS, `%USERPROFILE%\.doom.d` on Windows).
3. Run Doom’s installer (`~/.emacs.d/bin/doom install` on macOS, `~\.emacs.d\bin\doom install` in PowerShell).
4. Whenever you change `init.el`, `packages.el`, or `config.org`, run `doom sync`.
5. Restart Emacs after `doom sync` so the updated config loads.

## Post-install steps
- Install the fonts used by the theme:
  - JetBrains Mono (main code font)
    - macOS: `brew install --cask font-jetbrains-mono`
    - Windows 11: download from <https://www.jetbrains.com/lp/mono/> and double-click to install.
  - Noto Sans Symbols (Org heading bullets)
    - macOS: `brew tap homebrew/cask-fonts && brew install --cask font-noto-sans-symbols-2`
    - Windows 11: download from <https://fonts.google.com/noto/specimen/Noto+Sans+Symbols> and install via Settings ▸ Personalization ▸ Fonts.
- Restart Emacs (or run `M-x doom/reload-font`) after installing fonts so Doom picks them up.
- CMake language server (Linux/Omarchy): Python 3.14 is currently incompatible with `cmake-language-server` because its `pygls` 1.x dependency calls a removed `asyncio` API. Install Python 3.13 from the AUR, then isolate the server in a virtual environment and pin its compatible dependencies:

  ```bash
  yay -S python313
  python3.13 -m venv ~/.local/venvs/cmake-lsp
  ~/.local/venvs/cmake-lsp/bin/pip install --upgrade pip
  ~/.local/venvs/cmake-lsp/bin/pip install --force-reinstall \
    'cmake-language-server==0.1.11' \
    'pygls==1.3.1'
  mv ~/.local/bin/cmake-language-server ~/.local/bin/cmake-language-server.py314
  ln -s ~/.local/venvs/cmake-lsp/bin/cmake-language-server ~/.local/bin/cmake-language-server
  cmake-language-server --version
  ```

  The `pygls` pin is required: `cmake-language-server` 0.1.11 imports the pre-2.0 `pygls` API. Restart Emacs after the command succeeds. If you use `paru` rather than `yay`, substitute `paru -S python313`.
- Doom may prompt to install missing tree-sitter grammars the first time you open a tree-sitter-enabled language buffer after `doom sync`. Accept the prompt when you want that language to use tree-sitter-backed modes.

## Optional integrations
- **GitHub Copilot chat via gptel**: install GitHub CLI (`gh`), authenticate with `gh auth login --scopes "copilot"`, and ensure `gh` is on your `PATH`. Set `GITHUB_TOKEN` or rely on the CLI auth cache before launching Emacs.
- **Org directory**: the config expects `~/org/`. Create it or adjust the path in `config.org`.
- **Codex CLI**: install the `codex` binary and run `M-x hhsaez/codex-run` from any Projectile project to open an interactive Codex session in a proper terminal buffer (falls back to Emacs `term`, so it works even without vterm).
- **Terminal Emacs on macOS**: configure your terminal to send Option/Alt as Meta (Terminal.app: Settings ▸ Profiles ▸ Keyboard ▸ “Use Option as Meta key”; iTerm2: Profiles ▸ Keys ▸ Left/Right Option Key = Esc+). Super is not available in `-nw` sessions, and this config uses Option/Alt as Meta in GUI sessions too.
- **Hyprland (Omarchy)**: if you use Omarchy’s macOS-like keybindings, make sure `Ctrl+F` is excluded from remapping so Emacs still sees `C-f`. After editing Hyprland bindings, run `hyprctl reload` or restart your session.

## Troubleshooting

### clangd is crashing with segmentation fault
Symptom: The crash occurs during preamble build and AST serialization involving concept/template-heavy code.
Likely fix: upgrade =clangd= to a newer release, ideally =17+=

## C++ formatting

Native Emacs C++ indentation in `config.org` is tuned to approximate the project's `.clang-format`, mainly for indentation-sensitive constructs such as access modifiers, namespace contents, argument lists, inheritance lists, and constructor initializer lists.

Exact `.clang-format` parity is not expected from native indentation alone. Spacing rules, brace placement, line wrapping, include sorting, and comment formatting still require `clang-format` or `clangd` formatting support.
