## Ethio Learners — LMS (Next.js 15)

Modern learning management system built with Next.js App Router. It supports public course browsing, admin course management (chapters/lessons, drag-and-drop ordering), secure media uploads to S3-compatible storage, Stripe payments and enrollments, Better Auth (GitHub + Email OTP) with admin support, transactional emails via Resend, and request protection via Arcjet.

Visit in development at [http://localhost:3000](http://localhost:3000).

### Tech Stack

- **Framework**: Next.js 15 (App Router, Server Actions, React 19)
- **Database/ORM**: PostgreSQL + Prisma
- **Auth**: Better Auth (GitHub OAuth + Email OTP, admin plugin)
- **Payments**: Stripe (checkout + webhooks)
- **Storage**: S3-compatible (presigned uploads)
- **Email**: Resend
- **Security**: Arcjet middleware (bot protection, shields, rate limits)
- **UI**: Tailwind CSS 4, Radix UI, ShadCN-inspired components, TipTap editor

### Features

- **Public**: Course catalog, course detail pages, slug-based routing
- **Auth**: GitHub OAuth, Email OTP verification, sessions stored in DB
- **Admin**: Create/edit courses, chapters, and lessons; reorder via DnD; delete flows
- **Media**: Direct S3 uploads for images/video; signed upload/delete routes
- **Payments**: Stripe checkout, webhook to activate enrollments
- **Access Control**: Middleware protects `/admin` route tree

---

## Project Structure

Key directories only:

- `app/(public)`: Public pages, course catalog and detail
- `app/(auth)`: Auth pages (login, verify-request)
- `app/admin`: Admin dashboard for courses/chapters/lessons
- `app/api`: API routes (auth, S3 upload/delete, Stripe webhook)
- `lib`: Clients and utilities (Prisma, auth, env, S3, Stripe, Arcjet, Resend)
- `prisma`: Prisma schema
- `components`: UI, editor, sidebar, file uploader

---

## Prerequisites

- Node.js 20+
- pnpm (recommended) or npm/yarn
- PostgreSQL database
- Stripe account, Resend account
- S3-compatible bucket (e.g., Cloudflare R2, MinIO, AWS S3)

---

## Environment Variables

Validated via `@t3-oss/env-nextjs` in `lib/env.ts`.

Server-side:

- `DATABASE_URL`
- `BETTER_AUTH_SECRET`
- `BETTER_AUTH_URL` (e.g., `http://localhost:3000` in dev)
- `AUTH_GITHUB_CLIENT_ID`
- `AUTH_GITHUB_CLIENT_SECRET`
- `RESEND_API_KEY`
- `ARCJET_KEY`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_ENDPOINT_URL_S3` (e.g., `https://<account>.r2.cloudflarestorage.com`)
- `AWS_ENDPOINT_URL_IAM`
- `AWS_REGION`
- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`

Client-side:

- `NEXT_PUBLIC_S3_BUCKET_NAME_IMAGES`

Create a `.env.local` with the variables above before running the app.

---

## Getting Started (Local Dev)

Install deps and generate Prisma client:

```bash
pnpm install
pnpm prisma migrate dev --name init
pnpm prisma generate
```

Run the dev server:

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000).

### Useful Scripts

```bash
pnpm dev         # Next.js dev (Turbopack)
pnpm build       # Production build (Turbopack)
pnpm start       # Start production server
pnpm lint        # Run ESLint
```

---

## Database (Prisma)

- Schema: `prisma/schema.prisma`
- Generated client output: `lib/generated/prisma`
- Models include `User`, `Session`, `Account`, `Verification`, `Course`, `Chapter`, `Lesson`, `Enrollment`
- Run migrations: `pnpm prisma migrate dev`

---

## Authentication (Better Auth)

- Server setup: `lib/auth.ts` with Prisma adapter and plugins: GitHub provider, Email OTP, `admin()`
- Client setup: `lib/auth-client.ts` with `emailOTPClient()` and `adminClient()`
- Middleware: `middleware.ts` protects all routes and specifically enforces auth for `/admin`
- Configure GitHub OAuth with your app credentials

---

## Storage (S3-compatible)

- S3 client: `lib/S3Client.ts` uses `AWS_ENDPOINT_URL_S3` and region `auto`
- Upload/delete API routes under `app/api/s3/*` provide presigned operations
- Public image host is allowed in `next.config.ts` under `images.remotePatterns`

---

## Payments (Stripe)

- Stripe client: `lib/stripe.ts`
- Webhook route: `app/api/webhook/stripe/route.ts`
- Set `STRIPE_SECRET_KEY` and `STRIPE_WEBHOOK_SECRET`

Local webhook forwarding (example):

```bash
stripe listen --forward-to localhost:3000/api/webhook/stripe
```

---

## Email (Resend)

- Config: `lib/resend.ts`
- Used by Better Auth Email OTP in `lib/auth.ts`

---

## Security (Arcjet)

- Base config: `lib/arcjet.ts` (shield, characteristics)
- Middleware: `middleware.ts` uses Arcjet with `detectBot` and allows `STRIPE_WEBHOOK` category
- Set `ARCJET_KEY` from your Arcjet dashboard

---

## Contributing

1. Create a feature branch
2. Make changes with clear commits
3. Ensure `pnpm lint` passes
4. Open a PR

---

## License

Proprietary. All rights reserved unless otherwise stated.
