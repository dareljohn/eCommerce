# docs/domain-glossary.md

# Domain Glossary

## Customer

A user who purchases products from the store.

## Administrator

A user with permissions to manage store operations.

## Product

An item offered for sale.

## Category

A classification used to organize products.

## Inventory

The quantity of a product available for sale.

## Cart

A customer's current collection of products intended for purchase.

## Cart Item

A product and quantity currently contained in a cart.

## Checkout

The process of converting a cart into a purchase.

## Order

A persistent record of a customer's purchase.

## Order Item

A specific product and quantity included in an order.

## Payment

The financial transaction associated with an order.

## Payment Status

The state of payment processing.

Example states may include:

```text
PENDING
PAID
FAILED
REFUNDED
```

## Order Status

The operational state of an order.

Potential states:

```text
PENDING
PROCESSING
SHIPPED
DELIVERED
CANCELLED
```

Exact states should be finalized when the order workflow is implemented.

## SKU

Stock Keeping Unit.

A unique identifier used to distinguish a sellable product or variant.

If product variants are introduced, SKU handling should be incorporated into the database design.

## Product Variant

A purchasable variation of a product.

Examples:

```text
T-Shirt
 ├── Small / Black
 ├── Medium / Black
 └── Large / Black
```

Variants are not part of the initial minimum database model unless required.

## Server Authoritative

Means the server/database determines the trusted value.

Example:

```text
Browser says:
price = $1

Server says:
actual price = $19.99

$19.99 is authoritative.
```

## Idempotency

The property that repeating an operation does not accidentally create duplicate effects.

This is especially important for payments and webhooks.
