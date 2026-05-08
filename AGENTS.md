## Agent Guidelines

- Apply all configuration changes in `config.org`; do not edit `config.el` unless expressly instructed.
- You can edit `init.el` as long as it is for editing DOOM Emacs settings (by enabling/disabling them). You should not add new things to this file.
- Before adding manual configuration in `config.org`, check whether an existing DOOM Emacs module, module flag, or combination of modules already provides the behavior. Prefer DOOM defaults when they satisfy the need; add custom config only when the module path is missing, insufficient, or intentionally being overridden.
- Any new function should be prefixed with `hhsaez/` (not `my/`).
- Always ensure the README.md file has all required steps to correctly setup DOOM Emacs in the current environment, specially any custom command and/or manual process needed beyond `doom sync`.
