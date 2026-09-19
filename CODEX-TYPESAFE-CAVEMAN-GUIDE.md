# A Practical Codex Setup for TypeSafe AI, Caveman, and Lower-Token Workflows

> A reusable guide to separating project instructions, specialist skills, and session-level response modes in Codex.

[Download the complete PDF guide](./codex-typesafe-caveman-setup-guide.pdf)

## Why I created this guide

As AI coding workflows grow, it is easy to mix several different concerns into one large prompt or `AGENTS.md` file:

- permanent project rules;
- reusable technical knowledge;
- instructions for the current task;
- response-length preferences;
- and attempts to reduce token usage.

That usually produces duplicated context, unclear instructions, and unnecessary maintenance.

This guide presents a cleaner model for using **Codex**, the **TypeSafe AI skill**, and **Caveman** together:

| Layer | Responsibility |
| --- | --- |
| `AGENTS.md` | Stable, project-specific rules and quality gates |
| TypeSafe AI skill | Reusable guidance for typed AI judgments and probabilities |
| Caveman | Session-level control over response conciseness |
| Current prompt | The immediate task, constraints, scope, and expected outcome |

The central idea is simple:

> Keep permanent instructions small, load specialist guidance only when relevant, and change communication style according to the phase and risk of the work.

## What the complete guide covers

The accompanying PDF is a comprehensive operational reference covering:

1. Installing TypeSafe AI skills for Codex.
2. Deciding between project-local and global skill installation.
3. Using TypeSafe for routing, ranking, extraction, verification, and confidence-aware workflows.
4. Installing Caveman for Codex.
5. Switching between `lite`, `full`, `ultra`, and normal mode.
6. Choosing a mode based on task risk and ambiguity.
7. Structuring a project so skills remain separate from source code and standing instructions.
8. Writing a concise, token-conscious `AGENTS.md`.
9. Running practical architecture, implementation, debugging, review, and maintenance workflows.
10. Using reusable prompts for TypeSafe projects and low-narration coding sessions.
11. Measuring token savings honestly.
12. Troubleshooting skill discovery, activation, and unexpectedly high usage.

It also contains a one-page quick reference and links to the upstream documentation used to verify the commands.

## 1. Install the TypeSafe AI skill

From the root of your project, run:

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai
```

Choose **Codex** when prompted.

The installation is project-local by default. A typical project structure is:

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

To install the skill globally instead:

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai -g
```

Use project-local installation when you want the skill to be reviewable and versioned with a specific repository. Use a global installation when you intentionally want the same skill available across unrelated projects.

### Do not copy the skill into `AGENTS.md`

`AGENTS.md` and `SKILL.md` serve different purposes.

`AGENTS.md` should contain stable project rules, such as:

- canonical install, test, lint, and build commands;
- architectural boundaries;
- repository-specific conventions;
- safety constraints;
- and required quality checks.

A skill should contain reusable specialist guidance, such as how to design and implement TypeSafe workflows.

Copying an entire skill into `AGENTS.md` duplicates content, increases repeated context, and makes updates harder to review.

## 2. What TypeSafe is useful for

TypeSafe is useful when ordinary code needs a bounded semantic judgment rather than open-ended generated text.

Common patterns include:

- routing a request to one of several handlers;
- ranking candidates against a defined criterion;
- extracting or selecting a value from known candidates;
- verifying a claim against supplied evidence;
- interpreting application state;
- and escalating uncertain decisions.

The recommended division of responsibility is:

```text
Application code
    owns rules, policy, validation, thresholds, side effects, and audit

TypeSafe judgment
    supplies a typed semantic decision and probabilities
```

Typed output guarantees the structure of the interface. It does not guarantee that the answer is correct. Representative testing, confidence policy, and safe escalation are still required.

### Example TypeSafe project prompt

