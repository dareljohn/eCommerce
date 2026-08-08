# docs/development-workflow.md

# Development Workflow

## Feature Workflow

Use:

```text
Requirement
    ↓
Understand context
    ↓
Inspect repository
    ↓
Plan
    ↓
Implement
    ↓
Validate
    ↓
Test
    ↓
Review diff
    ↓
Document
    ↓
Commit
```

## Step 1 — Requirement

Clearly identify:

* Desired behavior
* User affected
* Data affected
* Expected outcome
* Constraints

## Step 2 — Repository Inspection

Before coding:

```text
Read documentation
Search existing implementation
Inspect relevant files
Inspect database schema
Inspect tests
```

## Step 3 — Plan

For non-trivial features, identify:

* Files to change
* New files
* Database changes
* API changes
* UI changes
* Tests
* Documentation

## Step 4 — Implementation

Make focused changes.

Avoid unrelated refactoring.

## Step 5 — Validation

Run appropriate:

```text
TypeScript
Lint
Tests
Build
```

## Step 6 — Diff Review

Review:

```bash
git diff
```

Look for:

* Accidental changes
* Debugging code
* Secrets
* Unused imports
* Incorrect files
* Unexpected dependency changes

## Step 7 — Documentation

Update documentation when:

* Architecture changes
* Database changes
* API behavior changes
* Technology decisions change
* Roadmap status changes

## Step 8 — Commit

Use focused commit messages.

Examples:

```text
feat: add product catalog
fix: validate cart inventory
refactor: extract product card
docs: document checkout flow
test: add order total tests
```

## Pull Requests

A useful pull request should explain:

* What changed
* Why it changed
* How it was tested
* Any migration requirements
* Any known limitations

---
