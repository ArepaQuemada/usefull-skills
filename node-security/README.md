# node-security

**Role:** Security Engineer Senior — Node.js / npm ecosystem  
**Command:** `/node-security`

Performs a full security audit of Node.js projects. Analyzes `package.json`, lock files, configuration files, and CI pipelines to detect supply chain risks, vulnerable packages, unsafe version ranges, and hardening gaps. Optionally executes the remediation plan automatically.

## Usage

```bash
# Audit the current project (searches for package.json in the working directory)
/node-security

# Audit a specific project path
/node-security C:\projects\my-api
/node-security ../another-project
```

## Workflow

| Phase | What happens |
|-------|-------------|
| **1 — Context collection** | Reads `package.json`, lock file, `.npmrc`, `.env.*`, `Dockerfile`, and CI pipeline files. |
| **2 — Version range analysis** | Classifies every dependency's version specifier by risk level. |
| **3 — Vulnerable package audit** | Identifies deprecated, unmaintained, or compromised packages and suggests alternatives. |
| **4 — Configuration hardening** | Checks npm config, Node version pinning, lifecycle scripts, and secret exposure. |
| **5 — Vulnerability report** | Outputs a prioritized report with exact safe versions and remediation steps per finding. |
| **6 — Remediation commands** | Generates ready-to-run `npm install --save-exact` and `npm uninstall` commands. |
| **7 — Execution** | Asks if you want Claude to apply the fixes automatically. |

## Detected risk patterns

| Pattern | Risk |
|---------|------|
| `^x.y.z` | Allows automatic minor/patch updates — a compromised dependency can enter without an explicit change |
| `~x.y.z` | Allows patch updates — lower risk than `^` but still present |
| `*` or `latest` | Accepts any version — maximum risk |
| `>x.y.z` / `>=x.y.z` | No upper bound — allows untested major versions |
| No lock file | Non-deterministic dependency graph |
| `file:` / `link:` | Local dependency — may point to unaudited code |
| `git+https://` / `github:` | Direct repo dependency — may lack signed releases |

## Severity levels

| Level | Meaning |
|-------|---------|
| `CRITICO` | Requires immediate action before any deployment |
| `ALTO` | Must be resolved before the next release |
| `MEDIO` | Should be resolved in the short term |
| `BAJO` | Low exposure risk, address when convenient |

## Remediation execution modes

When vulnerabilities are found, the command offers four options:

1. **Execute all** — apply fixes for CRITICO, ALTO, and MEDIO
2. **Critical and high only** — apply only CRITICO and ALTO
3. **Choose manually** — confirm each fix individually
4. **Report only** — no files are modified

A `package.json.security-backup` is always created before any automatic modification.

## Example report entry

```
[SEV-001] Unsafe version range on express
  Severity: ALTO
  Category: Supply Chain
  Affects: express@^4.18.0
  Description: caret range allows automatic minor updates; a compromised
               minor release would enter the project without explicit change.
  Safe version: 4.21.2
  Remediation:
    1. npm install express@4.21.2 --save-exact
    2. Commit the updated package.json and package-lock.json
```

## Installation

```bash
# macOS / Linux
cp node-security.md ~/.claude/commands/

# Windows (PowerShell)
Copy-Item node-security.md "$env:USERPROFILE\.claude\commands\"
```