```text
Create a FastAPI service for inbound support tickets.

Use the installed TypeSafe AI skill where semantic judgment helps.

Requirements:
- route tickets to billing, technical, account, or other
- return typed probabilities
- require human review below an evaluated threshold
- keep authentication, policy, and side effects in code
- begin with the architecture and representative test cases
- implement incrementally after the design is consistent
```

Before writing integration code, the agent should consult the current TypeSafe API or SDK documentation instead of inventing version-dependent methods.

## 3. Install Caveman for Codex

For a global Codex installation:

```bash
npx skills add JuliusBrussee/caveman --skill '*' -a codex --yes -g
```

The shorter upstream quick-start command is:

```bash
npx skills add JuliusBrussee/caveman -g
```

Restart or reload Codex if the new skill is not discovered in the current session.

Caveman's response-style skill primarily reduces prose output. The project also offers a separate optional proxy intended to reduce repetitive input such as logs, JSON, diffs, and search results. These are distinct tools and should be evaluated separately.

## 4. Switch between Caveman modes

```text
/caveman lite
/caveman full
/caveman ultra
/caveman off
```

You can also return to ordinary responses with:

```text
normal mode
```

### Mode-selection guide

| Mode | Recommended use | Avoid for |
| --- | --- | --- |
| Normal | Architecture, explanation, risk analysis, approvals, and handoff | Routine narration |
| Lite | Repository discovery, collaboration, and unclear debugging | Maximum compression |
| Full | Routine implementation, testing, fixing, and review loops | Ambiguous high-risk decisions |
| Ultra | Known mechanical edits with strong verification | New systems, migrations, security, or destructive work |

Do not choose a mode based only on the desire for shorter answers. Choose it based on the cost of misunderstanding the task.

A good rhythm is:

1. Start in normal or lite mode while defining scope and architecture.
2. Switch to full mode after the important decisions are settled.
3. Use ultra only for narrow, reversible, easy-to-verify work.
4. Return to normal mode for risks, approvals, explanations, and final handoff.

Conciseness must never hide security warnings, irreversible actions, exact error messages, important uncertainty, or required confirmations.

## 5. A token-conscious `AGENTS.md`

The most useful `AGENTS.md` is short, concrete, and testable.

```md
# Project instructions

## Scope
- Work only in this repository unless explicitly asked.
- Preserve unrelated user changes.

## Architecture
- Keep domain logic independent of HTTP and persistence.
- Put external integrations behind interfaces.

## Commands
- Install: <one canonical command>
- Test: <fast test command>
- Full check: <lint + typecheck + tests>

## Working style
- Inspect before editing.
- Prefer the smallest complete change.
- Do not narrate routine file reads or successful commands.
- Report decisions, failures, risks, and verification results.
- Ask only when a missing choice materially changes the result.

## TypeSafe
- For typed semantic decisions, routing, ranking, extraction,
  verification, or confidence-aware workflows, use the installed
  TypeSafe AI skill and current TypeSafe documentation.
- Keep deterministic policy and side effects in code.

## Quality gates
- Add or update focused tests for behavior changes.
- Never claim success without running the relevant checks.
```

Useful token-saving rules include:

- do not narrate routine file reads or successful commands;
- report decisions, failures, risks, and verification results;
- search narrowly before opening entire files;
- run the smallest relevant test first;
- avoid restating instructions that already exist in `AGENTS.md`;
- stop after the requested outcome is verified;
- and link to detailed procedures instead of copying them into permanent context.

Do not compress away exceptions, acceptance criteria, safety boundaries, or exact commands. Ambiguous instructions frequently cost more through rework than they save in context.

## 6. A practical Codex workflow

### Architecture and discovery

```text
normal mode

Use the installed TypeSafe AI skill.
Inspect the existing application and identify which behavior should remain
deterministic and which behavior requires a typed semantic judgment.

Explain the proposed architecture, uncertainty policy, risks, and tests.
Do not edit yet.
```

### Implementation

```text
/caveman full

Implement the approved plan with the smallest complete change.
Preserve unrelated work.
Run focused tests first, then the required full check.
Report changed behavior, verification, and remaining risks only.
```

