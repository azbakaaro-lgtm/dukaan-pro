# Dukaan Pro

Dukaan Pro is a multi-shop retail management platform for small-to-medium shops. It combines a fast point-of-sale workflow with inventory, flexible package conversions, customer credit, supplier purchasing, expenses, and owner reporting in one calm workspace.

## Product model

The platform has three roles. **SUPER_ADMIN** manages the platform and may view platform-level shop and user statistics plus read-only product data. **ADMIN** owns a shop and can manage its catalog, stock, purchases, suppliers, customers, debts, expenses, payroll, reports, and staff. **STAFF** can work the point of sale, search products, create customers, and record customer credit payments, but cannot access private expenses, supplier debt, purchase cost, profit, loss, or P&L.

Every shop-owned table includes a `shopId`, and server procedures scope reads and writes to the authenticated user’s shop. The frontend role views are only a convenience layer; unauthorized server procedures reject requests before data is returned.

## Technology

The project uses React 19, TypeScript, Tailwind CSS 4, Express, tRPC, Drizzle ORM, MySQL/TiDB, and JWT-backed HTTP-only sessions. Passwords are hashed with Node’s `scryptSync` using a per-password random salt. The existing Manus OAuth route remains available as an optional SSO path, while Dukaan Pro also supports credential login through `auth.login`.

## Local setup

Install dependencies with `pnpm install`, then configure a `.env` file using the environment provided by the hosting environment. The normal development command is `pnpm dev`. Run `pnpm check`, `pnpm test`, and `pnpm build` before a release.

Run the schema workflow with `pnpm drizzle-kit generate`, review the SQL under `drizzle/`, and apply it through the project database migration workflow. The current migration is `drizzle/0001_fine_ezekiel.sql`.

## Secure bootstrap

The initial platform account is always the following email, but its password is never stored in source code:

```text
jeemisafgooye@gmail.com
```

To create or reset the platform owner and the first shop admin, provide `DATABASE_URL`, `SUPER_ADMIN_PASSWORD`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, and `SHOP_NAME`, then run:

```bash
pnpm db:seed
```

Optional `ADMIN_NAME` sets the shop owner’s display name. The script creates the Super Admin with `super_admin` and the first shop owner with `admin` and associates the owner with the shop.

## Unit conversion model

Products store stock in a base unit. A package definition describes how many base units are inside one package. For example, Sugar can use `packageUnit = Bag`, `baseUnit = KG`, and `unitsPerPackage = 50`. Ten purchased bags become 500 base kilograms. A sale of 1 KG subtracts one base kilogram and the display becomes 9 bags plus 49 KG. Oomo can use pieces with 100 pieces per bag, while Pasta can use 10 KG per carton. The shared `unitEngine` contains deterministic conversion and breakdown tests for these cases.

## Financial controls

Sales create a customer debt record when `total - paid` is positive. Purchases are modeled as drafts until an ADMIN confirms them; confirmation is the point at which stock movements should be posted. Expenses support salary, electricity, water, rent, transport, purchase charges, and other categories, with flexible start and end periods for rent and utility records. Private financial reports are intentionally excluded from the STAFF workspace and the Super Admin platform statistics view.

## Files of interest

| Area | Location |
|---|---|
| Database schema | `drizzle/schema.ts` |
| Migration SQL | `drizzle/0001_fine_ezekiel.sql` |
| Database helpers | `server/db.ts` |
| tRPC procedures | `server/routers.ts` |
| Credential bootstrap | `server/seed.mjs` |
| Shared unit engine | `shared/unitEngine.ts` |
| Frontend workspace | `client/src/pages/Home.tsx` |
| Responsive role shell | `client/src/components/DashboardLayout.tsx` |
| Theme tokens | `client/src/index.css` |

## Validation

The unit engine tests are run with `pnpm test`. TypeScript is validated with `pnpm check`, and the production bundle is validated with `pnpm build`. Browser verification should cover credential login, the role-aware navigation, product search, low-stock rendering, and the authenticated dashboard against a seeded environment.

## Environment variables

The runtime provides `DATABASE_URL` for MySQL/TiDB, `JWT_SECRET` for session signing, `VITE_APP_ID`, `OAUTH_SERVER_URL`, and `VITE_OAUTH_PORTAL_URL` for optional workspace SSO, plus the built-in Forge and storage variables documented by the full-stack template. Credential bootstrap additionally requires `SUPER_ADMIN_PASSWORD`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, and `SHOP_NAME`; `ADMIN_NAME` is optional. Never commit real passwords or production secrets.

## Deployment

Use the project’s managed WebDev hosting flow after creating a checkpoint. For a conventional Node deployment, build with `pnpm build` and run `pnpm start` with the required environment variables and a reachable MySQL/TiDB database. Run the reviewed Drizzle migration before bootstrapping the first users. The service is designed as a single Node process and does not require a custom Dockerfile.
