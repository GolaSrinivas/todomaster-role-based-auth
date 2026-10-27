# TodoMaster — Role-Based Task Management App

TodoMaster is a full-stack task management application built with Next.js and TypeScript. It uses Clerk for authentication, PostgreSQL with Prisma for data storage, and role-based access for user and admin experiences.

## Features

- User sign-up, sign-in, and email verification with Clerk
- Create, view, complete, and delete personal todos
- Search and paginate todo lists
- Role-based admin dashboard to look up users and manage their todos
- Clerk webhook integration to create a database user when a new account is registered
- Demo subscription status and a free-tier limit of three todos

> **Note:** The subscription flow in the current code is a demonstration only. It does not process payments; the API can activate a subscription without payment confirmation. Do not use it as a real billing system without implementing and verifying payment-provider integration.

## Tech Stack

- **Frontend:** Next.js 14, React, TypeScript, Tailwind CSS
- **Authentication:** Clerk
- **Database:** PostgreSQL (for example, Neon)
- **ORM:** Prisma
- **UI:** Radix UI components, Lucide icons
- **Webhook verification:** Svix

## Getting Started

### Prerequisites

- Node.js 18.17 or later
- npm
- A PostgreSQL database
- A Clerk application

### 1. Clone the repository

Replace the URL with the repository URL after you create it on GitHub.

```bash
git clone
cd
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a file named `.env.local` in the project root and add your own credentials:

```env
DATABASE_URL="your_postgresql_connection_string"
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="your_clerk_publishable_key"
CLERK_SECRET_KEY="your_clerk_secret_key"
WEBHOOK_SECRET="your_clerk_webhook_signing_secret"
```

Use the exact variable names above. Get the Clerk keys from your Clerk Dashboard and the database URL from your PostgreSQL provider. Never commit `.env.local` or publish real keys, passwords, or connection strings.

### 4. Set up the database

Generate the Prisma Client and apply the migrations included in the repository:

```bash
npx prisma generate
npx prisma migrate deploy
```

### 5. Configure the Clerk webhook

The app uses a webhook to create a corresponding database record when a Clerk user registers.

1. In the Clerk Dashboard, create a webhook endpoint pointing to `https://YOUR_DOMAIN/api/webhook/register`.
2. Subscribe to the `user.created` event.
3. Copy the webhook signing secret into `WEBHOOK_SECRET` in `.env.local`.
4. For local testing, expose your development server through a secure tunnel so Clerk can reach the webhook endpoint.

### 6. Configure an admin user

The application checks the user's Clerk public metadata for `role: "admin"`. For development, set the `role` value to `admin` in the intended test user's **Public metadata** in the Clerk Dashboard. Only assign admin access to trusted accounts.

### 7. Run the application

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Available Scripts

| Command         | Description                              |
| --------------- | ---------------------------------------- |
| `npm run dev`   | Start the development server             |
| `npm run build` | Create a production build                |
| `npm run start` | Run the production server after building |
| `npm run lint`  | Run the project's lint command           |

## Project Structure

```text
app/
  (authenticated)/   # Dashboard, admin dashboard, subscription page
  api/               # Todo, admin, subscription, and webhook endpoints
  sign-in/           # Sign-in page
  sign-up/           # Sign-up and email verification
components/          # Shared UI and todo components
hooks/               # Shared React hooks
lib/                 # Prisma client and utilities
prisma/               # Database schema and migrations
middleware.ts         # Authentication and role-based routing
```
