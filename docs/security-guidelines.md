# docs/security-guidelines.md

# Security Guidelines

## Principle

Security is a server-side responsibility.

Never assume the browser is trusted.

## Secrets

Never commit:

* Passwords
* API keys
* Database credentials
* Authentication secrets
* Payment secrets

Use environment variables.

## Authentication

Authentication must be handled through an established, well-maintained solution.

Do not implement custom password hashing/session infrastructure unless explicitly required and reviewed.

## Authorization

Every sensitive operation must verify authorization.

Examples:

* Admin product management
* Inventory changes
* Order management
* User management
* Refunds
* Private customer data

## Input Validation

Validate all external input.

Never rely solely on:

* HTML validation
* Client-side validation
* TypeScript types

Client-side validation improves UX.

Server-side validation provides security.

## XSS

Never render untrusted HTML without appropriate sanitization and justification.

Prefer normal React escaping.

## SQL/Database Security

Use Prisma's parameterized operations.

Never construct SQL by concatenating untrusted input.

If raw SQL is required, review it carefully.

## Authentication Cookies

Authentication/session cookies should use appropriate:

* Secure
* HttpOnly
* SameSite

settings where applicable.

## Payments

Never store raw payment-card credentials.

Never mark an order as paid based solely on browser state.

Verify payment events through the payment provider.

## Webhooks

Verify webhook signatures.

Design webhook processing to tolerate retries.

## Logging

Never log:

* Passwords
* Secrets
* API keys
* Payment credentials
* Session tokens

## Authorization Failures

Do not reveal unnecessary information.

For example, avoid telling an unauthorized user details about another customer's order.

## Dependencies

Keep dependencies updated.

Review dependencies for:

* Maintenance
* Security
* Necessity
* Bundle impact
