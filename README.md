# Remindify

**A governed workspace for recurring business obligations and proof-gated payments.**

Remindify gives every obligation an owner, a backup, a risk level and a definition of acceptable proof. People and scoped agents work from the same register. For PayPal payments, the payer authorizes a hold and funds are captured only after the submitted evidence passes review.

> **Live demo:** [remindify-paypal.vercel.app](https://remindify-paypal.vercel.app). Create an account to open a private, seeded workspace. PayPal money movement is configured for Sandbox.

## What to try

1. **Capture:** Turn a renewal notice, filing or handoff into a proposed obligation. Review the parsed details before saving.
2. **Govern work:** See owners, backups, risk, agent scopes and autonomy limits together. Run the built-in sandbox executor to inspect an agent run and its proof record.
3. **Check evidence:** Open **Trust** to review proof receipts and the PayPal ledger. The seeded escrow is labelled as a rehearsal and cannot settle; create a fresh PayPal Sandbox authorization for a full capture and payout.
4. **Inspect agent access:** Review the scoped API and MCP surfaces under **Agent access**.

The app and core demo flows work without third-party credentials. Capture and proof review use deterministic local fallbacks when Gemini is not configured. PayPal payment and payout actions require PayPal Sandbox credentials.

## Run locally

Requires Node.js 22.5 or later.

```bash
npm ci
npm run dev
```

Open [http://localhost:5173](http://localhost:5173). Create an account to open a private workspace seeded with example obligations and agents. The development server starts the API on port `4178` and the Vite app on port `5173`.

To run the production build locally:

```bash
npm run build
npm start
```

Then open [http://localhost:4178](http://localhost:4178).

## PayPal Sandbox

The payment rail uses PayPal Orders and authorizations for holds, captures verified holds after evidence review, and sends provider payouts through PayPal Payouts. PayPal invoicing is also available. The integration defaults to Sandbox; the seeded rehearsal record has placeholder PayPal identifiers and is deliberately blocked from settlement.

To exercise real Sandbox requests, configure these variables in your local environment:

```bash
PAYPAL_ENV=sandbox
PAYPAL_CLIENT_ID=your-sandbox-client-id
PAYPAL_CLIENT_SECRET=your-sandbox-client-secret
```

Use a PayPal Sandbox buyer to approve a hold. Never commit credentials or enable live payments for the hackathon demo.

## Vercel deployment

Vercel functions do not provide persistent local storage, so a deployed app needs its own Postgres database. Set these variables in the Vercel project:

```text
DATABASE_URL
PUBLIC_ORIGIN=https://your-project.vercel.app
COOKIE_SECURE=true
```

Optional integrations:

| Variable | Purpose |
| --- | --- |
| `PAYPAL_ENV=sandbox`, `PAYPAL_CLIENT_ID`, `PAYPAL_CLIENT_SECRET` | Enable PayPal Sandbox payments and payouts. |
| `GEMINI_API_KEY` | Enable Gemini-assisted capture and multimodal proof review; deterministic fallbacks remain available without it. |
| `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` | Enable Google sign-in. Email and password sign-in remain available without OAuth. |

Keep all secret values in Vercel's environment-variable settings, not in this repository. Do not use SQLite for a public Vercel deployment.

## Development checks

```bash
npm test          # regression tests
npm run typecheck  # TypeScript
npm run build     # client, server-rendered pages and SEO assets
```

The stack is React, TypeScript, Vite and Node.js. Local development uses SQLite; deployments use Postgres. See [SECURITY.md](./SECURITY.md) for the security reporting policy.

## Honest limitations

- This is a hackathon prototype, not an audited or production-ready payment service.
- PayPal availability and payout eligibility depend on the merchant's account and region. The demo is intended for Sandbox use, not real-money settlement.
- Gemini is optional. When it is not configured, the deterministic local parser and auditor are used; image-based AI review requires Gemini.
- A proof receipt records submitted evidence and the audit result; it is not independent proof that the underlying claim is true.

## License

MIT. See [LICENSE](./LICENSE).
