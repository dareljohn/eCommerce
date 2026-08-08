# docs/environment-configuration.md

# Environment Configuration

## Purpose

This document describes application configuration that differs between environments.

## Environment Files

Expected local development files may include:

```text
.env
.env.local
.env.example
```

The exact Next.js environment-file strategy should follow the framework's conventions.

## Secrets

Real secrets must never be committed.

Examples:

```text
DATABASE_URL
AUTH_SECRET
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
EMAIL_API_KEY
```

## Example File

`.env.example` should contain variable names without real credentials.

Example:

```text
DATABASE_URL=

AUTH_SECRET=

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=

EMAIL_API_KEY=
```

## Client Exposure

Only variables intentionally designed for browser exposure should use the appropriate public environment-variable convention.

Never expose:

* Database credentials
* Private API keys
* Authentication secrets
* Stripe secret keys
* Webhook secrets

## Local Development

Local development should use isolated development resources.

Do not point local development at production databases unless explicitly required and safely controlled.

## Database

The application should eventually use:

```text
DATABASE_URL
```

for Prisma/PostgreSQL connection configuration.

## Third-Party Services

External integrations should be configured through environment variables.

Do not hard-code credentials.

## Configuration Validation

As the application grows, required environment variables should be validated at application startup or integration boundaries.

Missing required configuration should produce a clear developer-facing error.

## Production

Production configuration must be managed by the deployment platform's secure environment-variable system.

Do not commit production credentials to Git.