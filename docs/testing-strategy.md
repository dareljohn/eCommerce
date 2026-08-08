# docs/testing-strategy.md

# Testing Strategy

## Testing Goals

Tests should protect business rules and critical user flows.

Priority should be given to:

1. Payment correctness
2. Order creation
3. Inventory correctness
4. Authentication/authorization
5. Cart behavior
6. Product functionality
7. UI behavior

## Unit Tests

Use unit tests for isolated business logic.

Examples:

* Price calculations
* Quantity validation
* Discount calculations
* Inventory rules
* Order total calculations

## Integration Tests

Use integration tests for interactions between:

* Application logic
* Database
* Authentication
* External integrations where practical

## End-to-End Tests

Critical flows should eventually have end-to-end tests.

Primary flow:

```text
Browse
 ↓
Product
 ↓
Add to cart
 ↓
Checkout
 ↓
Payment
 ↓
Order confirmation
```

## Test Data

Test data should be deterministic.

Do not rely on production data.

## Database Tests

Use an isolated test database or appropriate test database strategy.

Never run destructive tests against production.

## Regression Testing

When fixing a bug:

1. Reproduce it.
2. Add a regression test where practical.
3. Implement the fix.
4. Verify the regression test passes.

## Test Naming

Tests should describe behavior.

Good:

```text
calculates order total using server-side product prices
```

Bad:

```text
test1
```

## Definition of Test Completion

A feature requiring tests is not complete until relevant tests pass.
