# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
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
