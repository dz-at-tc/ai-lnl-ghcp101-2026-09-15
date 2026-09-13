# GitHub Copilot 101: From Autocomplete to Agentic Workflows

Lunch and learn for **Tuesday, September 15, 2026**

GitHub Copilot can do much more than finish the next line of code. This session follows a practical progression from inline suggestions, to focused conversations in VS Code, to delegating substantial repository-level work through GitHub Copilot CLI.

The goal is not to hand development over to AI. It is to learn where Copilot is useful, how to give it enough context, and how to remain accountable for the result.

> **Format:** 60-minute, demo-led session with discussion. No local setup is required to attend.

## Table of contents

- [What you will learn](#what-you-will-learn)
- [Session itinerary](#session-itinerary)
- [The progression](#the-progression)
  - [1. Inline suggestions: complete the code](#1-inline-suggestions-complete-the-code)
  - [2. VS Code Chat: collaborate on a change](#2-vs-code-chat-collaborate-on-a-change)
  - [3. Copilot CLI: delegate a repository-level task](#3-copilot-cli-delegate-a-repository-level-task)
- [A repeatable workflow](#a-repeatable-workflow)
- [Prompt patterns to reuse](#prompt-patterns-to-reuse)
- [Trust, review, and responsible adoption](#trust-review-and-responsible-adoption)
- [After the session](#after-the-session)
- [Reference and self-study](#reference-and-self-study)

## What you will learn

By the end of the session, attendees should be able to:

- choose between inline suggestions, IDE chat, and an agentic CLI workflow;
- give Copilot useful context, constraints, and success criteria;
- use Copilot to understand, test, refactor, document, and port code;
- inspect and validate generated changes instead of accepting them on trust; and
- identify practical ways to evaluate Copilot adoption on a team.

## Session itinerary

| Time | Topic | What we will see |
|---:|---|---|
| 0:00-0:05 | **Framing** | Copilot as a tool, not an oracle; what changes as tasks grow in scope |
| 0:05-0:13 | **Stage 1: Inline suggestions** | Completing repetitive code and tests without leaving the editor |
| 0:13-0:23 | **Stage 2: VS Code Chat** | Explaining unfamiliar code and making a small, scoped change |
| 0:23-0:43 | **Stage 3: Copilot CLI** | Exploring a repository, planning work, editing multiple files, adding tests, and verifying the result |
| 0:43-0:50 | **Trust and adoption** | Review practices, guardrails, and signals that matter to teams and leaders |
| 0:50-1:00 | **Discussion and next steps** | Questions, use cases from the room, and a one-week practice challenge |

## The progression

The three stages are not a maturity ranking. They are different tools for different-sized jobs.

| Mode | Best fit | Human role | Typical scope |
|---|---|---|---|
| Inline suggestions | Clear, local, repetitive work | Direct every step and accept or reject each suggestion | A line, block, or function |
| VS Code Chat | Questions and bounded edits with visible context | Describe the outcome, steer, and review | A symbol, file, or small change |
| Copilot CLI | Multi-step work requiring repository exploration and tools | Set constraints, approve actions, inspect diffs, and validate | Multiple files or an entire repository |

### 1. Inline suggestions: complete the code

**Demo idea:** Start a small function or unit test, write a descriptive name and signature, and compare the suggested completion with the intended behavior.

Good uses include:

- repetitive or boilerplate code;
- completing a well-named function;
- generating test cases from an established pattern; and
- translating an explanatory comment into a small implementation.

**Watch for:** plausible code that misses an edge case, uses the wrong API, or merely reproduces a weak local pattern.

### 2. VS Code Chat: collaborate on a change

**Demo idea:** Select an unfamiliar function and ask Copilot to explain its purpose, inputs, outputs, side effects, and failure modes. Then request one tightly scoped improvement.

```text
Explain the selected function to a developer who is new to this repository.
Cover its inputs, output, side effects, dependencies, and likely edge cases.
Do not change any code.
```

Follow with a bounded implementation request:

```text
Refactor this function to separate validation from business logic.
Preserve its public behavior and follow the patterns already used in this project.
Add or update focused tests, then explain the changes and any remaining risks.
```

Chat works best when the relevant files or selection are in context and the request has a clear boundary.

### 3. Copilot CLI: delegate a repository-level task

GitHub Copilot CLI can inspect a codebase, build a plan, edit multiple files, run commands, and iterate on failures. That makes it useful for work that is larger than a single editor interaction.

**Demo flow:**

1. Start in the repository root and ask for an architecture and test-strategy overview.
2. Ask Copilot to identify the smallest meaningful change and propose a plan.
3. Review the plan and explicitly state constraints.
4. Let Copilot implement while reviewing requested tool permissions.
5. Inspect the diff and ask questions about surprising choices.
6. Run the relevant tests, linting, or build.
7. Iterate until the result and evidence satisfy the request.

Representative tasks:

| Task | Example request |
|---|---|
| Understand unfamiliar code | `Trace how an incoming request reaches the data layer. Cite the relevant files and call out error handling.` |
| Refactor safely | `Refactor this module to remove duplication without changing its public API. Add characterization tests first.` |
| Add a test foundation | `This repository has no tests. Identify its framework and seams, propose the smallest useful test setup, and implement tests for the core behavior.` |
| Improve documentation | `Compare the README with the actual setup and scripts. Correct stale instructions and add a concise troubleshooting section.` |
| Port code | `Port this component from language A to language B. Preserve observable behavior, use idiomatic B, and create parity tests for edge cases.` |

For a language port, treat the original implementation and tests as a behavioral specification—not as proof that a line-by-line translation is correct.

## A repeatable workflow

Use this loop for anything larger than a trivial completion:

1. **Orient:** Ask Copilot to inspect the repository, cite relevant files, and explain the current behavior.
2. **Specify:** State the outcome, scope, constraints, examples, and what must not change.
3. **Plan:** For substantial work, review the proposed approach before edits begin.
4. **Act:** Let Copilot make a bounded change and surface errors rather than hiding them.
5. **Inspect:** Read the diff. Ask why a dependency, abstraction, or behavior changed.
6. **Verify:** Run the smallest relevant tests, formatter, linter, type-checker, or build.
7. **Iterate:** Correct gaps with concrete feedback. Start a fresh conversation when old context becomes distracting.

Copilot accelerates this loop; it does not remove any step.

## Prompt patterns to reuse

A useful prompt usually contains:

> **Context + outcome + constraints + verification**

```text
In this repository, [context].

I need [observable outcome].

Preserve [behavior/API/compatibility]. Follow [existing patterns or standards].
Do not [out-of-scope or risky action].

Before editing, inspect [relevant area] and propose a short plan.
After editing, run [specific checks] and summarize the diff and remaining risks.
```

Useful follow-ups:

- `Show me where in the repository you found that assumption.`
- `What edge cases are not covered by these tests?`
- `Why is this dependency or abstraction necessary?`
- `Can this be solved with a smaller change?`
- `Review the diff against my original requirements before continuing.`
- `The check failed. Diagnose the root cause; do not weaken or delete the test.`

When a result is weak, add missing context or constraints instead of repeatedly asking Copilot to "try again."

## Trust, review, and responsible adoption

**For every generated change:**

- understand the code before merging it;
- inspect the complete diff, including configuration and dependency changes;
- run automated checks and test important behavior yourself;
- check security, privacy, licensing, accessibility, and maintainability as applicable;
- never put secrets, credentials, customer data, or other unauthorized content into a prompt; and
- use normal peer review and repository protections—AI-generated code does not get a shortcut.

**For teams and leaders:**

- begin with bounded, low-risk workflows and make expectations explicit;
- teach review and verification, not just prompting;
- preserve accountability: the author and reviewer remain responsible for what ships;
- use organization policies and approved tools to match the risk profile; and
- evaluate outcomes such as cycle time, review quality, test coverage, developer satisfaction, and escaped defects—not lines of generated code or suggestion acceptance alone.

Copilot is strongest at reducing mechanical effort and accelerating feedback. Its value is realized when that saved attention is reinvested in design, correctness, and collaboration.

## After the session

Try this one-week practice path:

1. **Day 1:** Use inline suggestions for one repetitive task; reject anything you cannot explain.
2. **Day 2:** Ask Chat to explain an unfamiliar function, then verify the explanation in the code.
3. **Day 3:** Ask Chat for tests around a small existing behavior.
4. **Day 4:** Use Copilot CLI to inspect a repository and propose—without implementing—a small improvement.
5. **Day 5:** Let the CLI implement that bounded change, then review the diff and run the checks.

Keep a short note of the task, time saved or added, corrections required, and what you learned. This gives you better evidence than a raw usage count.

## Reference and self-study

### Start here

- [What is GitHub Copilot?](https://docs.github.com/en/copilot/get-started/what-is-github-copilot)
- [GitHub Copilot quickstart](https://docs.github.com/en/copilot/get-started/quickstart)
- [Best practices for using GitHub Copilot](https://docs.github.com/en/copilot/get-started/best-practices)
- [Prompt engineering for GitHub Copilot Chat](https://docs.github.com/en/copilot/concepts/prompting/prompt-engineering)

### Practice next

- [GitHub Copilot tutorials](https://docs.github.com/en/copilot/tutorials)
- [GitHub Copilot Cookbook](https://docs.github.com/en/copilot/tutorials/copilot-cookbook)
- [GitHub Copilot CLI for Beginners](https://github.com/github/copilot-cli-for-beginners), a hands-on course that progressively improves a sample application through explanation, testing, debugging, and automation
- [Getting started with GitHub Copilot CLI](https://docs.github.com/en/copilot/get-started/cli-quickstart)
- [Best practices for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-best-practices)
- [GitHub Copilot CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

### Go deeper

- [GitHub Copilot features](https://docs.github.com/en/copilot/get-started/features)
- [Adopting GitHub Copilot in your enterprise](https://docs.github.com/en/copilot/get-started/enterprise-ai-governance)
- [Integrating AI agents into the software development lifecycle](https://docs.github.com/en/copilot/tutorials/roll-out-at-scale/enable-developers/integrate-ai-agents)
- [Awesome GitHub Copilot](https://github.com/github/awesome-copilot), a community collection of agents, instructions, skills, hooks, and other customizations
- [Awesome Copilot Learning Hub](https://awesome-copilot.github.com/learning-hub)

> Copilot changes quickly. Use the linked documentation as the source of truth for current features, setup steps, plans, and organizational controls.
