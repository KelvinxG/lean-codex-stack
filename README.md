# Lean Codex Stack

**A practical setup for TypeSafe AI, Caveman, `AGENTS.md`, and token-conscious Codex workflows.**

[Read the complete Markdown guide](./CODEX-TYPESAFE-CAVEMAN-GUIDE.md) · [Download the PDF](./codex-typesafe-caveman-setup-guide.pdf)

## Overview 

Lean Codex Stack is a reusable reference for organizing AI-assisted development without putting every rule, workflow, and preference into one oversized prompt.

It explains how to separate four concerns:

| Layer | Purpose |
| --- | --- |
| `AGENTS.md` | Stable project rules, commands, boundaries, and quality gates |
| TypeSafe AI skill | Reusable guidance for typed semantic judgments and probabilities |
| Caveman | Session-level control over response conciseness |
| Task prompt | The current outcome, scope, constraints, and acceptance criteria |

The core workflow is:

> Start clear, compress during routine execution, and expand again for risk, explanation, and handoff.

## What is included

This repository contains:

- a comprehensive Markdown guide;
- a professionally formatted 14-page PDF;
- TypeSafe AI installation and usage guidance;
- Caveman installation and mode-selection guidance;
- a recommended project structure;
- a token-conscious `AGENTS.md` template;
- practical Codex prompts and session workflows;
- troubleshooting and maintenance advice;
- and caveats for measuring token savings honestly.

## Repository files

```text
lean-codex-stack/
├── README.md
├── CODEX-TYPESAFE-CAVEMAN-GUIDE.md
└── codex-typesafe-caveman-setup-guide.pdf
```

| File | Description |
| --- | --- |
| [`README.md`](./README.md) | Repository overview and quick start |
| [`CODEX-TYPESAFE-CAVEMAN-GUIDE.md`](./CODEX-TYPESAFE-CAVEMAN-GUIDE.md) | Complete web-friendly documentation |
| [`codex-typesafe-caveman-setup-guide.pdf`](./codex-typesafe-caveman-setup-guide.pdf) | Shareable and printable reference guide |

## Quick start

### 1. Install the TypeSafe AI skill

From your project root:

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai
```

Choose **Codex** when prompted. The installation is project-local by default.

To install it globally:

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai -g
```

### 2. Install Caveman for Codex

For a global installation:

```bash
npx skills add JuliusBrussee/caveman --skill '*' -a codex --yes -g
```

The shorter upstream quick-start command is:

```bash
npx skills add JuliusBrussee/caveman -g
```

### 3. Select a response mode

```text
/caveman lite
/caveman full
/caveman ultra
/caveman off
```

You can also return to regular responses with:

```text
normal mode
```

## Which Caveman mode should I use?

| Mode | Recommended for | Avoid for |
| --- | --- | --- |
| Normal | Architecture, risks, approvals, explanations, and handoff | Routine narration |
| Lite | Discovery, collaboration, and unclear debugging | Maximum compression |
| Full | Implementation, tests, fixes, and review loops | Ambiguous high-risk decisions |
| Ultra | Narrow mechanical work with strong verification | New systems, migrations, security, or destructive work |

Select the mode according to the cost of misunderstanding the task—not only the desired response length.

## Recommended project structure

```text
your-project/
├── AGENTS.md
├── .agents/
│   └── skills/
│       └── typesafe-ai/
│           └── SKILL.md
├── docs/
├── src/
├── tests/
└── README.md
```

Do not copy an entire `SKILL.md` into `AGENTS.md`.

- Put stable repository instructions in `AGENTS.md`.
- Put reusable specialist workflows in skills.
- Put the current objective and constraints in the task prompt.

Codex reads `AGENTS.md` as persistent project guidance and discovers repository skills from `.agents/skills`. See the official OpenAI documentation for [custom instructions with `AGENTS.md`](https://developers.openai.com/en-US/docs/agent-configuration/agents-md) and [building skills](https://developers.openai.com/en-US/docs/build-skills).

## Minimal `AGENTS.md` example

```md
# Project instructions

## Scope
- Work only in this repository unless explicitly asked.
- Preserve unrelated user changes.

## Commands
- Install: <canonical install command>
- Test: <fast test command>
- Full check: <lint + typecheck + tests>

## Working style
- Inspect before editing.
- Prefer the smallest complete change.
- Do not narrate routine reads or successful commands.
- Report decisions, failures, risks, and verification results.

## TypeSafe
- Use the installed TypeSafe AI skill for typed semantic decisions,
  routing, ranking, extraction, verification, or confidence-aware flows.
- Keep deterministic policy and side effects in ordinary code.

## Quality gates
- Add or update focused tests for behavior changes.
- Never claim success without running the relevant checks.
```

## Example workflow

### Explore and design

```text
normal mode

Use the installed TypeSafe AI skill.
Identify which behavior should remain deterministic and which behavior
requires a typed semantic judgment.

Explain the architecture, uncertainty policy, risks, and tests.
Do not edit yet.
```

### Implement

```text
/caveman full

Implement the approved plan with the smallest complete change.
Preserve unrelated work.
Run focused tests first, then the required full check.
Report changed behavior, verification, and remaining risks only.
```

### Hand off

```text
normal mode

Summarize the final behavior, important decisions, verification,
remaining risks, and rollback path.
```

## Token-saving reality check

Shorter answers do not automatically produce cheaper coding sessions.

Total usage may include:

- permanent instructions;
- repository context;
- source files and diffs;
- tool definitions and output;
- test logs;
- model responses;
- clarification turns;
- and retries.

Measure the total cost of completing the same task at the same quality bar. A shorter response that causes another debugging cycle is not a saving.

## Read the complete guide

The complete documentation includes detailed examples, a prompt library, troubleshooting, maintenance guidance, source links, and a one-page quick reference.

- [Read the complete Markdown guide](./CODEX-TYPESAFE-CAVEMAN-GUIDE.md)
- [Download the PDF reference](./codex-typesafe-caveman-setup-guide.pdf)

## Primary sources

- [Official OpenAI documentation: `AGENTS.md`](https://developers.openai.com/en-US/docs/agent-configuration/agents-md)
- [Official OpenAI documentation: skills](https://developers.openai.com/en-US/docs/build-skills)
- [TypeSafe Agent Skills](https://github.com/typesafe-ai/skills)
- [TypeSafe documentation](https://docs.typesafe.ai/llms.txt)
- [Caveman](https://github.com/JuliusBrussee/caveman)
- [Caveman installation guide](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- [Caveman measurement caveats](https://github.com/JuliusBrussee/caveman/blob/main/docs/HONEST-NUMBERS.md)

## Important notice

This is an independent community reference, not official documentation from OpenAI, TypeSafe, or Caveman.

Commands, defaults, supported platforms, licensing, telemetry, and product behavior may change. Review the current upstream documentation and your organization's security requirements before installing third-party tooling.

## Contributing

Corrections and improvements are welcome. Useful contributions include:

- updated installation commands;
- clearer workflow examples;
- measured token-usage comparisons;
- platform-specific troubleshooting;
- and corrections backed by primary sources.

When proposing a change, describe the source, environment, and verification method.

## later improvement

- snapshots of real workflow and token usage