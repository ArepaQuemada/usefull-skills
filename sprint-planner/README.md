# sprint-planner

**Role:** Sprint Planner & Functional Analyst Senior  
**Command:** `/sprint-planner`

Translates business requirements written in natural language into a structured technical development plan with categorization, estimates, and explicit scope boundaries.

## Usage

```
/sprint-planner <functional requirements>
```

You can pass requirements inline or as a multi-line block:

```
/sprint-planner
  - Users must be able to register with email and password
  - Users must see their purchase history
  - Send email notifications when an order changes status
```

## Workflow

The command runs five sequential phases:

| Phase | What happens |
|-------|-------------|
| **1 — Requirement Analysis** | Decomposes each requirement, flags ambiguities, and asks clarifying questions before proceeding. Also confirms the tech stack if not specified. |
| **2 — Categorization** | Classifies every requirement by type and priority, then outputs a table. |
| **3 — Technical Translation** | Converts each item into a technical user story with affected layers and verifiable acceptance criteria. |
| **4 — Sprint Plan** | Produces a prioritized backlog with story points, dependency ordering, risks, assumptions, and explicit out-of-scope items. |
| **5 — Validation** | Summarizes total estimated capacity and asks for scope adjustments before closing. |

## Requirement types

| Label | Meaning |
|-------|---------|
| `FEATURE` | New product functionality |
| `ENHANCEMENT` | Improvement to existing functionality |
| `BUG FIX` | Correction of incorrect behavior |
| `TECH DEBT` | Refactor or optimization |
| `INFRA` | Infrastructure, CI/CD, environments |
| `SECURITY` | Security or compliance requirements |
| `UX` | Interface or user experience changes |

## Priority levels

| Label | Meaning |
|-------|---------|
| `P0` | Blocker — critical for the business |
| `P1` | High priority, must be in the sprint |
| `P2` | Medium priority, ideal to include |
| `P3` | Low priority, can be deferred |

## Output format

Each sprint task is documented as:

```
[ID] Task title
  Type: FEATURE / BUG FIX / ...
  Technical description: ...
  Layers: Backend, Frontend, DB
  Acceptance criteria:
    - [ ] criterion 1
    - [ ] criterion 2
  Estimate: X story points
  Depends on: [ID] (if applicable)
```

Estimates use the Fibonacci scale: **1, 2, 3, 5, 8, 13**.

## Installation

```bash
# macOS / Linux
cp sprint-planner.md ~/.claude/commands/

# Windows (PowerShell)
Copy-Item sprint-planner.md "$env:USERPROFILE\.claude\commands\"
```
