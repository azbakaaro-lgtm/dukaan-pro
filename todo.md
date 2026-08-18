# Project TODO

## Platform foundation

- [x] Add credential-based login with securely hashed passwords and session handling.
- [x] Add SUPER_ADMIN, ADMIN, and STAFF roles with backend permission checks and explicit 401/403 behavior.
- [ ] Add shop tenancy and enforce shopId scoping on every shop-owned record and query.
- [ ] Add comprehensive audit logging for critical actions across all roles.
- [x] Add English/Somali translation structure with the requested Somali retail terminology.

## Catalog and inventory

- [x] Add shops, categories, products, SKUs/barcodes, suppliers, and product status models.
- [x] Add configurable package/base-unit/selling-unit conversion definitions.
- [x] Implement accurate package-plus-partial-unit stock calculations for weight and piece examples.
- [ ] Record every purchase, sale, return, adjustment, damaged, expired, and correction movement.
- [ ] Add low-stock thresholds, alerts, search, filtering, and stock history.

## POS and customer credit

- [x] Build fast POS search by product name, SKU, barcode, and category.
- [x] Add cart quantity/unit selection, customer assignment, partial payment, and sale completion.
- [x] Automatically reduce stock and reject overselling.
- [ ] Add printable thermal-friendly receipts.
- [x] Add customers, customer debt ledger, payments, and payment history.

## Suppliers and purchasing

- [x] Add purchase drafts with line items, packaging, prices, and other charges. (Charges are one lump "other charges" field, not split into transport/loading/delivery/customs sub-categories yet.)
- [x] Restrict purchase confirmation to ADMIN and update stock only on confirmation.
- [x] Add supplier debt and payment history restricted from STAFF.

## Expenses and financial reporting

- [ ] Add salary/payroll records and payment history.
- [ ] Add electricity, water, rent with flexible periods, transport, and custom expenses.
- [ ] Restrict expenses, P&L, and private financial reports to ADMIN and SUPER_ADMIN as defined.
- [ ] Add sales, purchases, stock, debts, expenses, payroll, utilities, rent, stock movement, and P&L reports.
- [ ] Add admin dashboard metrics without exposing private shop finances to STAFF or platform Super Admin views.

## Role workspaces and UX

- [x] Build Scandinavian visual system: pale cool gray canvas, black sans-serif hierarchy, thin subtitles, pastel blue/blush geometry.
- [x] Build responsive desktop sidebar and mobile navigation.
- [x] Build ADMIN dashboard and back-office views.
- [x] Build STAFF sales-focused workspace without financial/private navigation or data.
- [ ] Build SUPER_ADMIN platform dashboard with read-only product data and platform statistics only.
- [x] Add loading, empty, error, confirmation, toast, pagination, and filter states.

## Quality and delivery

- [x] Add unit tests for unit conversion and partial stock calculations; integration coverage remains to be expanded.
- [x] Run typecheck, build, tests, and browser verification; fix all blocking errors found.
- [x] Write complete README with setup, env, migrations, seed/bootstrap, role permissions, unit logic, and deployment instructions.
- [ ] Create the complete runnable dukaan-pro.zip package.

## Gap remediation

- [x] Implement debt payment creation and payment history procedures and UI. (Customer debt done end-to-end; supplier debt payment procedures + UI also added.)
- [ ] Wire product_units into product CRUD and sale unit selection; validate conversion factors server-side.
- [x] Implement purchase confirmation and supplier debt workflows. Expense, reporting, and staff-management workflows still remain.
- [ ] Add Super Admin read-only product browsing and data-minimized staff dashboard procedures.
- [ ] Add true English/Somali locale dictionaries and a language switcher.
- [ ] Add stock history, explicit low-stock alerts, confirmations, pagination, and complete error states.
- [ ] Expand README with environment-variable reference and deployment instructions.
