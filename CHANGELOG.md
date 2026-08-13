# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- `answer-dont-act` rule: an explanation question produces an explanation only — no `Edit`, `Write`, or commit on that turn; a real problem found while answering is described in prose and waits for the go-ahead
- `answer-only-gate` hook (`UserPromptSubmit`): detects explanation-shaped prompts and injects the answer-only reminder for that turn. An imperative verb in command position cancels it, so "why is this slow? fix it" is unaffected while "why did you add the retry?" still trips — the verb's clause position is what separates the two
- `explain-pr` skill: explains a pull request in plain English for a reader who does not know the project — a fixed six-section template written in ASD-STE100 Simplified Technical English, plus a worked example; reads file statistics rather than the diff, and reports mismatches between the description and the files changed
- `grill-with-docs` rule: routes "grill me" and plan stress-testing to the mattpocock-skills plugin (`grilling` + `domain-modeling`), so no prefixed slash command is needed; preferred over `grill-me`
- `straight-talk` rule: enforces honest, peer-level communication with no flattery, filler praise, or false validation (always-on)
- `straight-talk` skill: portable version of the rule for sharing with teammates
- `tests-and-comments` rule: every test and comment must earn its place — no narration comments, no external references in source, tests target behavior boundaries and must state what regression they catch
- `codebase-memory` skill and settings hooks wiring the codebase-memory MCP server into sessions (PreToolUse gate on Grep/Glob, SessionStart and SubagentStart reminders via `~/.claude/hooks/cbm-*` scripts)
- `write-a-skill` skill: guides creation of new Claude Code skills with proper structure, descriptions, and review checklist

### Changed
- `straight-talk` rule and skill: added a "Plain language" section anchored on ASD-STE100 Simplified Technical English, matching `explain-pr` — the standard carries sentence length, active voice, and word choice, and its Technical Names and Technical Verbs rule keeps terms like `useEffect` legal, so the section only names the failures STE does not: abstraction in place of the real file or function, hazard-speak instead of what happens, metaphors for code, answers that continue past the answer, and repeating a message with more words when it did not land
- `straight-talk` rule and skill: put "Plain language" ahead of the honesty rules, since jargon is the more frequent failure and order in an always-on prompt is not neutral; the original bullets keep their wording under a "Honesty" heading, which the reorder made necessary — "Rules" read as a leftover bucket once a second rule list sat above it
- global `CLAUDE.md`: documented the hooks path mapping, which was missing — hook scripts are symlinked into `~/.claude/hooks/` per file, since that directory also holds vendor-installed scripts and cannot itself be a symlink
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
