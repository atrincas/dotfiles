# Dotfiles

Personal dotfiles for macOS (Apple Silicon).

## What's included

| Directory | Contents |
|-----------|----------|
| `zsh/` | Zsh config with zinit, fzf (with `fd`), zoxide |
| `git/` | Git config with delta pager, trunk-based defaults |
| `starship/` | Custom two-line prompt (directory, git, nodejs, docker, duration) |
| `claude/` | Claude Code global settings, rules, and skills |
| `homebrew/` | Brewfile with formulas, casks, and fonts |

## Install

```sh
mkdir -p ~/Developer
git clone git@github.com:agnlez/dotfiles.git ~/Developer/dotfiles
cd ~/Developer/dotfiles
./install.sh
```

The install script will:
1. Install Homebrew (if missing)
2. Install all packages from the Brewfile
3. Install Claude Code via the native installer (self-contained, auto-updating)
4. Symlink config files to their expected locations
5. Back up any existing files to `~/.dotfiles-backup/`

### Personal machine

`./install.sh --personal` links `claude/settings.personal.json` as the Claude Code settings instead of `claude/settings.json`. It has no Vizzuality plugins, no superpowers and no codebase-memory hooks. To let Claude Code do the full setup, including plugins and hook checks, open it in the repo and say "follow claude/SETUP-PERSONAL.md".

## Manual steps after install

### Git identity

`install.sh` creates `~/.gitconfig.local` from `git/.gitconfig.local.example` on first run. Edit it with your name and email. It's loaded via `[include]` and **overrides** the tracked gitconfig, so machine-specific values belong here too.

### Apps

Sign into the rest (Slack, etc.)
