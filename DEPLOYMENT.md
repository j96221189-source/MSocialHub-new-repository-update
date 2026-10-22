# MSocialHub — New Repository and Vercel Deployment

This archive is structured with the application files at the repository root. Extract it before uploading; do not upload this ZIP as a single file to GitHub.

## Important status and safety notes

- This source is a Next.js/TypeScript starter and must not be represented as a fully production-ready live reseller service.
- Supplier integration is intentionally MOCK-only. It does not call SocialPlatforms and does not spend supplier wallet funds. Do not enable paid automatic fulfillment until a real supplier adapter is implemented and tested against the supplier's current API docs.
- Payment checkout is not fully implemented in this archive; the existing NOWPayments webhook route alone does not create a hosted invoice/checkout. Keep `PAYMENTS_MODE=manual` and do not accept live payments until checkout creation, IPN verification, order-to-payment binding, amount/currency checks, and fulfillment gating have been tested end to end.
- Never put API keys, IPN secrets, database passwords, or `AUTH_SECRET` in source code or GitHub. Add them only as Vercel environment variables.
- No deployment is claimed by this archive. Verify the Vercel build and runtime after configuring the project.

## GitHub upload from iPhone

1. Download and extract this ZIP using the Files app or a computer.
2. Create/open the new GitHub repository.
3. Upload the *contents* of the extracted folder so `package.json` is at the repository root. Do not upload only the ZIP. If GitHub mobile cannot upload folders, use GitHub's web upload flow from a computer or GitHub Desktop.
4. Commit the files to the default branch.

Expected root includes `package.json`, `src/`, `prisma/`, `next.config.ts`, `tsconfig.json`, and `.env.example`.

## Vercel setup

1. Import the new GitHub repository into Vercel as a Next.js project.
2. Set the required environment variables in Vercel for the relevant environment: `DATABASE_URL`, `AUTH_SECRET`, `SUPPLIER_MODE=mock`, `SUPPLIER_LIVE_ENABLED=false`, `PAYMENTS_MODE=manual`, and `APP_URL` set to the actual deployment URL. Keep supplier/payment secrets blank until their integrations are implemented and verified.
3. Ensure the database is a managed PostgreSQL instance and run Prisma generation/schema setup through a controlled deployment step. Do not run destructive schema changes against production without a backup.
4. Deploy a preview first. Inspect Build Logs and test registration/login, customer order flow, admin authorization, database health, and error handling.
5. Do not switch production traffic or enable live payment/supplier calls until all checks pass.

## Local verification

Use Node.js 20+ and npm:

```bash
npm ci
npm run db:generate
npm run build
```

A successful build does not by itself verify production payment processing, database connectivity, or supplier fulfillment.
