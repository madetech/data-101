# Using Generative AI (Claude) – A Practical Guide

This guide describes my experience using **Claude Code** for planning, development, and testing across projects.  
It is designed for developers, analysts, and data scientists with mixed experience levels.

---

## 1. Overview

Claude Code provides an interactive development companion inside the terminal.  
It supports **planning**, **code editing**, **testing**, and **memory-based documentation** through simple commands and prompts.

I use Claude Code to:
- Plan and review features using Test-Driven Development (TDD)
- Maintain consistent engineering standards across projects
- Reduce context switching between chat and code
- Encourage clear checkpoints and auditability

---

## 2. Setup

### Install Claude Code

```bash
brew install --cask claude-code
```

This installs Claude Code and associated editor integrations for macOS.

### Launch Claude Code

```bash
claude
```

Key commands available inside a session:

| Command | Description |
|----------|--------------|
| `/clear` | Clears the current context window |
| `/context` | Displays token usage and current context size |
| `/init` | Creates a CLAUDE.md file in your project |
| `/exit` | Closes the current session |

Additional keyboard shortcuts:

```
! for bash mode       double tap esc to clear input      ctrl + _ to undo
/ for commands        shift + tab to cycle modes         ctrl + z to suspend
@ for file paths      ctrl + o for verbose output        ctrl + v to paste images
# to memorise         ctrl + t to show todos
                      tab to toggle thinking
                      shift + ⏎ for newline
```

> **Tip:** Keep each feature within a single context window to avoid compaction and degraded output quality.

### Authentication

When first launched, Claude Code prompts for login or API key.

**Browser authentication:** If you have a Claude Pro, Team, or Max subscription, authenticate through browser login.

**API key authentication:** If you have an Anthropic API key, provide it when prompted, or set it as an environment variable:

```bash
export ANTHROPIC_API_KEY="your_api_key_here"
```

---

## 3. Understanding Modes

Claude Code has three interaction modes that you cycle through using **Shift + Tab**:

### Default Mode
Claude suggests code changes but requires your confirmation before making any file edits.  
**Use when:** You want full control and manual approval of every change.

### Auto-Accept Edits Mode
Claude can directly modify files without asking for confirmation on each change.  
**Use when:** 
- You have a clear, approved plan
- The scope is well-bounded and understood
- You are actively monitoring the session

**Important:** Never leave auto-accept mode running unattended. You remain responsible for all changes.

### Plan Mode
Claude operates in read-only mode, focusing on reasoning, design, and planning without making any file modifications.  
**Use when:**
- Designing a new feature or approach
- Creating a TDD workflow plan
- Exploring architecture decisions
- You want Claude to think through the solution before touching code

**Cycling through modes:** Press **Shift + Tab** repeatedly to cycle: default → auto-accept edits → plan mode → default.

The current mode should be visible in your Claude Code interface. Always verify which mode you're in before proceeding with work.

---

## 4. Configuration

### Global Prompt Configuration

**File path:** `~/.claude/CLAUDE.md`

This global file defines Claude's tone, coding style, and workflow preferences across all projects.

Example:

```markdown
# Preferences

## Rules

- Be concise with explanations  
- Do not output emojis  
- When investigating an issue, do 5 checks maximum before reporting back  
- Follow TDD (create test → make it fail → implement stub → document → review)  
- Use comments in methods to describe intent before writing logic  
- Re-run all tests after modification for regression coverage  
- Use type hints in function signatures  
- Use `with` when handling files or database connections  
- Use Ruff for linting — do not run linting manually, always use the tool
- Use UV for Python package management  
- Use UK spelling only

## Tasks

- Once a feature is confirmed as complete, read the latest file from `memory_bank/`,  
  create a new version (e.g. `feature_004.md`), and document updates succinctly.  
- Update `README.md` when the memory bank changes to ensure it stays current.
```

> **Note:** Customise these examples for your own tooling and preferences. The key principle is to let specialised tools (like Ruff, Prettier, or ESLint) handle tasks they're designed for—Claude should not attempt manual linting or formatting.

### Project-Level Prompt Configuration

**File path:** `${workspaceFolder}/.claude/CLAUDE.md`

This file defines project-specific objectives and current development context.  
It supplements (not replaces) the global configuration.

