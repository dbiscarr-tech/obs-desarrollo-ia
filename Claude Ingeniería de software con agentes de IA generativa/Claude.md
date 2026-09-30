```
# CLAUDE.md

## Project

Aplicación para registrar, categorizar, consultar y analizar gastos personales.

Prioridades del proyecto:

1. Integridad de los datos.
2. Seguridad y privacidad.
3. Corrección funcional.
4. Mantenibilidad.
5. Simplicidad.
6. UX y performance.
    

---

## Core Principles

- Apply **SOLID** and **KISS**.
- Prefer simple, explicit and maintainable solutions.
- Do not over-engineer.
- Reuse existing functionality before creating new abstractions.
- Keep business logic separated from UI and infrastructure.
- Minimize the scope of each change.
 Do not refactor unrelated code while implementing a feature.
- Never guess business rules; inspect the repository or ask.
- Preserve existing behavior unless the task explicitly requires changing it.
    

---

## Before Changing Code

Always:

1. Inspect the relevant project structure.   
2. Search for existing implementations.   
3. Identify consumers and dependencies.  
4. Review relevant tests.   
5. Make the smallest appropriate change.
    

Do not create a new function, component, service or dependency if an existing one can reasonably be reused.

---

## Architecture

Maintain clear separation between:

```text
UI
 ↓
Application / Use Cases
 ↓
Business Logic / Domain
 ↓
Data Access
 ↓
Database / External Services
```

Business rules should not be embedded unnecessarily in UI components.

Avoid introducing new architectural patterns, frameworks or infrastructure unless there is a concrete need.

---

## Financial Data

Financial data requires special care.

### Monetary values

- Never use floating-point arithmetic for monetary calculations.
    
- Use the project's appropriate decimal/integer representation.
    
- Always preserve the currency explicitly.
    
- Never silently convert currencies.
    

### Dates

Keep these concepts separate:

- transaction date;
    
- created timestamp;
    
- updated timestamp.
    

Use `America/Santiago` as the default timezone unless the project explicitly defines another one.

### Data integrity

- Never silently delete financial records.
    
- Never silently overwrite historical transactions.
    
- Prefer explicit operations for destructive changes.
    
- Preserve traceability where appropriate.
    

---

## Expense Data

An expense should conceptually distinguish:

- amount;
    
- currency;
    
- transaction date;
    
- category;
    
- payment method/account;
    
- description;
    
- status;
    
- identifiers and timestamps.
    

Do not confuse:

```text
Category ≠ Account ≠ Payment Method
```

Follow the existing domain model if it already defines these concepts differently.

---

## Imports

For CSV, Excel, bank or external-data imports:

- Validate structure and data types.
    
- Validate dates and amounts.
    
- Detect duplicates whenever possible.
    
- Do not silently discard invalid records.
    
- Report import errors clearly.
    
- Do not delete existing records as a side effect of an import unless explicitly required.
    
- Prefer idempotent imports.
    

Real financial files must never be added to the repository.

---

## Security & Privacy

Treat financial information as sensitive.

Never commit:

- passwords;
    
- API keys;
    
- tokens;
    
- credentials;
    
- private keys;
    
- real financial data.
    

Use environment variables or the project's secret-management mechanism.

Do not expose sensitive financial information in logs or error messages.

Use synthetic data for tests and development whenever possible.

---

## Dependencies

Before adding a dependency:

1. Check whether the functionality already exists.
    
2. Prefer standard/project-native solutions when reasonable.
    
3. Consider security, maintenance and complexity.
    
4. Add the dependency only when it provides meaningful value.
    

Do not add dependencies for trivial functionality.

---

## Testing

Every meaningful change should be verified with the project's available:

- unit tests;
    
- integration tests;
    
- linting;
    
- type checking;
    
- build.
    

Financial logic should have tests covering relevant edge cases such as:

- zero values;
    
- invalid values;
    
- large amounts;
    
- date boundaries;
    
- month/year changes;
    
- duplicate transactions;
    
- invalid imports.
    

Do not reduce test coverage simply to make a change pass.

---

## Error Handling

Errors should be:

- explicit;
    
- useful;
    
- actionable;
    
- safe.
    

Never expose secrets, credentials or unnecessary internal information.

Validate external input at the appropriate application boundary.

Frontend validation does not replace backend/domain validation.

---

## Git

Keep commits focused and logically coherent.

Prefer conventional messages such as:

```text
feat: add expense import
fix: prevent duplicate expenses
refactor: simplify expense calculation
test: add monthly expense tests
docs: update import workflow
```

Do not mix unrelated:

- features;
    
- refactoring;
    
- formatting;
    
- dependency upgrades.
    

Always review:

```bash
git status
git diff
```

before considering a task complete.

---

## Development Workflow

For a non-trivial task:

### 1. Understand

Inspect the repository and relevant code.

### 2. Plan

Identify:

- problem;
    
- affected components;
    
- intended solution;
    
- potential risks.
    

### 3. Implement

Make the smallest appropriate change.

### 4. Verify

Run the relevant tests, lint, type checks and build.

### 5. Review

Inspect the final diff and check for unintended changes.

### 6. Report

Summarize:

- what changed;
    
- files/components affected;
    
- verification performed;
    
- relevant limitations or follow-up items.
    

---

## High-Risk Changes

Treat these as high risk:

- database schema changes;
    
- migrations;
    
- deletion of data;
    
- financial calculations;
    
- authentication/authorization;
    
- imports;
    
- API contract changes;
    
- changes affecting historical financial data.
    

For high-risk changes, inspect all relevant consumers and tests before implementation.

Never perform destructive operations based on assumptions.

---

## AI / Automation

If AI is used for categorization, classification, anomaly detection or recommendations:

- AI output must be treated as a suggestion, not authoritative data.
    
- Users must be able to correct AI-generated classifications.
    
- AI must not silently modify historical financial data.
    
- Important automated decisions should remain traceable.
    

---

## Documentation

Document decisions that are important for understanding or maintaining the system.

Do not document obvious code.

When a feature introduces a significant business rule, architectural decision or non-obvious constraint, update the appropriate documentation.

---

## Do Not Invent

Never invent:

- APIs;
    
- database tables or fields;
    
- business rules;
    
- dependencies;
    
- configuration;
    
- environment variables;
    
- existing functionality.
    

When uncertain:

1. Search the repository.
    
2. Check existing documentation/tests.
    
3. Infer only when the evidence is clear.
    
4. Ask before making a decision that could affect data, architecture or security.
    

---

## Definition of Done

A task is complete when:

- The requested functionality works.
    
- Existing relevant behavior remains intact.
    
- Relevant tests pass.
    
- Lint/type checking/build pass when available.
    
- No secrets or sensitive data were introduced.
    
- No unrelated files were modified.
    
- The final `git diff` has been reviewed.
    
- The implementation is consistent with the project's existing architecture.
    

---

## Final Rule

**Understand first. Change minimally. Verify everything.**

Optimize for correctness, data integrity, security and maintainability—not for the amount of code produced.
```