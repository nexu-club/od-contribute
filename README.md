# od-contribute

A Claude Code / Codex / Cursor skill that walks any [Open Design](https://github.com/nexu-io/open-design) user — **including non-coders** — through a first-PR contribution flow.

## Install

```sh
npx skills add nexu-io/od-contribute
```

That's it. After the install runs, type one of these in your AI agent's chat:

| Agent | Trigger |
|---|---|
| Claude Code | `/od-contribute` |
| Codex CLI | `@od-contribute` or pick "Open Design — Contribute" from `/skills` |
| Cursor / others | "Run the od-contribute skill" — agent follows `~/.claude/skills/od-contribute/SKILL.md` |

## What it does

The skill asks you 2–4 questions and then opens a real PR (or issue) on `nexu-io/open-design` on your behalf — fork, clone, branch, commit, push, PR body, all of it. You never touch a terminal, never read a git command, never write a `gh` flag.

Four contribution paths:

| | Path | What you ship | Where it lands |
|---|---|---|---|
| 🎨 | **Ship something you made with OD** | a Skill, Design System, HyperFrame, or template | `skills/<name>/` or `design-systems/<brand>/DESIGN.md` |
| 🌍 | **Translate OD docs** | README / QUICKSTART / CONTRIBUTING / MAINTAINERS into your language | `<DOC>.<lang>.md` |
| 📝 | **Fix docs, typos, dead links, or write a blog** | one-line edits or short use-case writeups | `docs/`, `README*.md`, `QUICKSTART*.md` |
| 🐛 | **Report a bug** | a structured issue (no PR) using OD's `bug-report.yml` schema | a new issue with the `bug` label |

For each path, the skill replaces a traditional TDD dev-loop with type-specific **no-code validators** — frontmatter parse, markdown link check, code-fence balance, structural overlap with reference `DESIGN.md` files. Content-only contributions don't need lint / typecheck / tests, so the validators don't pretend to.

## Why it exists

OD is built by and for designers as much as developers. The single biggest friction in turning an OD user into an OD contributor isn't talent — it's the eight-step `git fork → clone → branch → commit → push → PR` ritual. This skill collapses that into "answer a couple of questions, click Ship it."

## Safety rails (built in)

- Never pushes to `main` / `master` / `develop`
- Never `--force` pushes
- Workspace confined to `$OD_WORK_ROOT` (default `~/od-contrib-work/`)
- Bug-report path runs `gh search issues` and **refuses to create** if duplicates exist (caller can override with `--allow-duplicates`)
- Validators fail-closed: a Skill submission with broken frontmatter or escape paths can't be PR'd
- Final confirmation gate (Step 7) requires explicit "Ship it" before any push

## What gets installed where

`npx skills add` drops the skill into the standard per-agent locations:

| Path | Used by |
|---|---|
| `~/.claude/skills/od-contribute/` | Claude Code (native skill format) |
| `~/.claude/commands/od-contribute.md` | Claude Code slash command `/od-contribute` |
| `~/.agents/skills/od-contribute/` | Codex CLI (canonical) |
| `~/.codex/skills/od-contribute/` | Codex CLI (legacy, populated only if you have `~/.codex/`) |

Re-running `npx skills add` upgrades to the latest version while preserving local state (e.g. `.gh-token` for sandboxed agents that can't reach the macOS keychain).

## Language

Skill content is English. At runtime the skill **mirrors your chat language** in user-facing prompts — translate it into your native language by simply chatting in that language. Generated artifacts (PR titles, commit messages, PR/issue bodies, branch names) stay English regardless, per GitHub convention.

## Cross-runtime support

| Agent | Native support |
|---|---|
| Claude Code | ✅ frontmatter + scripts/ + slash command |
| Codex CLI | ✅ same SKILL.md format ([verified](https://github.com/openai/codex/blob/main/codex-rs/core-skills/src/loader.rs)); `agents/openai.yaml` sidecar adds picker UX |
| Cursor / OpenCode / Gemini / others | files at `~/.claude/skills/od-contribute/` — point your agent at SKILL.md |

## Requirements

- `gh` (GitHub CLI) — authenticated (`gh auth login`)
- `git`
- `jq`
- An AI agent (Claude Code, Codex CLI, Cursor, etc.) with Bash tool

For sandboxed agents (Codex.app, Cursor) that can't reach the macOS keychain where `gh auth login` stores tokens, drop a token at `~/.claude/skills/od-contribute/.gh-token` (mode 600) from a regular terminal:

```sh
gh auth token > ~/.claude/skills/od-contribute/.gh-token
chmod 600 ~/.claude/skills/od-contribute/.gh-token
```

The skill picks it up automatically next run. A follow-up release will add GitHub OAuth Device Flow so this manual step goes away entirely.

## Discord

Join the OD community: <https://discord.gg/qhbcCH8Am4>

## License

Apache-2.0 — same as Open Design itself.
