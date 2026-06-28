# Skill Explainer
> A plain-English guide to every skill installed in Claude Code and Codex.
> Last updated: 2026-06-28 (added understand-anything)

---

## How to read this doc

Each skill has:
- **What it does** — one sentence
- **Use it when** — concrete triggers / situations
- **How to invoke** — the slash command or trigger phrase

Skills marked `[agent-only]` are not user-invocable — they are loaded automatically by other skills or agent orchestrators.

---

## Table of Contents

1. [Code Quality & Shipping (kunchenguid)](#1-code-quality--shipping-kunchenguid)
2. [Fleet Management (Firstmate)](#2-fleet-management-firstmate)
3. [Code Review (roborev)](#3-code-review-roborev)
4. [Visual Explanations (visual-explainer)](#4-visual-explanations-visual-explainer)
5. [Video Creation (hyperframes)](#5-video-creation-hyperframes)
6. [Codebase Harness (skills plugin)](#6-codebase-harness-skills-plugin)
7. [Everything Claude Code (ecc)](#7-everything-claude-code-ecc)
8. [Cybersecurity Skills](#8-cybersecurity-skills)
9. [Understand Anything](#9-understand-anything)
10. [Pi Agent](#10-pi-agent)
11. [Quick Reference](#quick-reference-what-skill-should-i-use-for-x)

---

## 1. Code Quality & Shipping (kunchenguid)

These five skills form a suite by the same author. They work together to build, validate, review, and visualise your code before it ships.

---

### `no-mistakes`
**What it does:** Runs your code changes through a full validation pipeline — intent check, rebase, AI code review, tests, lint, docs, push, PR creation, and CI — before anything reaches your remote branch.

**Use it when:**
- You've made commits and want to push safely without breaking main
- You want to run a task AND validate it in one go (e.g. "add a --json flag and then validate")
- You want to skip specific steps (e.g. "skip the lint step")

**How to invoke:** `/no-mistakes` or `/no-mistakes <task description>`

**Two modes:**
- Validate-only: `/no-mistakes` — changes already committed, just run the gate
- Task-first: `/no-mistakes add dark mode support` — Claude does the task, then runs the gate

---

### `gnhf`
**What it does:** GNHF (Go Now, Handle First) is an agent orchestrator — it launches another coding agent in a loop and supervises it until a stop condition you define is met.

**Use it when:**
- You're going to sleep or walking away and want work to keep running unattended
- You have a bounded task with a clear "done" condition that another agent should execute
- You want to steer or review an active unattended run (Companion mode)

**How to invoke:** `/gnhf`, "I'm going afk", "supervise this run", "go now handle X"

**Two modes:**
- Hands-Off: Define task + stop condition, launch, come back to results
- Companion: Stay in the loop to steer or review mid-run

---

### `axi`
**What it does:** Documents the Agent eXperience Interface (AXI) standard — ergonomic conventions for building CLI tools that AI agents use via shell execution, including TOON output format, exit codes, and flag design.

**Use it when:**
- You're building a CLI that agents will call (not just humans)
- You're reviewing or modifying an agent-facing tool for token efficiency
- You want to understand how `no-mistakes axi` output works

**How to invoke:** Reference skill — Claude loads it when building agent-facing CLIs. Also invocable as `/axi`.

**Key idea:** Agents receive CLI output as tokens, so every field costs money. AXI defines how to keep that output minimal and structured using TOON format (~40% token savings over JSON).

---

### `lavish`
**What it does:** Turns complex agent responses into rich, interactive HTML pages that you can annotate, give feedback on, and share — using the `lavish-axi` CLI tool.

**Use it when:**
- Claude is about to give you a plan, comparison table, diagram, or data-heavy response that would be clearer as a visual page
- You want an interactive review surface where you can annotate specific elements
- You need to send a visual report or prototype to someone else

**How to invoke:** `/lavish <what to show>` or just describe what you want visualised

**Behind the scenes:** Claude generates an HTML artifact, runs `npx -y lavish-axi <file>` to open it in your browser, then polls for your annotations via `npx -y lavish-axi poll`.

---

### `lavish-design` `[agent-only]`
**What it does:** Provides the full Lavish brand design system — colors, typography, CSS tokens, icon guidelines, and UI kit components — so Claude produces on-brand HTML artifacts.

**Use it when:** Claude needs to generate a Lavish-styled visual artifact or production UI. Loaded automatically by `lavish`.

**Key design rules (for reference):**
- Dark ink + cream type + one brass accent only
- Serif (EB Garamond italic) for prose, Sans (Geist) for chrome, Mono for code
- No emoji, no exclamation marks, no "powerful seamless" marketing copy
- Sage = agent actions, Amber = user actions, Rust = danger

---

## 2. Fleet Management (Firstmate)

Firstmate is an autonomous agent fleet system — a "captain" (you) manages multiple AI "crewmates" working in parallel. These skills are the operating procedures for that fleet.

---

### `afk`
**What it does:** Enters away-mode so the fleet's sub-supervisor daemon can handle routine wake-ups in bash instead of burning LLM tokens, batching any important escalations into one digest when you return.

**Use it when:**
- You're stepping away and have agents running in the background
- You want to cut supervision token costs during walk-away stretches

**How to invoke:** `/afk`, "/afk back in an hour", "going afk"

**What happens:** Sets a `state/.afk` flag file. The daemon picks it up and switches to low-cost bash triage. Any captain-relevant event is held until you return as a single batched digest.

---

### `updatefirstmate`
**What it does:** Self-updates the running firstmate and all secondmates to the latest from origin — fast-forward only, never disruptive to in-flight work.

**Use it when:**
- You want to pull the latest firstmate instructions, bin scripts, and skills
- There's been an upstream update to firstmate and your fleet needs it

**How to invoke:** `/updatefirstmate`, "update firstmate", "pull the latest firstmate"

**Safe by design:** Only fast-forwards clean branches. Skips anything dirty, diverged, or offline. Never touches your project files or in-progress work.

---

### `harness-adapters` `[agent-only]`
**What it does:** Reference playbook for firstmate to correctly spawn, interrupt, exit, resume, and interact with crewmates across different agent harnesses (Claude Code, Codex, OpenCode, Pi).

**Use it when:** Loaded automatically before spawning or recovering a crewmate. Not user-invocable.

**Contains:** Verified facts for each supported harness — how to detect it, send keystrokes, handle trust dialogs, invoke skills, and resume a stopped agent.

---

### `secondmate-provisioning` `[agent-only]`
**What it does:** Reference for setting up a persistent secondmate — a specialised sub-supervisor that owns a domain of work (e.g. "all Python backend tasks").

**Use it when:** Loaded automatically when creating, seeding, or retiring a secondmate home. Not user-invocable.

**Key concepts:** Each secondmate has a charter, a home directory (a treehouse worktree), and a natural-language scope in `data/secondmates.md`. Routing is by scope, not by project.

---

### `stuck-crewmate-recovery` `[agent-only]`
**What it does:** Escalating playbook for when a crewmate is stuck — from peeking the pane, to steering, to interrupting, to relaunching with progress context.

**Use it when:** Loaded automatically by firstmate when a direct report goes stale, loops, or stops responding. Not user-invocable.

**Escalation order:** Peek → one-line steer → interrupt → relaunch with progress → mark failed and report to captain.

---

### `fmx-respond` `[agent-only]`
**What it does:** Handles X (Twitter) mentions from your own account when firstmate is running in X mode — classifies them as requests, questions, or acknowledgements, and replies autonomously.

**Use it when:** Loaded automatically when an `x-mention` wake arrives and X mode is configured (`FMX_PAIRING_TOKEN` in `.env`). Not user-invocable.

**Key rule:** Only your own owner's mentions reach this skill (owner-only routing). Replies are posted autonomously — no asking for approval unless the action is destructive.

---

## 3. Code Review (roborev)

Roborev is an automated code and design review daemon. It runs reviews asynchronously in the background, returns structured findings, and can fix them.

---

### `roborev-review`
**What it does:** Requests an AI code review for a single commit and presents the results.

**Use it when:**
- You've made a commit and want a structured code review before pushing
- You want to know about correctness, security, and style issues in one specific change

**How to invoke:** `/roborev-review [--commit <sha>] [--panel <name>|none]`

---

### `roborev-review-branch`
**What it does:** Requests a code review across all commits on the current branch (from where it diverged from base).

**Use it when:**
- Your branch has multiple commits and you want the whole thing reviewed as a unit
- You're about to open a PR and want a last pass

**How to invoke:** `/roborev-review-branch [--base <branch>] [--panel <name>|none]`

---

### `roborev-design-review`
**What it does:** Requests a design/architecture review for a single commit — focuses on structure, patterns, and design decisions rather than correctness bugs.

**Use it when:**
- You want feedback on whether your approach is well-structured
- You've made a change that involves new abstractions or architectural decisions

**How to invoke:** `/roborev-design-review [--commit <sha>]`

---

### `roborev-design-review-branch`
**What it does:** Design review across all commits on the current branch.

**Use it when:** Same as above but for an entire feature branch before merging.

**How to invoke:** `/roborev-design-review-branch [--base <branch>]`

---

### `roborev-fix`
**What it does:** Discovers open failing roborev reviews and fixes them.

**Use it when:**
- Reviews came back with findings and you want Claude to fix them automatically
- You have specific job IDs from previous reviews to address

**How to invoke:** `/roborev-fix` or provide job IDs directly

---

### `roborev-refine`
**What it does:** Iterative review-fix loop — reviews the branch, fixes inline, re-reviews, repeats until passing or max iterations reached.

**Use it when:**
- You want fully automated review → fix → re-review cycles without manual intervention
- You're confident the codebase is close to passing and just needs polish

**How to invoke:** `/roborev-refine`

---

### `roborev-respond`
**What it does:** Adds a comment to an existing roborev review and closes it.

**Use it when:**
- You want to acknowledge or respond to a review finding without fixing it
- You want to mark a finding as won't-fix with an explanation

**How to invoke:** `/roborev-respond <job-id> <your response>`

---

## 4. Visual Explanations (visual-explainer)

Generates self-contained HTML pages for diagrams, plans, diffs, slides, and data — no external dependencies, works offline, opens in your browser.

---

### `visual-explainer:visual-explainer`
**What it does:** The main skill — generates any kind of HTML visual explanation: architecture diagrams, comparison tables, system overviews, data visualisations.

**Use it when:**
- You want to understand a complex system visually
- You need a shareable diagram or overview
- A table or chart would explain something better than prose

**How to invoke:** `/visual-explainer <what to visualise>`

---

### `visual-explainer:generate-web-diagram`
**What it does:** Generates a standalone HTML diagram (using Mermaid, D3, or similar) and opens it in your browser.

**Use it when:** You need a specific diagram — flowchart, sequence diagram, ER diagram, network topology.

**How to invoke:** `/generate-web-diagram <what to diagram>`

---

### `visual-explainer:generate-slides`
**What it does:** Creates a self-contained HTML slide deck you can present directly in your browser.

**Use it when:** You need to present technical content, share a walkthrough, or create a quick deck without PowerPoint.

**How to invoke:** `/generate-slides <topic>`

---

### `visual-explainer:generate-visual-plan`
**What it does:** Turns an implementation plan into a visual HTML page showing steps, dependencies, and decision points.

**Use it when:** Claude has written a plan and you want to see it laid out visually before approving it.

**How to invoke:** `/generate-visual-plan`

---

### `visual-explainer:diff-review`
**What it does:** Generates a visual HTML diff review — shows what changed, why, and flags potential issues in a readable layout.

**Use it when:** You want a more readable version of `git diff` before reviewing a PR.

**How to invoke:** `/diff-review`

---

### `visual-explainer:plan-review`
**What it does:** Compares an implementation plan against the current codebase to check for gaps, conflicts, or outdated assumptions.

**Use it when:** You have a plan written earlier and want to verify it still matches reality before implementing.

**How to invoke:** `/plan-review`

---

### `visual-explainer:project-recap`
**What it does:** Generates a visual project recap — what was built, what changed, what's next — useful for context switching or handoffs.

**Use it when:** You're returning to a project after time away, or onboarding someone new.

**How to invoke:** `/project-recap`

---

### `visual-explainer:fact-check`
**What it does:** Verifies a generated document against the actual code and git history — catches hallucinations or stale content.

**Use it when:** Claude generated a technical doc or explanation and you want to verify it's accurate.

**How to invoke:** `/fact-check`

---

## 5. Video Creation (hyperframes)

Hyperframes creates videos using AI — from code changes, websites, slides, and media — powered by Remotion and compositing tools.

---

### `hyperframes:hyperframes` (main)
**What it does:** Core skill — the entry point for video creation. Routes to the appropriate sub-skill.

**Use it when:** You want to create any kind of video from code, content, or media.

**How to invoke:** `/hyperframes <what to create a video about>`

---

### `hyperframes:general-video`
**What it does:** Creates a general-purpose video for any topic or content.

**Use it when:** You need a video that doesn't fit a specific template.

---

### `hyperframes:product-launch-video`
**What it does:** Creates a polished product launch video with demo footage, captions, and branding.

**Use it when:** Launching a feature, product, or update and need a shareable video.

---

### `hyperframes:pr-to-video`
**What it does:** Turns a pull request into an explainer video showing what changed and why.

**Use it when:** You want a video walkthrough of a PR for async review or demos.

---

### `hyperframes:website-to-video`
**What it does:** Records and edits a website into a video walkthrough.

**Use it when:** Demo-ing a web app or creating a product tour.

---

### `hyperframes:slideshow`
**What it does:** Converts slides or a presentation into a video.

**Use it when:** Turning a talk or deck into shareable video content.

---

### `hyperframes:faceless-explainer`
**What it does:** Creates a faceless explainer video (no camera needed) with narration and visuals.

**Use it when:** You want to explain a concept or feature without recording yourself.

---

### `hyperframes:talking-head-recut`
**What it does:** Re-edits a talking-head recording — removes filler, tightens pacing.

**Use it when:** You have a raw screen recording or talking-head video that needs cleanup.

---

### `hyperframes:embedded-captions`
**What it does:** Adds burned-in captions to a video.

**Use it when:** Your video needs captions for accessibility or social media.

---

### `hyperframes:music-to-video`
**What it does:** Creates a video from a music track — visualiser or montage style.

**Use it when:** You need a video to accompany audio content.

---

### `hyperframes:motion-graphics`
**What it does:** Creates motion graphics — animated titles, lower thirds, transitions.

**Use it when:** You need animated visual elements for a video.

---

### `hyperframes:hyperframes-animation`
**What it does:** Creates code-driven animations using Remotion.

**Use it when:** You want a custom animated sequence built programmatically.

---

## 6. Codebase Harness (skills plugin)

These skills, from the `ai-builder-club` plugin, set up the infrastructure needed for reliable agent-driven development on any repo.

---

### `skills:setup-codebase-harness`
**What it does:** Master setup skill — makes any repo fully agent-ready: maps the codebase, sets up a one-command dev stack, adds an e2e test gate, and enforces commit hygiene.

**Use it when:**
- You're onboarding a new or unfamiliar codebase to agent-driven development
- You want reliable, repeatable agent workflows on a project

**How to invoke:** "set up the harness", "make this repo agent-ready", `/setup-codebase-harness`

---

### `skills:dev-local-setup`
**What it does:** Generates a single `scripts/dev-local.sh` script that starts every dev server (database, backend, frontend) in one tmux session with up/down/status/logs/restart commands.

**Use it when:**
- Your project has multiple services that need to run together
- You want a one-command "start everything" script for local development

**How to invoke:** "set up dev-local", "make a one-command dev launcher", `/dev-local-setup`

---

### `skills:e2e-setup`
**What it does:** Sets up a Playwright end-to-end test suite covering real user flows, with auth helpers, video+trace evidence, and a compounding test suite.

**Use it when:**
- Your repo has no e2e tests or very weak ones
- You want system-level tests as a PR gate

**How to invoke:** "set up e2e", "add end-to-end tests", `/e2e-setup`

---

### `skills:crabbox-setup`
**What it does:** Scaffolds an isolated cloud dev box per agent (via Crabbox + Daytona) so parallel agents each get their own full stack — own database, own dev server, own browser — without port conflicts.

**Use it when:**
- You're running multiple agents in parallel and they conflict on ports or shared state
- You need true per-agent isolation for testing
- You want cloud-based testing on Daytona

**How to invoke:** "set up crabbox", "give each agent its own box", "add parallel-safe testing"

---

### `skills:new-loop`
**What it does:** Spins up a new "loop" (recurring domain/workstream) in a file-based knowledge base — creates the charter, scaffolds `domains/<loop>/README.md`, runs a test iteration, and logs it.

**Use it when:**
- You have a recurring job you want an agent to own long-term (e.g. "monitor PRs weekly")
- You're setting up a new knowledge base domain

**How to invoke:** "set up a new loop", "create a domain", "start a new workstream"

---

### `skills:pr`
**What it does:** Verifies that the feature you just built actually works (runs the real app via a fresh sub-agent), then opens a GitHub pull request with proof of working functionality.

**Use it when:**
- You've finished a feature and want to ship it
- You want a PR that includes evidence the feature works, not just the code

**How to invoke:** `/pr`, "open a PR", "ship this", "raise a PR"

---

## 7. Everything Claude Code (ecc)

ECC is the largest plugin — it provides specialised sub-agents for every language, framework, and workflow.

---

### Code Review Sub-agents

| Skill | Use it when |
|-------|-------------|
| `ecc:code-reviewer` | General code review of current diff |
| `ecc:react-reviewer` | Reviewing React/JSX/TSX changes |
| `ecc:python-reviewer` | Reviewing Python code |
| `ecc:typescript-reviewer` | TypeScript/JS changes |
| `ecc:go-reviewer` | Go code changes |
| `ecc:rust-reviewer` | Rust code changes |
| `ecc:security-reviewer` | Scanning for security vulnerabilities |
| `ecc:fastapi-reviewer` | FastAPI application review |
| `ecc:django-reviewer` | Django application review |
| `ecc:kotlin-reviewer` | Kotlin/Android review |
| `ecc:swift-reviewer` | Swift/iOS review |
| `ecc:flutter-reviewer` | Flutter/Dart review |
| `ecc:vue-reviewer` | Vue.js review |
| `ecc:cpp-reviewer` | C++ review |
| `ecc:csharp-reviewer` | C# / .NET review |
| `ecc:java-reviewer` | Java / Spring Boot / Quarkus review |
| `ecc:php-reviewer` | PHP review |
| `ecc:rust-reviewer` | Rust review |
| `ecc:database-reviewer` | PostgreSQL / SQL / Supabase review |
| `ecc:mle-reviewer` | ML engineering / MLOps review |

---

### Build Fix Sub-agents

| Skill | Use it when |
|-------|-------------|
| `ecc:build-error-resolver` | Any build failure |
| `ecc:react-build-resolver` | React/Next.js/Vite build failures |
| `ecc:go-build-resolver` | Go build errors |
| `ecc:rust-build-resolver` | Rust/Cargo errors |
| `ecc:kotlin-build-resolver` | Kotlin/Gradle failures |
| `ecc:dart-build-resolver` | Flutter/Dart build failures |
| `ecc:java-build-resolver` | Java/Maven/Gradle failures |
| `ecc:swift-build-resolver` | Swift/Xcode failures |
| `ecc:cpp-build-resolver` | C++/CMake failures |
| `ecc:django-build-resolver` | Django startup/migration failures |
| `ecc:pytorch-build-resolver` | PyTorch/CUDA training crashes |

---

### Planning & Architecture

| Skill | Use it when |
|-------|-------------|
| `ecc:planner` | Plan a feature or refactor before coding |
| `ecc:architect` | System design and technical decisions |
| `ecc:code-architect` | Design feature architecture from codebase patterns |
| `ecc:code-explorer` | Deep-dive into existing codebase features |

---

### Testing

| Skill | Use it when |
|-------|-------------|
| `ecc:tdd-guide` | Write tests first (TDD) |
| `ecc:e2e-runner` | Generate, run, and maintain Playwright e2e tests |
| `ecc:pr-test-analyzer` | Review test coverage on a PR |

---

### Code Quality

| Skill | Use it when |
|-------|-------------|
| `ecc:code-simplifier` | Simplify and clean up code |
| `ecc:refactor-cleaner` | Remove dead code and duplicates |
| `ecc:performance-optimizer` | Profile and fix performance bottlenecks |
| `ecc:silent-failure-hunter` | Find swallowed errors and bad fallbacks |
| `ecc:comment-analyzer` | Check comments for accuracy and staleness |
| `ecc:type-design-analyzer` | Analyse type safety and invariant expression |

---

### Agent Orchestration

| Skill | Use it when |
|-------|-------------|
| `ecc:gan-planner` | Expand a prompt into a full product spec |
| `ecc:gan-generator` | Implement features, iterate from evaluator feedback |
| `ecc:gan-evaluator` | Test the live app and score it against a rubric |
| `ecc:loop-operator` | Operate and intervene in autonomous agent loops |
| `ecc:harness-optimizer` | Improve your local agent harness configuration |

---

### Open Source

| Skill | Use it when |
|-------|-------------|
| `ecc:opensource-forker` | Fork and strip a project for open-sourcing |
| `ecc:opensource-sanitizer` | Verify no secrets or PII in a fork before release |
| `ecc:opensource-packager` | Generate README, CONTRIBUTING, LICENSE, setup.sh |

---

### Miscellaneous

| Skill | Use it when |
|-------|-------------|
| `ecc:doc-updater` | Update READMEs and codemaps after shipping |
| `ecc:seo-specialist` | Technical SEO audit and meta tag fixes |
| `ecc:a11y-architect` | Accessibility audit and WCAG 2.2 fixes |
| `ecc:marketing-agent` | Campaign planning, landing pages, ad copy |
| `ecc:chief-of-staff` | Triage email, Slack, and LINE messages |
| `ecc:homelab-architect` | Home network design and safe change plans |
| `ecc:checkpoint` | Save session progress for resuming later |
| `ecc:resume-session` | Resume a previously saved session |

---

## 8. Cybersecurity Skills

500+ specialised cybersecurity skills covering the full spectrum of offensive and defensive security. Use these for authorised pentesting, CTF challenges, security research, and defensive work.

> All offensive skills require clear authorisation context. Never use them outside of authorised engagements, CTF competitions, or isolated lab environments.

---

### Threat Intelligence

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:analyzing-threat-actor-ttps-with-mitre-attack` | Map adversary TTPs to MITRE ATT&CK |
| `cybersecurity-skills:collecting-threat-intelligence-with-misp` | Gather and correlate threat intel |
| `cybersecurity-skills:building-threat-intelligence-platform` | Stand up a TIP from scratch |

---

### Incident Response

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:conducting-malware-incident-response` | Respond to a malware infection |
| `cybersecurity-skills:conducting-phishing-incident-response` | Handle a phishing incident |
| `cybersecurity-skills:performing-ransomware-response` | Ransomware IR playbook |
| `cybersecurity-skills:containing-active-breach` | Immediate breach containment steps |

---

### Digital Forensics

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:analyzing-memory-dumps-with-volatility` | Memory forensics (Volatility) |
| `cybersecurity-skills:performing-disk-forensics-investigation` | Disk image analysis |
| `cybersecurity-skills:performing-network-forensics-with-wireshark` | Packet capture analysis |

---

### Penetration Testing (authorised use only)

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:conducting-network-penetration-test` | Full network pentest |
| `cybersecurity-skills:performing-web-application-penetration-test` | Web app pentest |
| `cybersecurity-skills:conducting-cloud-penetration-testing` | Cloud infrastructure pentest |
| `cybersecurity-skills:performing-active-directory-penetration-test` | Active Directory pentest |

---

### Threat Hunting

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:hunting-for-cobalt-strike-beacons` | Find C2 beacons in your environment |
| `cybersecurity-skills:hunting-for-lateral-movement-via-wmi` | Hunt WMI-based lateral movement |
| `cybersecurity-skills:detecting-ransomware-encryption-behavior` | Early ransomware detection |

---

### Hardening & Zero Trust

| Skill | Use it when |
|-------|-------------|
| `cybersecurity-skills:implementing-zero-trust-network-access` | Design and deploy ZTNA |
| `cybersecurity-skills:hardening-linux-endpoint-with-cis-benchmark` | CIS Linux hardening |
| `cybersecurity-skills:implementing-secrets-management-with-vault` | HashiCorp Vault setup |

---

## 9. Understand Anything

Understand Anything is a codebase analysis plugin that builds an interactive knowledge graph from your code — mapping architecture, relationships, and business domains — then surfaces it as a visual dashboard and a set of Q&A, diff, and onboarding tools.

> **Prerequisite for most sub-skills:** run `/understand` once to build the knowledge graph (stored in `.understand-anything/knowledge-graph.json`). Subsequent skills read from that graph — fast and cheap.

---

### `understand`
**What it does:** Scans the codebase and produces a `knowledge-graph.json` — a structured map of every file, function, class, config, service, and their relationships — which powers all other understand-anything skills.

**Use it when:**
- You've just cloned or onboarded onto an unfamiliar repo and want a structured map
- The codebase has changed significantly and you want to refresh the graph
- You want to enable auto-updates on every commit (`--auto-update`)

**How to invoke:** `/understand [path] [--full] [--auto-update] [--language <lang>]`

**Options:**
- `--full` — force a complete rebuild (ignores cached graph)
- `--auto-update` — regenerate graph automatically on each git commit
- `--language zh` (or any ISO 639-1 code) — generate summaries in another language
- A directory path — analyse a repo other than the current one

---

### `understand-dashboard`
**What it does:** Launches a local interactive web dashboard to explore the knowledge graph visually — nodes, edges, clusters, and component relationships all rendered as a navigable graph.

**Use it when:**
- You want to visually browse architecture before diving into code
- You're presenting or explaining a codebase to someone else
- You want an overview of component relationships at a glance

**How to invoke:** `/understand-dashboard [project-path]`

**Note:** Requires `/understand` to have been run first.

---

### `understand-chat`
**What it does:** Answers natural-language questions about the codebase by querying the knowledge graph — "what does X depend on?", "which files touch auth?", "how does the payment flow work?"

**Use it when:**
- You want to ask questions about the repo without reading every file
- You need to find where something is implemented or how components connect
- You're navigating an unfamiliar codebase

**How to invoke:** `/understand-chat <your question>`

**Example:** `/understand-chat how does the checkout flow work?`

---

### `understand-explain`
**What it does:** Provides a deep, thorough explanation of a specific file, function, class, or module — its purpose, how it works internally, what it depends on, and what depends on it.

**Use it when:**
- You're reading a complex file and want it explained in plain English
- You want to understand what a function does before modifying it
- You're reviewing someone else's code

**How to invoke:** `/understand-explain <file-path or function name>`

---

### `understand-diff`
**What it does:** Analyses the current git diff against the knowledge graph to explain what changed, which components are affected, and what risks or downstream effects to be aware of.

**Use it when:**
- You've made changes and want to understand their blast radius before pushing
- You're reviewing a PR and want automated impact analysis
- You want to catch unintended side effects early

**How to invoke:** `/understand-diff` (runs against current unstaged/staged changes)

---

### `understand-domain`
**What it does:** Extracts business domain knowledge from the codebase — domains, business flows, and process steps — and generates an interactive horizontal flow diagram showing how business processes are implemented in code.

**Use it when:**
- You want to understand the business logic rather than the technical structure
- You need to map what the code *does* in business terms (checkout, onboarding, billing, etc.)
- You're doing domain-driven design work and want to see existing domain boundaries

**How to invoke:** `/understand-domain [--full]`

- Without `--full`: derives domain knowledge from an existing knowledge graph (fast)
- `--full`: forces a fresh file scan even if a graph exists

---

### `understand-knowledge`
**What it does:** Analyses a Karpathy-pattern LLM wiki knowledge base (raw sources + wiki markdown + schema file) and generates an interactive knowledge graph with entity extraction, implicit relationships, and topic clustering.

**Use it when:**
- You maintain a personal or team LLM wiki (markdown files with `[[wikilinks]]`)
- You want to see the relationships between topics, articles, and sources visualised
- You're building a knowledge base and want to explore its structure

**How to invoke:** `/understand-knowledge [wiki-directory]`

**Pattern it detects:** `index.md` + multiple `.md` files with `[[wikilink]]` syntax, optionally a `raw/` sources directory and a `CLAUDE.md` schema file.

---

### `understand-onboard`
**What it does:** Generates a comprehensive written onboarding guide for new team members — covering architecture, key entry points, important conventions, and how to navigate the codebase — derived from the knowledge graph.

**Use it when:**
- A new engineer is joining the team and needs a written guide
- You want a living onboarding doc that reflects the actual code, not hand-written docs that go stale
- You're returning to a project after a long break

**How to invoke:** `/understand-onboard`

---

## 10. Pi Agent

Pi is an open-source AI coding agent installed at `/Users/karanmanoharan/.hermes/node/bin/pi`. Current version: **0.80.2**.

**What it does:** A terminal-based coding agent that works with Claude, OpenAI, Google, and other LLM providers through a unified interface — an alternative to running Claude Code directly.

**Use it when:**
- You want to use a different model provider (Google, OpenAI) with a Claude-Code-style workflow
- You want to compare agent outputs across providers on the same task
- You're testing agent behaviour on a provider other than Anthropic

**How to launch:** Open a terminal and run `pi` (PATH is set via `~/.zshrc` line 22)

**Install extensions:** `pi install npm:@foo/bar` or `pi install git:github.com/user/repo`

---

## Quick Reference: "What skill should I use for X?"

| Situation | Skill to use |
|-----------|-------------|
| Push code safely with full validation | `/no-mistakes` |
| Walk away and let an agent keep working | `/gnhf` |
| Get AI code review on current branch | `/roborev-review-branch` |
| Automatically fix review findings | `/roborev-refine` |
| Generate a visual diagram | `/generate-web-diagram` |
| Create a slide deck | `/generate-slides` |
| Make a product demo video | `hyperframes:product-launch-video` |
| Turn a PR into a video | `hyperframes:pr-to-video` |
| Open a PR with working proof | `/pr` (via `skills:pr`) |
| Set up one-command dev server | `skills:dev-local-setup` |
| Add end-to-end tests | `skills:e2e-setup` |
| Fix a broken build | `ecc:build-error-resolver` |
| Review React code | `ecc:react-reviewer` |
| Scan for security issues | `ecc:security-reviewer` |
| Run a pentest (authorised) | `cybersecurity-skills:conducting-network-penetration-test` |
| Step away from the fleet | `/afk` |
| Update firstmate | `/updatefirstmate` |
| Turn a response into a visual HTML page | `/lavish` |
| Explain a codebase visually | `/visual-explainer` |
| Plan before coding | `ecc:planner` |
| Remove dead code | `ecc:refactor-cleaner` |
| Open source a private project | `ecc:opensource-forker` → `ecc:opensource-sanitizer` → `ecc:opensource-packager` |
| Build a knowledge graph of a codebase | `/understand` |
| Browse architecture visually | `/understand-dashboard` |
| Ask questions about the codebase | `/understand-chat <question>` |
| Explain a specific file or function | `/understand-explain <path>` |
| Analyse blast radius of current diff | `/understand-diff` |
| Map business domains in the code | `/understand-domain` |
| Generate onboarding guide for new devs | `/understand-onboard` |
| Visualise a markdown wiki knowledge base | `/understand-knowledge [wiki-dir]` |
