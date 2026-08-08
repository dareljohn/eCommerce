# docs/ui-ux-guidelines.md

# UI/UX Guidelines

## Design Goals

The storefront should feel:

* Clear
* Trustworthy
* Fast
* Responsive
* Accessible
* Consistent

## Responsive Design

Design mobile-first.

Consider:

```text
Mobile
Tablet
Desktop
Large desktop
```

Do not assume desktop width.

## Components

Prefer reusable components for repeated UI patterns.

Potential groups:

```text
components/ui/
components/layout/
components/products/
components/cart/
components/checkout/
components/account/
components/admin/
```

## States

Every asynchronous feature should consider:

```text
Loading
Success
Empty
Error
```

## Forms

Forms should have:

* Labels
* Appropriate input types
* Validation
* Useful error messages
* Keyboard support
* Loading/submission state

## Buttons

Buttons should communicate the action clearly.

Examples:

```text
Add to cart
Buy now
Continue to checkout
Place order
Save address
Cancel
```

Avoid vague labels where possible.

## Product Pages

A product page should eventually communicate:

* Product name
* Images
* Price
* Description
* Availability
* Quantity
* Add-to-cart action
* Relevant product information

## Cart

The cart should clearly display:

* Products
* Quantities
* Prices
* Subtotal
* Applicable shipping/tax information
* Total
* Checkout action

## Accessibility

Use:

* Semantic HTML
* Proper labels
* Keyboard navigation
* Visible focus
* Alt text
* Accessible dialogs
* Meaningful error messages

Do not rely on color alone to communicate state.

## Loading

Avoid layout shifts where practical.

Use skeletons or meaningful loading indicators when appropriate.

## Error Messages

Errors should tell users:

1. What went wrong.
2. What they can do next.

Avoid technical errors.

Bad:

```text
PrismaClientKnownRequestError
```

Better:

```text
We couldn't update your cart. Please try again.
```