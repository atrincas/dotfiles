# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- `explain-pr` skill: explains a pull request in plain English for a reader who does not know the project — a fixed six-section template written in ASD-STE100 Simplified Technical English, plus a worked example; reads file statistics rather than the diff, and reports mismatches between the description and the files changed
- `grill-with-docs` rule: routes "grill me" and plan stress-testing to the mattpocock-skills plugin (`grilling` + `domain-modeling`), so no prefixed slash command is needed; preferred over `grill-me`
- `straight-talk` rule: enforces honest, peer-level communication with no flattery, filler praise, or false validation (always-on)
- `straight-talk` skill: portable version of the rule for sharing with teammates
- `tests-and-comments` rule: every test and comment must earn its place — no narration comments, no external references in source, tests target behavior boundaries and must state what regression they catch
- `codebase-memory` skill and settings hooks wiring the codebase-memory MCP server into sessions (PreToolUse gate on Grep/Glob, SessionStart and SubagentStart reminders via `~/.claude/hooks/cbm-*` scripts)
- `write-a-skill` skill: guides creation of new Claude Code skills with proper structure, descriptions, and review checklist

### Changed
- settings: enabled `vizz-core` (Vizzuality marketplace), `warp`, and `mattpocock-skills` plugins, registering the vizzuality and claude-code-warp marketplaces
- settings: disabled the Bash sandbox
- settings: moved `defaultMode` into the `permissions` block per current settings schema
- settings: enabled fullscreen TUI and hold-to-talk voice input
- settings: default model changed from `claude-opus-4-6` to `opus[1m]` (Opus 5, 1M context)
- global gitignore: expanded AI-tool entries (CLAUDE.md, CONTEXT.md, AGENTS.md, `.claude/`, `.agents/`, `.mcp.json`, `.playwright-mcp/`, copilot instructions, skills-lock.json)
- `grill-me` skill: shortened closed-questions rule for brevity
- `grill-with-docs` rule: added a presentation override so `grilling` asks one closed, labeled question per message instead of batching the whole frontier — the plugin's design-tree traversal with the `grill-me` prompting style
- `grill-with-docs` rule: gave the presentation override its missing rationale and precedence — why the picker is banned (truncated option bodies), why questions are serialised (answers reframe the frontier), that `grilling`'s `❓`/`➡️` format survives with cumulative numbering, that recommendations must state what would change them, and that markdown wins over `AskUserQuestion` in plan mode
- `grill-with-docs` rule: added a domain-model section with concrete triggers and paths (`docs/domain/glossary.md`, `docs/domain/adr/NNN-<slug>.md`), written as each decision settles — previously "maintain the domain model as decisions crystallise" had no trigger, threshold, or destination, so nothing was ever written

### Fixed
- global `CLAUDE.md`: the configuration block pointed at `~/Developer/dotfiles`, which does not exist — every path now names the real source, `~/dev/vizzuality/dotfiles`
