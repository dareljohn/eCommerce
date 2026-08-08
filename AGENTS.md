# AGENTS.md

## Project

This repository contains an e-commerce web application.

The application is being developed as a production-oriented, maintainable, secure, and scalable web application.

Detailed project documentation is located in the `docs/` directory.

## Required Reading

Before making significant changes, AI agents should read:

1. `docs/README.md`
2. `docs/ai-agent-guide.md`
3. `docs/project-context.md`
4. `docs/architecture.md`
5. `docs/technology-stack.md`
6. `docs/coding-standards.md`

For domain-specific work, also read the relevant documentation:

* Database work → `docs/database-design.md`
* API/server work → `docs/api-conventions.md`
* Security/authentication → `docs/security-guidelines.md`
* UI work → `docs/ui-ux-guidelines.md`
* Testing → `docs/testing-strategy.md`
* Environment/configuration → `docs/environment-configuration.md`
* Planning/features → `docs/roadmap.md`
* Architectural decisions → `docs/architectural-decisions.md`

## Core Rule

Do not make architectural assumptions when the repository documentation already defines the decision.

Existing code and documented project decisions take precedence over generic recommendations.

## Development Principles

AI agents must:

1. Inspect the existing code before modifying it.
2. Understand the surrounding architecture before introducing new code.
3. Prefer existing patterns over creating new patterns.
4. Make the smallest change that correctly solves the requested problem.
5. Avoid unrelated refactoring.
6. Avoid unnecessary dependencies.
7. Use strict TypeScript.
8. Validate all external input.
9. Keep secrets server-side.
10. Enforce authentication and authorization server-side.
11. Never trust client-provided prices, totals, inventory, roles, or payment status.
12. Preserve existing functionality unless the requested change explicitly requires otherwise.

## Before Coding

The agent should:

1. Read relevant documentation.
2. Inspect the repository structure.
3. Search for similar existing functionality.
4. Identify affected files.
5. Identify affected database models.
6. Identify affected server/client boundaries.
7. Consider security implications.
8. Consider loading, error, and empty states.
9. Consider mobile and accessibility requirements.
10. Make a concise implementation plan.

## During Coding

The agent should:

* Follow established naming conventions.
* Reuse existing components and utilities.
* Avoid unnecessary abstractions.
* Keep components focused.
* Keep business logic out of presentation components.
* Keep database access on the server.
* Avoid exposing server-only modules to client components.
* Avoid introducing global state unless necessary.
* Use typed interfaces and schemas.
* Handle expected failures explicitly.

## After Coding

The agent should:

1. Run formatting checks where available.
2. Run ESLint.
3. Run TypeScript checks.
4. Run relevant tests.
5. Review the Git diff.
6. Check for accidental changes.
7. Verify environment variables were not exposed.
8. Verify authorization.
9. Verify error and loading states.
10. Update documentation if architecture, domain models, APIs, or roadmap status changed.

## Database Safety

Never make destructive database changes without explicit justification.

Never:

* Drop production tables casually.
* Delete production data during development instructions.
* Commit database credentials.
* Hard-code database credentials.
* Trust database IDs or ownership supplied by the browser without verification.

Database schema changes must be represented through Prisma migrations.

## Git Safety

Never:

* Force-push without explicit instruction.
* Rewrite history unnecessarily.
* Commit secrets.
* Commit `.env` files containing real credentials.
* Delete unrelated branches/files.
* Modify unrelated code merely to make a task easier.

Prefer focused commits.

## Security

Security-sensitive code requires extra scrutiny.

Always consider:

* Authentication
* Authorization
* Input validation
* SQL/database safety
* XSS
* CSRF where applicable
* Rate limiting
* Session security
* Secret management
* Payment verification
* Webhook signature verification
* Sensitive logging

## E-commerce Rules

The server is authoritative for:

* Product price
* Product availability
* Inventory
* Discounts
* Taxes
* Shipping calculations
* Order totals
* Payment state
* User permissions

The client is never authoritative for these values.

## Documentation

When making a significant decision, update the appropriate documentation.

Examples:

Architecture change:

```text
docs/architecture.md
docs/architectural-decisions.md
```

Database change:

```text
docs/database-design.md
```

API change:

```text
docs/api-conventions.md
```

Feature progress:

```text
docs/roadmap.md
```

Technology change:

```text
docs/technology-stack.md
```

## Final Response

When completing a coding task, report:

1. What was changed.
2. Which files were changed.
3. Important implementation decisions.
4. Validation/tests performed.
5. Any known limitations.
6. Any follow-up work that should be added to the roadmap.

Do not claim that a test, build, deployment, migration, or command succeeded unless it was actually verified.
