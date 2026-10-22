# Social Services Dashboard

A Next.js + TypeScript starter for a social media services storefront and reseller dashboard.

## Safety defaults

- `SUPPLIER_MODE=mock`
- `SUPPLIER_LIVE_ENABLED=false`
- No live supplier calls are implemented in the mock adapter.
- No supplier API key is required.
- Payment flow is manual/demo; no real card data is collected.
- Service seed data is illustrative and uses only content-management services.
- No account-selling, OTP resale, or login-code features are included.

The official SocialPlatforms documentation describes a single endpoint:
`POST https://socialplatforms.org/api/reseller/api-order` with a Bearer token and actions such as `boost.services`, `boost.order`, and `boost.status`. This project intentionally does not call that endpoint. Live integration requires separate supplier verification, compliance review, testing, and an explicit implementation change.

## Stack

- Next.js App Router / TypeScript
- Prisma ORM / PostgreSQL
- Password hashing with bcrypt
- Signed HTTP-only session cookie using `jose`
- Mock supplier adapter

## Local setup

Requirements: Node.js 20+ recommended (Node.js 18+ can run some parts but use a supported Next.js runtime), npm, PostgreSQL.

1. Extract this ZIP and open a terminal in the project directory.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Copy `.env.example` to `.env` and set `DATABASE_URL` and a long random `AUTH_SECRET`.
4. Generate Prisma client and create tables:
   ```bash
   npm run db:generate
   npm run db:push
   npm run db:seed
   ```
5. Start:
   ```bash
   npm run dev
   ```
6. Open `http://localhost:3000`.

## Create an administrator

Register an account using the site. Then promote that account manually in PostgreSQL after confirming its email:

```sql
UPDATE "User" SET role = 'ADMIN' WHERE email = 'your-verified-email@example.com';
```

Do not expose the admin role in a public registration form. In production, use a controlled migration or one-time admin bootstrap process instead of direct ad hoc SQL.

## Main routes

- `/` — public landing page
- `/services` — catalog
- `/login` and `/register` — authentication
- `/dashboard` — customer dashboard
- `/dashboard/orders` — customer order history
- `/admin` — administrator dashboard (ADMIN only)

## Security checklist before public launch

- Use HTTPS and managed PostgreSQL with backups.
- Set a strong `AUTH_SECRET`; rotate it if exposed.
- Add email verification, password reset, CSRF protections where applicable, rate limiting, bot protection, monitoring and security headers.
- Configure trusted origin and secure cookie settings for your domain.
- Integrate a payment provider's hosted checkout and verify payment server-side before changing order status.
- Add a privacy policy, terms, refund policy, and business contact details.
- Review the rules of each social platform and sell only compliant services.
- Keep supplier live mode disabled until the supplier, API key, pricing, refund behavior and service policy have been verified.

## Important limitation

This is a functional starter scaffold, not a fully audited production service. Before taking public customer payments, run tests, security review, email verification and a payment-provider integration.
