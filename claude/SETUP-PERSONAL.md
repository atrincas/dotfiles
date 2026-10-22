# Set up Claude Code on a personal machine

Instructions for Claude Code. Open Claude Code in `~/dev/vizzuality/dotfiles` on the personal machine and say:
"follow claude/SETUP-PERSONAL.md".

The personal machine is for personal projects only. It uses `claude/settings.personal.json`: no Vizzuality plugins (vizz-core, atlassian, figma), no superpowers, no codebase-memory hooks and no status line. Rules, skills, sounds and hooks are the same files as on the work machine.

## Rules for this run

- Do the steps in order. If a step fails, stop and report the output. Do not work around it.
- Delete nothing. `install.sh` moves an existing file to `~/.dotfiles-backup/<timestamp>/` before it links over it.
- Steps that need the user (passwords, browser logins) go to the user as a `! <command>` to run.

## 1. Check the starting point

- `git -C ~/dev/vizzuality/dotfiles status` must show a clean tree on `main`. Then `git -C ~/dev/vizzuality/dotfiles pull --ff-only`.
- If `~/.claude` exists, list it with `ls -la ~/.claude` and show the user which of `CLAUDE.md`, `settings.json`, `rules`, `skills`, `sounds` and `hooks/*` are real files (not symlinks). `install.sh` will back those up.
- `~/.gitconfig.local` holds the git identity. If it exists, tell the user that `install.sh` keeps it.

## 2. Run the installer

- Run `~/dev/vizzuality/dotfiles/install.sh --personal`. The `--personal` flag is required. Without it, the work `settings.json` is linked.
- It installs Homebrew if missing (this asks for the user's password, so give the user `! ~/dev/vizzuality/dotfiles/install.sh --personal` to run instead if a prompt appears), the Brewfile packages and Claude Code, and links zsh, git, starship and the Claude config.
- Check the links: `ls -la ~/.claude ~/.claude/hooks`. `settings.json` must point to `claude/settings.personal.json`. Every file in `claude/hooks/` must have a link in `~/.claude/hooks/`.
- `python3 --version` must work, because `comment-length-gate` runs on it. On macOS, `xcode-select --install` provides it. Give that command to the user if it is missing.

## 3. Install the plugins

- Run `claude plugin --help` and read the subcommands on this version before you use them.
- Add the marketplace `anthropics/claude-plugins-official` if `claude plugin marketplace list` does not show `claude-plugins-official`.
- Install each plugin in `enabledPlugins` of `claude/settings.personal.json`, and only those.
- Do not install `superpowers`, `vizz-core`, `atlassian`, `figma` or `warp`.
- `claude plugin list` must match `enabledPlugins` exactly. Report any difference.

## 4. Check the hooks

Send sample input to each hook and compare the result:

| Hook | Input | Expected |
|---|---|---|
| `banned-words-gate` | `{"tool_name":"Bash","tool_input":{"command":"git commit -m \"fix footgun\""}}` | JSON with `"permissionDecision": "deny"` |
| `banned-words-gate` | `{"tool_name":"Bash","tool_input":{"command":"git commit -m \"fix(map): clamp zoom\""}}` | no output |
| `comment-length-gate` | a `Write` of `/tmp/a.ts` whose content has two consecutive `//` lines | JSON with `"permissionDecision": "deny"` |
| `comment-length-gate` | the same with one `//` line | no output |
| `answer-only-gate` | `{"prompt":"why is this slow?"}` | JSON with `additionalContext` |
| `answer-only-gate` | `{"prompt":"why is this slow? fix it"}` | no output |

Example: `printf '%s' '<input>' | ~/.claude/hooks/banned-words-gate`.

## 5. Smoke test

- In a new empty git repo (`mkdir -p ~/tmp/claude-smoke && git -C ~/tmp/claude-smoke init`), run
  `claude -p "List the rule files and the skills you have loaded, names only."`
- The rules from `claude/rules/` and the `mattpocock-skills:*` skills must be in the list. No `superpowers:*` or `vizz-core:*` skill may be in it.
- Give the user this check to run in an interactive session: ask Claude to run `gh pr comment 1 --body test`. It must ask for permission. Cancel it.

## 6. Report

A table with one row per step: result (passed, failed, skipped), and the reason for anything not passed. List the files `install.sh` moved to `~/.dotfiles-backup/`.