> **Note:** Project-level prompts improve focus but increase context size. Keep them scoped to current work.

---

## 5. Running Claude Code

Once installed and configured, run:

```bash
claude
```

You will enter an interactive Claude session. Use `/context` to view current tokens, and `/clear` before starting a new task.  
Claude maintains an in-memory working window, so avoid letting the session grow too large.

**Context hygiene tips:**
- Clear the session when starting a new feature
- Avoid carrying over unrelated conversations
- Keep prompts and outputs focused on one task or component

---

## 6. Planning Features (TDD Workflow)

Use **Shift + Tab** to cycle into **plan mode** before defining your feature implementation.  
In plan mode, Claude will focus on design and planning without making any file modifications.

Claude will not infer Test-Driven Development automatically. You must explicitly instruct it to use **TDD** each time.

### Prompt Pattern

When entering plan mode, start with a directive like:

> "Plan this feature using TDD.  
>  Follow the sequence: create test, make it fail, create stub, implement, retest.  
>  Use minimal code to achieve the goal and stop after each checkpoint for review."  

### Recommended Plan Structure

This is the workflow I follow and recommend. Each plan should describe these stages:

1. **Method Stub Creation**  
   - Define new methods with correct type hints
   - Include Google-style docstrings
   - Body should raise `NotImplementedError`
   - Add inline comments explaining intent

2. **Test Definition**  
   - Write or update tests that define the feature's behaviour
   - Keep tests small and direct — no extra scaffolding

3. **Run Tests (Expect Fail)**  
   - Execute tests to confirm failure (red)
   - Claude must **stop and report back** once tests fail as expected
   - Review the failure output to ensure the tests are valid

4. **Implementation**  
   - Implement the minimal logic required to make the tests pass
   - Avoid adding helpers, abstractions, or refactors
   - Focus only on satisfying the defined tests

5. **Run Tests (Expect Green)**  
   - Rerun the full suite
   - If all tests pass, Claude must **stop and report back** for review
   - If tests fail three times consecutively, Claude must stop, report the errors, and wait for review before continuing

### Review Checkpoints

At every "stop" point, review and confirm:
- Tests and logic align with the original specification
- Code remains minimal and purpose-driven
- No new behaviour has been introduced unintentionally

Only after approval should Claude continue to the next step.

### Purpose

This controlled loop prevents uncontrolled code generation and ensures:
- Every feature is traceable to an explicit test
- No unnecessary code is introduced
- Each iteration ends in a clean, reviewed state

> **Summary:**  
> - Plan = define the TDD sequence in plan mode
> - Implement = follow it precisely  
> - Stop after **green tests** or **three failed attempts**, whichever comes first

---

## 7. Implementation Workflow

Once the plan has been reviewed and approved, you can switch to **default mode** or **auto-accept edits mode** to execute the plan.

### Choosing Your Mode

**Default mode:** Approve each change manually. Recommended for:
- Complex or unfamiliar features
- When learning Claude Code's behaviour
- High-risk changes

**Auto-accept edits mode:** Allow Claude to make changes directly. Recommended for:
- Well-defined, approved plans
- Bounded scope with clear tests
- When you can actively monitor the session

Both approaches are valid. The key is maintaining full awareness of what Claude is changing.

### Implementation Process

1. **Switch from plan mode** using Shift + Tab
2. **Confirm the plan** one final time before proceeding
3. **Create method stubs** with docstrings (Google-style) and inline comments
4. **Run existing tests** to confirm a clean baseline
5. **Implement the change** following the approved plan
6. **Monitor Claude's behaviour** — watch which files it touches and what commands it runs
7. **Rerun tests** — Claude can execute them and interpret results
8. **Interrupt if necessary** — Claude may attempt repeated test runs beyond the three-failure limit. Monitor its behaviour and interrupt if necessary using Esc

### Evaluation

- **If tests pass:** Verify the changes yourself before committing
- **If tests fail three times:** Claude should stop automatically and report errors for review

### Review and Commit

Once all tests pass:
- Verify the changes align with the plan
- Run the full test suite yourself to confirm (see Best Practices)
- Ensure Claude has updated the memory bank if requested
- Commit the changes

### Key Principles