### Narrow maintenance

```text
/caveman ultra

Fix the failing null-handling test only.
Preserve public behavior.
Run the focused test.

If the failure has a different root cause, stop and switch to normal mode
with the evidence.
```

### Final handoff

```text
normal mode

Summarize the final behavior, important implementation decisions,
verification performed, remaining risks, and rollback path.
```

## 7. Token-saving caveats

Shorter visible responses do not automatically mean a cheaper end-to-end coding session.

Total usage may include:

- permanent instructions;
- repository context;
- tool definitions;
- source files;
- command output;
- test logs;
- diffs;
- cached and uncached input;
- model output;
- clarification turns;
- and retries after an incorrect or incomplete result.

A concise-response skill can reduce output while adding a small amount of input instruction. In many agentic coding tasks, input and tool output may be larger than conversational prose.

Ultra compression may also become more expensive if missing context causes errors, clarification, or repeated work.

The right comparison is not simply "normal response length versus Caveman response length." It is:

> Total cost and time required to complete the same task at the same quality bar.

Measure representative tasks using:

| Variant | Successful? | Input tokens | Output tokens | Retries | Human review time |
| --- | --- | ---: | ---: | ---: | ---: |
| Normal |  |  |  |  |  |
| Lite |  |  |  |  |  |
| Full |  |  |  |  |  |
| Ultra |  |  |  |  |  |

Keep a mode or compression tool only when it reduces the complete cost without increasing failures or review effort.

## Recommended default setup

For most individual developers, a sensible starting configuration is:

1. Install TypeSafe project-locally.
2. Install Caveman globally if you want it across repositories.
3. Keep `AGENTS.md` short and project-specific.
4. Start unfamiliar work in normal or lite mode.
5. Use full mode for routine implementation.
6. Reserve ultra for narrow, verified operations.
7. Return to normal mode for high-risk decisions and final handoff.
8. Measure total workflow cost before making claims about savings.

## Complete PDF reference

The full guide includes detailed installation notes, mode-selection guidance, project structure, prompts, troubleshooting, maintenance advice, sources, and a one-page quick reference.

**Download it here:** [Codex Setup Guide: TypeSafe AI and Caveman](./codex-typesafe-caveman-setup-guide.pdf)

## Upstream sources

- [TypeSafe Agent Skills](https://github.com/typesafe-ai/skills)
- [TypeSafe documentation index](https://docs.typesafe.ai/llms.txt)
- [Caveman repository](https://github.com/JuliusBrussee/caveman)
- [Caveman installation guide](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- [Caveman measurement caveats](https://github.com/JuliusBrussee/caveman/blob/main/docs/HONEST-NUMBERS.md)

> Commands, defaults, licensing, telemetry, and supported integrations may change. Review the current upstream documentation and your organization's security requirements before installing third-party tooling.

---

## LinkedIn-ready announcement

I created a practical guide for combining **Codex**, **TypeSafe AI skills**, and **Caveman** without turning `AGENTS.md` into a giant instruction dump.

The guide covers:

- installing and using TypeSafe AI skills with Codex;
- deciding what belongs in `AGENTS.md` versus a reusable skill;
- installing Caveman and switching between lite, full, ultra, and normal modes;
- choosing response modes based on task risk and ambiguity;
- practical prompts for architecture, implementation, debugging, and handoff;
- project structure and token-conscious `AGENTS.md` rules;
- and why shorter responses do not always mean lower total cost.

My main takeaway:

**Start clear, compress during routine execution, and expand again for risk, explanation, and handoff. Measure the complete workflow, not just the visible response.**

The full PDF and Markdown reference are available in this GitHub repository.

#OpenAI #Codex #TypeSafeAI #AIAgents #DeveloperTools #SoftwareEngineering #PromptEngineering #LLM #Productivity

## License and sharing

You may adapt the explanatory text in this companion document for your own repository or social post. Third-party product names, projects, source code, and documentation remain subject to their respective licenses and terms.

