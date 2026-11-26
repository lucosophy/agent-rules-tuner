# agent-rules-tuner

English | [简体中文](README.zh-CN.md)

A cross-agent global and project rules tuner skill (compatible with Claude Code, Codex, Antigravity, Cursor, Copilot, etc.) with built-in **Context Engineering** guardrails to persist coding preferences safely without instruction bloat or overfitting.

## Key Features

- **Dual-Mode Environment Adapter**: Automatically detects [Magpie](https://usemagpie.ai) (syncing via `magpie library instructions`) and falls back seamlessly to native rule files (`~/.codex/AGENTS.md`, `~/.claude/CLAUDE.md`, `~/.gemini/GEMINI.md`) using `<!-- agent-rules-tuner:begin/end -->` marker blocks.
- **Built-in Context Engineering Guardrails**: Enforces signal isolation (ephemeral requests vs. persistent preferences), the Rule of Three, a 30-line attention budget, and consolidate-before-append pruning.
- **Non-Blocking Async & In-Session Relay**: Dispatches background subagents (inheriting the lead model for accuracy) without blocking the user's primary workflow, staying silent on clean writes and waking the lead agent only on conflicts.
- **Zero Script Dependencies**: Pure Markdown following the open Agent Skills specification—works out of the box on Windows, macOS, and Linux.

## Installation

### Option 1: Via `skills` CLI (Recommended)
```sh
npx skills add lucosophy/agent-rules-tuner
```

### Option 2: Via Magpie
In the Magpie app under **Library › Skills**, add the GitHub repository `lucosophy/agent-rules-tuner` (or its full URL) to distribute across all configured agents and keep it updated with `magpie library skill update`.

### Option 3: Manual
Clone or copy this folder into your shared skills directory (e.g., `~/.agents/skills/agent-rules-tuner`) or your agent's `skills/` directory.

## Standards & References

This skill follows open community standards and Context Engineering best practices:

1. **Agent Skills Specification**: https://agentskills.io/
   - Defines `SKILL.md` frontmatter, `references/` layout, and **Progressive Disclosure**.
2. **AGENTS.md Open Standard**: https://agents.md/ ([agentsmd/agents.md](https://github.com/agentsmd/agents.md))
   - Standardizes cross-tool project instruction files.
3. **Context Engineering Best Practices**: Anthropic *Effective Context Engineering for AI Agents*
   - Addresses **Attention Budget**, **Context Rot**, and **Instruction Dilution**.

## Repository Structure

```text
agent-rules-tuner/
├── SKILL.md                             # Core workflow & guardrails (agentskills.io spec)
├── README.md                            # English documentation
├── README.zh-CN.md                      # Chinese documentation
├── LICENSE                              # MIT License
└── references/
    └── scope-and-formatting.md          # Scope rules, anti-bloat criteria & checklist
```

## Maintenance Checklist

Every 1–3 months, maintainers should:
1. Check [agentskills.io](https://agentskills.io/) and [agents.md](https://agents.md/) for spec updates or new default agent paths.
2. Update the built-in capability exclusion list in `references/scope-and-formatting.md` as newer models natively enforce more baseline behaviors.
3. Run `magpie library sync` after updating local files if managed by Magpie.
