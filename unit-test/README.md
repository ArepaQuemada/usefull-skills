# unit-test

**Role:** Software Engineer Senior — Unit Testing  
**Command:** `/unit-test`

Creates and updates unit test suites following senior-level testing practices. Mocks all external dependencies, covers happy paths and edge cases, detects slow or bloated suites, and never modifies source code without explicit authorization.

## Usage

```bash
# Create a new test suite for a source file
/unit-test src/services/authService.ts

# Update an existing test suite
/unit-test update src/__tests__/authService.test.ts
```

## Modes

### Create mode
Triggered when a source file path is provided. The command:
1. Reads the full source file
2. Maps all exports, external dependencies, side effects, and internal state
3. Presents a complete test plan for approval before writing any code
4. Writes the suite following `Arrange / Act / Assert` structure
5. Audits the suite for performance issues and redundancy before delivery

### Update mode
Triggered when the argument starts with `update` or `actualizar`. The command:
1. Reads both the existing test file and the source it tests
2. Compares current source against what the suite covers
3. Flags new functions without tests, outdated mocks, and tests referencing removed code
4. **If a potential bug is detected in the source, stops and notifies before continuing**

## Mocking rules

- **100% of `node_modules` imports are mocked** — no real external calls in unit tests, without exceptions
- Internal project dependencies are mocked if they perform I/O or side effects
- Internal pure utilities may be used as-is if they don't make the test fragile
- Mocks are reset in `beforeEach` to prevent cross-test contamination
- Module-level `jest.mock()` / `vi.mock()` is preferred over inline mocks

## Test naming convention

```
"[input condition] → [expected result]"
```

Examples:
- `"when the token is expired → should throw UnauthorizedError"`
- `"given an empty array → should return zero"`

## Suite quality checklist

Before delivery the command audits the suite for:

| Check | Threshold |
|-------|-----------|
| Test duration | Flag any test likely exceeding 200ms (signals unmocked I/O) |
| Real timers | Replace `setTimeout`/`setInterval` with `jest.useFakeTimers()` |
| Redundant tests | If 3+ tests share setup and vary only in input/output → consolidate with `it.each()` |
| Oversized suites | If a single function has 20+ tests → suggest splitting or consolidating |
| Branch coverage | Every `if/else`, `switch`, and ternary must have at least one test |
| Error paths | Every `catch`, `reject`, and `throw` must be covered |

## Generated file structure

```typescript
import { exportedFn } from './module'

jest.mock('external-package')
jest.mock('../internal-dependency')

import { dependency } from 'external-package'
const mockDependency = jest.mocked(dependency)

describe('ModuleName', () => {
  beforeEach(() => {
    jest.clearAllMocks()
  })

  describe('functionName', () => {
    describe('happy path', () => {
      it('valid input → expected output', async () => {
        // Arrange
        mockDependency.mockResolvedValue(expectedValue)
        // Act
        const result = await exportedFn(input)
        // Assert
        expect(result).toEqual(expectedValue)
        expect(mockDependency).toHaveBeenCalledWith(expectedInput)
      })
    })

    describe('edge cases', () => { /* ... */ })
    describe('error handling', () => { /* ... */ })
  })
})
```

## Supported frameworks

Detected automatically from `package.json` and config files:

- **Jest** (`jest.config.*`)
- **Vitest** (`vitest.config.*`)
- **Mocha** (`.mocharc.*`)
- **Jasmine**, **AVA**, **tap**

If no framework is detected, the command asks before proceeding.

## Installation

```bash
# macOS / Linux
cp unit-test.md ~/.claude/commands/

# Windows (PowerShell)
Copy-Item unit-test.md "$env:USERPROFILE\.claude\commands\"
```