- You are responsible for the session — Claude executes your instructions
- Keep the context focused; clear it if output grows noisy
- Require Claude to explain what it is changing and why before confirming further steps
- Success means **green tests with minimal code**, not large-scale edits

> **Goal:** Supervise Claude as a pair-programmer — it edits code, you stay in control.

---

## 8. Testing & Review

Claude can run your tests and inspect their output.

```bash
uv run pytest -v
```

During TDD:
1. Write or modify tests first
2. Run tests to confirm they fail (red)
3. Implement the feature until all tests pass (green)
4. Run full regression suite to confirm stability

Claude can read failure traces and debug interactively, which is particularly effective for complex logic.

> Always stop and review once all tests pass. Do not allow Claude to proceed automatically into unrelated refactors.

---

## 9. Memory Bank & Documentation

I maintain a `memory_bank/` folder in each project to track incremental feature development.

**Example structure:**
```
memory_bank/
  feature_000.md
  feature_001.md
  feature_002.md
  feature_003.md
```

### Protocol

This is a workflow pattern I've found effective:

1. Once a feature is complete, read the latest file (e.g. `feature_003.md`)
2. Create a new version (e.g. `feature_004.md`)
3. Summarise new capabilities and changes clearly
4. Avoid referencing previous files — each one should stand alone
5. Update the project `README.md` to reflect the new version

This approach ensures the project remains auditable and avoids context drift.

---

## 10. Best Practices

- Keep each task within a single context window
- Use `/clear` liberally to reset state
- Ask Claude to clarify before implementing
- Only use auto-accept edits mode when confident the scope is well-bounded
- Use comments to describe intent before code generation
- **Always review test outputs manually, even when Claude reports "all tests passed"** — Claude may run only a subset of tests and incorrectly report that all tests are passing. Verify by running the full test suite yourself.
- Maintain up-to-date memory bank and README files
- Prefer precision over verbosity — clarity beats completeness
- Let specialised tools handle their domains — use Ruff for linting, Prettier for formatting, not Claude

---

## 11. Common Issues

| Issue | Cause | Solution |
|-------|--------|-----------|
| **Context compaction** | Session too long | Use `/clear` and reload current plan |
| **Unexpected file edits** | Auto-accept edits mode enabled | Use Shift + Tab to cycle back to default mode |
| **Claude loses task focus** | Multiple unrelated instructions | Start a new plan with `/clear` |
| **Incomplete test runs** | Claude runs subset of tests | Always verify by running the complete test suite manually |
| **Repeated test failures** | Claude attempting too many fixes | Interrupt with Esc, review the issue, provide clearer guidance |

---

## 12. Further Reading

To deepen your understanding of Claude Code, I recommend these resources:

**[Claude Code Best Practices - Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices)**  
The official best practices from Anthropic's engineering team. Essential reading covering context management, CLAUDE.md optimisation, correction strategies, and MCP integration.

**[Basic Claude Code - Harper Reed](https://harper.blog/2025/05/08/basic-claude-code/)**  
Excellent perspective on TDD workflows with Claude Code. Harper's insight that "robots LOVE TDD" aligns perfectly with the approach described in this guide. Includes practical examples and prompt generation strategies.

**[Cooking with Claude Code: The Complete Guide - Sid Bharath](https://www.siddharthbharath.com/claude-code-the-complete-guide/)**  
Comprehensive hands-on tutorial building a complete application. Covers features, usage patterns, sub-agents, hooks, and MCP servers with practical examples throughout.

---

## 13. Summary

Claude Code is a powerful tool when guided with structure.  
By combining **TDD**, **controlled context**, **appropriate mode selection**, and **clear documentation**, you can safely accelerate feature delivery while maintaining code quality and traceability.

**Recommended workflow:**
1. Enter plan mode (Shift + Tab) and create a TDD plan
2. Review and approve the plan
3. Switch to default or auto-accept edits mode
4. Monitor implementation closely
5. Verify all tests pass (run them yourself)
6. Update documentation and memory bank
7. Commit changes

**Next steps:**
- Begin with a small feature in plan mode
- Keep sessions short, focused, and well-documented
- Experiment with different modes to find what works for your workflow
- Adapt these patterns to your own needs