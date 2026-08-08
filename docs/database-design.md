# Database Design

## Database

The application uses PostgreSQL as its primary relational database.

Prisma is used as the ORM and database access layer.

## Core Entities

The initial domain consists of:

```text
User
 |
 +---- Address
 |
 +---- Cart
 |       |
 |       +---- CartItem
 |
 +---- Order
         |
         +---- OrderItem

Product
 |
 +---- Category
 |
 +---- ProductImage
 |
 +---- Inventory
```

## User

Represents a customer or administrator.

Potential fields:

```text
User
- id
- email
- name
- role
- createdAt
- updatedAt
```

Roles should be represented using a controlled enum or equivalent mechanism.

Example:

```text
CUSTOMER
ADMIN
```

Authorization must always be enforced server-side.

## Address

Represents a customer's saved address.

Potential fields:

```text
Address
- id
- userId
- firstName
- lastName
- addressLine1
- addressLine2
- city
- state
- postalCode
- country
- phone
- createdAt
- updatedAt
```

Orders should preserve the relevant shipping information at the time of purchase rather than relying only on a user's current address.

## Category

Represents a product category.

Potential fields:

```text
Category
- id
- name
- slug
- description
- createdAt
- updatedAt
```

The `slug` should be suitable for URLs.

Example:

```text
electronics
```

## Product

Represents an item that can be sold.

Potential fields:

```text
Product
- id
- name
- slug
- description
- price
- status
- categoryId
- createdAt
- updatedAt
```

Possible product status values:

```text
DRAFT
ACTIVE
ARCHIVED
```

Prices must be controlled by the server.

Do not trust prices sent by the client.

## ProductImage

Represents images associated with a product.

Potential fields:

```text
ProductImage
- id
- productId
- url
- alt
- sortOrder
- createdAt
```

The application should support multiple images per product.

## Inventory

Represents product stock.

Potential fields:

```text
Inventory
- id
- productId
- quantity
- updatedAt
```

Inventory changes must happen server-side.

Inventory-sensitive operations should use transactions or appropriate concurrency protection.

## Cart

Represents a user's active shopping cart.

Potential fields:

```text
Cart
- id
- userId
- createdAt
- updatedAt
```

Depending on the application's requirements, guest carts may later be supported.

## CartItem

Represents a product in a cart.

Potential fields:

```text
CartItem
- id
- cartId
- productId
- quantity
- createdAt
- updatedAt
```

The database should prevent duplicate product entries within the same cart when appropriate.

## Order

Represents a completed or in-progress purchase.

Potential fields:

```text
Order
- id
- userId
- status
- subtotal
- shippingAmount
- taxAmount
- discountAmount
- total
- currency
- paymentStatus
- shipping information
- createdAt
- updatedAt
```

Order totals should be stored because prices and product information can change after an order is placed.

## OrderItem

Represents a product purchased in an order.

Potential fields:

```text
OrderItem
- id
- orderId
- productId
- productName
- quantity
- unitPrice
- total
```

Important:

An order item should preserve the product name and price applicable when the order was created.

Do not depend entirely on the current `Product` record for historical order information.

## Relationships

Conceptually:

```text
User
 |
 +----< Address
 |
 +----1 Cart
 |       |
 |       +----< CartItem >---- Product
 |
 +----< Order
         |
         +----< OrderItem >---- Product

Category
 |
 +----< Product
          |
          +----< ProductImage
          |
          +---- Inventory
```

## Monetary Values

Money must not be represented using JavaScript floating-point arithmetic for authoritative financial calculations.

Use an appropriate database representation, such as integer minor units or a database decimal type.

For example:

```text
$19.99
```

could be represented as:

```text
1999 cents
```

The exact approach should be applied consistently throughout the application.

## Database Rules

1. Use foreign keys for relationships.
2. Add appropriate indexes for frequently queried fields.
3. Use unique constraints where business rules require uniqueness.
4. Use transactions for multi-step operations that must remain consistent.
5. Never expose database credentials to the client.
6. Never modify production data manually without a clear reason and migration/operational procedure.
7. Use Prisma migrations to track schema changes.
8. Do not delete historical order information merely because a product is archived.

## Schema Changes

When changing the database:

1. Update `schema.prisma`.
2. Create a migration.
3. Review the generated migration.
4. Update affected application code.
5. Test existing functionality.
6. Update this document if the domain model changes significantly.

## Future Entities

Potential future entities include:

```text
Coupon
Review
Wishlist
ProductVariant
Brand
Shipment
Refund
Payment
AuditLog
```

Do not implement these until the application actually requires them.
