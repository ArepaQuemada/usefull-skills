# usefull-skills

A collection of Claude Code custom slash commands that embed senior-level engineering roles directly into your workflow. Each command is a self-contained prompt that assumes a well-defined technical persona, runs a structured multi-phase process, and guides you to a concrete, actionable output.

## Commands

| Command | Role | What it produces |
|---------|------|-----------------|
| [`/sprint-planner`](./sprint-planner/) | Sprint Planner & Functional Analyst | Prioritized technical backlog from business requirements |
| [`/node-security`](./node-security/) | Security Engineer — Node.js/npm | Vulnerability report with safe versions and optional auto-remediation |
| [`/unit-test`](./unit-test/) | Software Engineer — Testing | Full unit test suite with mocks, happy paths, and edge cases |

## Installation

Copy the command files you want into your global Claude Code commands directory:

```bash
# macOS / Linux — install all
cp sprint-planner/sprint-planner.md ~/.claude/commands/
cp node-security/node-security.md   ~/.claude/commands/
cp unit-test/unit-test.md           ~/.claude/commands/

# Windows (PowerShell) — install all
Copy-Item sprint-planner\sprint-planner.md "$env:USERPROFILE\.claude\commands\"
Copy-Item node-security\node-security.md   "$env:USERPROFILE\.claude\commands\"
Copy-Item unit-test\unit-test.md           "$env:USERPROFILE\.claude\commands\"
```

Commands are available immediately in any Claude Code session after copying. No restart required.

## Requirements

- [Claude Code](https://claude.ai/code) CLI or desktop app

## Project structure

```
usefull-skills/
├── sprint-planner/
│   ├── sprint-planner.md   ← command file
│   └── README.md           ← detailed documentation
├── node-security/
│   ├── node-security.md
│   └── README.md
└── unit-test/
    ├── unit-test.md
    └── README.md
```

## Contributing

To add a new command, create a folder with the command name and place two files inside:

```
my-command/
├── my-command.md    ← the actual Claude Code command
└── README.md        ← documentation
```

The command file must include this frontmatter:

```markdown
---
description: One-line description of when this command should be used
argument-hint: Description of the expected argument
---
```

The `description` field drives Claude's understanding of when to suggest the command. The `argument-hint` is shown in the autocomplete UI when the user types the slash command.
