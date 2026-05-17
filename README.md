# usefull-skills

A collection of Claude Code custom slash commands that act as specialized senior-level engineering assistants. Each command is a focused prompt that assumes a well-defined technical role, covers edge cases, and guides you through a structured workflow.

## Commands

### `/sprint-planner` — Sprint Planner & Functional Analyst

Translates business requirements into technical development plans.

**What it does:**
- Detects ambiguities and asks clarifying questions before planning
- Categorizes each requirement by type (`FEATURE`, `TECH DEBT`, `INFRA`, `SECURITY`, etc.) and priority (`P0`–`P3`)
- Converts functional requirements into technical user stories with acceptance criteria
- Produces a prioritized sprint backlog with story point estimates (Fibonacci) and dependency ordering
- Lists assumptions, risks, and explicit out-of-scope items to prevent scope creep

**Usage:**
```
/sprint-planner <functional requirements>
```

---

### `/node-security` — Node.js Security Auditor

Performs a full security audit of Node.js/npm projects.

**What it does:**
- Analyzes `package.json`, lock files, `.npmrc`, `.env`, Dockerfiles, and CI pipelines
- Detects unsafe version ranges (`^`, `~`, `*`, `latest`, `git+https://`) and classifies them by risk level
- Identifies vulnerable, deprecated, or unmaintained packages and suggests secure alternatives
- Audits lifecycle scripts (`postinstall`, `prepare`) for arbitrary code execution risk
- Checks for hardcoded secrets, missing `engines.node`, and missing `ignore-scripts` in CI
- Generates a prioritized report (`CRITICO` / `ALTO` / `MEDIO` / `BAJO`) with exact safe versions
- Asks if you want Claude to execute the remediation plan automatically (with backup of `package.json`)

**Usage:**
```
/node-security               # audits the current project
/node-security <path>        # audits a specific project path
```

---

### `/unit-test` — Unit Test Engineer

Creates and updates unit test suites following senior-level testing practices.

**What it does:**
- Detects the test framework automatically (`jest`, `vitest`, `mocha`, etc.) from `package.json`
- Analyzes the source module: exports, external dependencies to mock, side effects, internal state
- Presents a full test plan (happy paths, edge cases, error handling, side effect verification) before writing code
- Mocks 100% of `node_modules` dependencies — no real external calls in unit tests
- Follows `Arrange / Act / Assert` structure with descriptive test names
- Detects redundant tests and suggests `it.each()` consolidation when a function has 20+ test cases
- In update mode: compares source vs existing suite, flags outdated mocks, and reports potential bugs in source code **before** modifying anything

**Usage:**
```
/unit-test <path/to/source-file>          # create a new suite
/unit-test update <path/to/test-file>     # update an existing suite
```

---

## Installation

These commands are Claude Code global slash commands. To install them:

**Option A — Copy manually:**
```bash
# macOS / Linux
cp commands/*.md ~/.claude/commands/

# Windows (PowerShell)
Copy-Item commands\*.md "$env:USERPROFILE\.claude\commands\"
```

**Option B — Symlink (changes in this repo reflect immediately):**
```bash
# macOS / Linux
for f in commands/*.md; do
  ln -sf "$(pwd)/$f" ~/.claude/commands/"$(basename $f)"
done
```

After copying, the commands are available immediately in any Claude Code session as `/sprint-planner`, `/node-security`, and `/unit-test`.

## Requirements

- [Claude Code](https://claude.ai/code) CLI or desktop app

## Contributing

To add a new command, create a `.md` file in the `commands/` directory following this frontmatter structure:

```markdown
---
description: One-line description of when this command activates
argument-hint: Description of expected arguments
---

# Command Title
...
```

The `description` field is used by Claude to understand when to suggest the command. The `argument-hint` is shown in the autocomplete UI.
