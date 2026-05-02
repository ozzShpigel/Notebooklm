# Implementation Plan

Tasks are grouped into phases that each produce a working slice of the product. Earlier phases unblock later ones — finish a phase before starting the next.

## Phase 1: Foundation

- [ ] Initialize monorepo (`/client`, `/server`, `/core`, `/e2e`) with shared TypeScript config
- [ ] Set up Express server with TypeScript
- [ ] Set up React app with TypeScript, Tailwind, and React Router
- [ ] Set up PostgreSQL locally (Docker is fine)
- [ ] Add Prisma, point at the database, run an initial empty migration
- [ ] Create the `core` package for shared Zod schemas and constants
- [ ] Add a `validate` helper on the server that runs a Zod schema against `req.body` and returns 400 on failure
- [ ] Add a `parseId` helper for numeric route params

## Phase 2: Authentication

- [ ] Add `User` model to Prisma schema (id, email, password hash, role: `admin` | `agent`)
- [ ] Implement password hashing on user create
- [ ] Implement database-backed session storage (sessions table)
- [ ] Build login API endpoint
- [ ] Build logout API endpoint
- [ ] Build session-validation middleware that sets `req.user`
- [ ] Build a `requireAdmin` middleware for admin-only routes
- [ ] Seed the initial admin account from environment variables
- [ ] Build login page on the client
- [ ] Build a `ProtectedRoute` wrapper that redirects to `/login` if unauthenticated
- [ ] Build an `AdminRoute` wrapper that redirects non-admins to `/`

## Phase 3: User Management (Admin)

- [ ] List users API endpoint (admin only)
- [ ] Create agent API endpoint (admin only)
- [ ] Edit user API endpoint (admin only)
- [ ] Delete user API endpoint (admin only)
- [ ] Users page on the client (table of users + create/edit/delete actions)
- [ ] Form for creating an agent (email, password, role)

## Phase 4: Ticket Domain

- [ ] Add `Ticket` model to Prisma schema (id, subject, body, requester email, status, category, assignee, timestamps)
- [ ] Define `TicketStatus` and `TicketCategory` constants in `core`
- [ ] List tickets API endpoint with filters (status, category) and sort (recent first)
- [ ] Get ticket detail API endpoint
- [ ] Update ticket API endpoint (change status, assign agent)
- [ ] Ticket list page with filter controls and sortable columns
- [ ] Ticket detail page (show subject, body, status, category, assignee)
- [ ] Status change controls on the detail page
- [ ] Assign-to-agent control on the detail page

## Phase 5: Inbound Email Pipeline

- [ ] Sign up for an email provider (SendGrid or Mailgun) and configure inbound webhook routing
- [ ] Set up the background job queue (`pg-boss` against the same Postgres instance)
- [ ] Wire `startQueue()` into server boot and `stopQueue()` into graceful shutdown
- [ ] Build inbound-email webhook endpoint (parses multipart form, extracts subject/body/from)
- [ ] Create ticket from inbound email with status `new`
- [ ] Add `processing` status; transition `new` → `processing` when the AI starts work
- [ ] Hide `new` and `processing` tickets from the agent UI by default

## Phase 6: AI Classification & Summarization

- [ ] Set up Claude API integration (API key, client wrapper)
- [ ] Background job: classify ticket into one of the three categories
- [ ] Persist category on the ticket
- [ ] Endpoint: generate ticket summary on demand
- [ ] Show category and summary on the ticket detail page
- [ ] Handle AI failures with retries and exponential backoff (queue config)

## Phase 7: AI Auto-Response

- [ ] Define the knowledge base data model (or pick a simple file/DB layout)
- [ ] Seed the knowledge base with initial content
- [ ] Background job: attempt to auto-resolve a ticket using the KB
- [ ] If confident, draft a reply, send it, transition ticket to `resolved`
- [ ] If not confident, transition ticket to `open` (visible to agents)
- [ ] Endpoint: AI-suggested reply for an open ticket (used by agents on detail page)
- [ ] "Use suggestion" button on the detail page that pre-fills the reply box

## Phase 8: Outbound Email & Threading

- [ ] Outbound email send when an agent (or AI) replies
- [ ] Persist replies as messages linked to the ticket
- [ ] Email threading headers (Message-ID, In-Reply-To, References) on outbound mail
- [ ] Inbound replies are appended to the existing ticket instead of creating a new one
- [ ] Show full message thread on the ticket detail page

## Phase 9: Dashboard

- [ ] Stats API endpoint (counts by status and by category)
- [ ] Dashboard page with status counts (open / resolved / closed)
- [ ] Category breakdown widget
- [ ] Recent tickets list
- [ ] Quick filter links into the ticket list page

## Phase 10: Quality & Deployment

- [ ] Component tests (Vitest + React Testing Library) for ticket list, detail, and login
- [ ] E2E tests (Playwright) for login redirect and webhook → ticket-appears-in-UI
- [ ] Error and loading states across pages
- [ ] Centralized error component for API failures
- [ ] Dockerfile for the server
- [ ] Dockerfile (or static-build setup) for the client
- [ ] `docker-compose.yml` for local Postgres + server + client
- [ ] Deployment config for the chosen host
