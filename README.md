# Ethio Learners LMS

EthioLearners is a full‑stack Learning Management System for creating, publishing, and selling online courses. Learners can browse public courses, purchase with Stripe, enroll, and track lesson progress; instructors/admins can create and manage courses, chapters, and lessons with rich content and media uploads.

## Features

- **User Authentication:** Secure login via Better Auth (with GitHub OAuth) and protected routes.
- **Course Catalog:** Public course listings with categories, levels, pricing, and detailed pages.
- **Purchases & Enrollment:** Stripe-powered one‑time payments and automatic enrollment on success.
- **Lesson Player & Progress:** Watch lessons and auto‑track completion progress per user.
- **Rich Text Content:** Tiptap editor for course descriptions and lesson content.
- **Media Uploads:** S3-compatible storage for thumbnails and videos with presigned uploads.
- **Admin Dashboard:** Create/edit courses, chapters, and lessons; manage publishing and pricing.
- **Responsive UI:** Modern, accessible components built with Next.js, Tailwind, and Radix UI.

## Tech Stack

- **Framework:** Next.js 15 (App Router), React 19, TypeScript
- **Database & ORM:** PostgreSQL, Prisma
- **Auth:** Better Auth (with GitHub OAuth)
- **Payments:** Stripe (webhooks for fulfillment)
- **Email:** Resend
- **Storage:** AWS S3 compatible (custom endpoint supported)
- **Validation:** Zod with `@t3-oss/env-nextjs`
- **UI:** Tailwind CSS v4, Radix UI, shadcn‑style components
- **Other:** Arcjet (bot/abuse protection), Recharts, DnD Kit

## Project Structure

```
lms/
│
├── app/                    # Next.js App Router routes
│   ├── (public)/           # Public pages (home, courses, course detail)
│   ├── (auth)/             # Auth routes (login, verify)
│   ├── admin/              # Admin dashboard for managing content
│   ├── dashboard/          # Learner dashboard and lesson player
│   └── api/                # Route handlers (auth, s3, stripe webhook)
│
├── components/             # UI components (ui, editor, uploader, sidebar)
├── hooks/                  # Reusable React hooks
├── lib/                    # Server libs (auth, db, env, stripe, s3)
├── prisma/                 # Prisma schema
└── public/                 # Static assets
```

## Getting Started

### Prerequisites

- Node.js 18+ (or 20+ recommended)
- pnpm (recommended) or npm
- A PostgreSQL database
- Stripe account (for test mode)

### 1) Install dependencies

```bash
pnpm install
# or
npm install
```

### 2) Environment variables

Create a `.env` file in the project root with at least the following variables. Adjust values to your environment.

```bash
DATABASE_URL="postgresql://user:password@localhost:5432/ethiolearners"

# Better Auth
BETTER_AUTH_SECRET="replace-with-random-secret"
BETTER_AUTH_URL="http://localhost:3000"

# GitHub OAuth (used by Better Auth)
AUTH_GITHUB_CLIENT_ID="gh-client-id"
AUTH_GITHUB_CLIENT_SECRET="gh-client-secret"

# Email (Resend)
RESEND_API_KEY="re_..."

# Arcjet (optional)
ARCJET_KEY="aj_..."

# S3 (or S3-compatible) storage
AWS_ACCESS_KEY_ID="..."
AWS_SECRET_ACCESS_KEY="..."
AWS_ENDPOINT_URL_S3="https://s3.your-endpoint.com"   # e.g., Cloudflare R2/Bunny/AWS
AWS_ENDPOINT_URL_IAM="https://iam.your-endpoint.com" # if applicable
AWS_REGION="us-east-1"
NEXT_PUBLIC_S3_BUCKET_NAME_IMAGES="your-bucket-name"

# Stripe
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..." # set after creating a local webhook
```

### 3) Database setup

```bash
# generate prisma client
pnpm prisma generate

# create tables in your database based on schema
pnpm prisma db push
```

### 4) Run the app

```bash
pnpm dev
# App runs at http://localhost:3000
```

## Payments (Stripe Test Mode)

- To purchase a course in test mode, use the sandbox card:
  - Card: `4242 4242 4242 4242`
  - Expiration: `04/42`
  - CVC: `424`
- Ensure `STRIPE_SECRET_KEY` is a test key and your webhook is configured.

### Webhook setup (local)

```bash
# in another terminal, forward Stripe events to your local app
stripe listen --events checkout.session.completed --forward-to localhost:3000/api/webhook/stripe

# copy the printed signing secret into STRIPE_WEBHOOK_SECRET
```

## Key API Routes

- `POST /api/s3/upload` – Request a presigned URL to upload media
- `POST /api/s3/delete` – Delete uploaded media by key
- `POST /api/webhook/stripe` – Stripe checkout webhook (enrollment fulfillment)
- `GET /api/auth/...` – Auth routes managed by Better Auth

## Live Demo

You can view the live application here: `https://ethiolearners.vercel.app`

## Scripts

```bash
pnpm dev     # start dev server (Next.js + Turbopack)
pnpm build   # production build
pnpm start   # run production server
pnpm lint    # run ESLint
```

## License

This project is licensed under the ISC License.
