# Claude Skills

Personal collection of Claude Code skills and plugins — installed in both Claude Code and Codex.

## Full Reference

See [SKILLS.md](./SKILLS.md) for a plain-English guide to every skill: what it does, when to use it, and how to invoke it.

## Installed Plugins

| Plugin | Source | Skills |
|--------|--------|--------|
| **understand-anything** | [Egonex-AI/Understand-Anything](https://github.com/Egonex-AI/Understand-Anything) | understand, understand-chat, understand-dashboard, understand-diff, understand-domain, understand-explain, understand-knowledge, understand-onboard |
| **roborev** | [kenn-io/roborev](https://github.com/kenn-io/roborev) | roborev-review, roborev-review-branch, roborev-design-review, roborev-design-review-branch, roborev-fix, roborev-refine, roborev-respond |
| **visual-explainer** | [nicobailon/visual-explainer](https://github.com/nicobailon/visual-explainer) | visual-explainer, generate-web-diagram, generate-slides, generate-visual-plan, diff-review, plan-review, project-recap, fact-check |
| **hyperframes** | hyperframes | 19 video creation skills |
| **ecc** | everything-claude-code | 100+ specialized subagents |
| **cybersecurity-skills** | cybersecurity-skills | 500+ cybersecurity skills |
| **skills** | ai-builder-club | dev-local-setup, e2e-setup, crabbox-setup, new-loop, pr, setup-codebase-harness |

## Manually Installed Skills

Individual skills copied from source repos into `~/.claude/skills/` and `~/.codex/skills/`.

| Skill | Source | Purpose |
|-------|--------|---------|
| `no-mistakes` | [kunchenguid/no-mistakes](https://github.com/kunchenguid/no-mistakes) | Gate pipeline: validate before push |
| `gnhf` | [kunchenguid/gnhf](https://github.com/kunchenguid/gnhf) | Orchestrate an agent with a stop condition |
| `axi` | [kunchenguid/axi](https://github.com/kunchenguid/axi) | Agent CLI ergonomics standard (TOON format) |
| `lavish` | [kunchenguid/lavish-axi](https://github.com/kunchenguid/lavish-axi) | Generate HTML visual artifacts |
| `lavish-design` | [kunchenguid/lavish-axi](https://github.com/kunchenguid/lavish-axi) | Lavish brand/design system reference |
| `afk` | [firstmate](https://github.com/kunchenguid/firstmate) | Away-mode fleet supervision |
| `updatefirstmate` | [firstmate](https://github.com/kunchenguid/firstmate) | Self-update firstmate fleet |
| `harness-adapters` | [firstmate](https://github.com/kunchenguid/firstmate) | Cross-harness spawn/recover reference |
| `secondmate-provisioning` | [firstmate](https://github.com/kunchenguid/firstmate) | Manage persistent secondmate homes |
| `stuck-crewmate-recovery` | [firstmate](https://github.com/kunchenguid/firstmate) | Recover stalled fleet agents |
| `fmx-respond` | [firstmate](https://github.com/kunchenguid/firstmate) | Handle X/Twitter mentions autonomously |

## Pi Agent

Open-source coding agent by earendil-works, installed via npm at `~/.hermes/node/bin/pi`.
Version: 0.80.2 — supports Claude, OpenAI, Google, and other LLM providers.

## Skills Source Files

Raw `SKILL.md` files for each manually installed skill are in the [`skills/`](./skills/) directory.
