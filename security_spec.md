# Security Specification - Medical Store Management

## Data Invariants
1. Products must have positive sale price and non-negative stock quantity.
2. Sales invoices must contain at least 1 item and positive total, with valid date.
3. Purchases must record quantity > 0 and purchase price >= 0.
4. Stock history must accurately track previous and resulting stock.
5. All operations must require authenticated store operator access.

## The Dirty Dozen Payloads (Rejection Targets)
1. Product with negative salePrice (-15.00)
2. Product missing required batchNumber
3. Product with oversized name (>150 chars)
4. Sale with negative remaining or invalid invoice number
5. Sale with unknown malicious fields injected (privilege escalation)
6. Purchase with negative quantity
7. Customer with empty name or corrupt payload
8. Supplier with oversized company string (>150 chars)
9. Expense with negative amount or missing date
10. Unauthenticated write attempt to /products
11. Unauthenticated write attempt to /sales
12. Arbitrary collection write attempt to /{document=**}

## Rule Strategy
- Strict schema adherence matching `firebase-blueprint.json`
- Default-deny catch-all rule
- Verified authenticated user access for pharmacy operators
- Hardened path variable validation
