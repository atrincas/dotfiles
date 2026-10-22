## Claude Code configuration

All Claude Code global configuration is managed through `~/dev/vizzuality/dotfiles` and symlinked to `~/.claude/`. When creating or modifying rules, skills, hooks, or settings, edit the source files in the dotfiles repo so changes are tracked in git:

- `~/dev/vizzuality/dotfiles/claude/settings.json` → `~/.claude/settings.json` on the work machine; `settings.personal.json` → `~/.claude/settings.json` on the personal machine (`install.sh --personal`). A change to permissions or hooks usually belongs in both files
- `~/dev/vizzuality/dotfiles/claude/CLAUDE.md` → `~/.claude/CLAUDE.md`
- `~/dev/vizzuality/dotfiles/claude/rules/` → `~/.claude/rules/`
- `~/dev/vizzuality/dotfiles/claude/skills/` → `~/.claude/skills/`
- `~/dev/vizzuality/dotfiles/claude/sounds/` → `~/.claude/sounds/`
- `~/dev/vizzuality/dotfiles/claude/hooks/<script>` → `~/.claude/hooks/<script>` (per file — `~/.claude/hooks/` also holds vendor-installed scripts and is not itself a symlink)

## Memory

- Record a trap — a command or step that looks like it worked but did not — as a `feedback` memory whose description starts with `TRAP:`

## Tool preferences

- Assume `rg`, `fd`, `bat`, `eza`, `jq`, `yq`, and `zoxide` are available in the environment
- Use `rg --type` flags to scope searches by language when appropriate
- Use `fd -e` to filter by extension or `fd -t` to filter by type when appropriate
- Prefer `rg` (ripgrep) over `grep` for all file and pattern searches
- Prefer `fd` over `find` for all filesystem searches
- Prefer `bat` over `cat` for reading and displaying file contents
- Prefer `eza` over `ls` for directory listings; use `eza --tree` instead of `tree`
- Prefer `jq` for JSON querying, filtering, and transformation from the command line
- Prefer `yq` for YAML, TOML, and XML querying and in-place edits; use `yq -o json` to convert to JSON when needed
