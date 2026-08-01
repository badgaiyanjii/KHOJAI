# KhojAI Implementation Playbook

This file is the working playbook for coding agents implementing KhojAI. It is not a generic repo policy file. It defines how to start building the product from the current documentation set, what is already decided, what must be implemented first, and what must not be changed.

## Current Repo State

- The repository is currently documentation-first.
- The implementation specifications live under `docs/`.
- The root `README.md` is a high-level overview only.
- The README currently contains stale links that point to root-level spec files; the authoritative spec files are the ones under `docs/`.

If there is any conflict between the README summary and the files in `docs/`, follow `docs/`.

## Source of Truth Order

Use the documents below in this precedence order when implementing. Higher items win if two docs conflict.

1. `docs/requirements.md`
2. `docs/workflow.md`
3. `docs/data-model.md`
4. `docs/api-spec.md`
5. `docs/architecture.md`
6. `docs/framework.md`
7. `docs/ui-tech.md`
8. `docs/ai-matching.md`
9. `README.md`
10. legacy PDF files in `docs/`

## Locked Product Decisions

These choices are already made. Do not re-decide them during implementation.

### Product Scope

- product name: `KhojAI`
- target: campus lost-and-found platform
- phase scope: `Phase 1 MVP`
- notifications: in-app only
- found items require physical deposit to a department office or lost-and-found cell
- admin approval and handover are mandatory for final closure

### Technical Stack

- web app: `React 18 + Vite`
- UI: `Tailwind CSS`
- routing: `React Router`
- web data fetching: `TanStack Query`
- web forms: `React Hook Form + Zod`
- API: `Node.js 20 + Express`
- database: `MongoDB + Mongoose`
- auth: `JWT access token + refresh session flow`
- uploads: `Multer`
- AI service: `Python 3.11 + FastAPI + PyTorch + open_clip`
- image storage: local filesystem under `storage/items/`

### Canonical Enums

Do not rename these values.

- item statuses: `open`, `under_review`, `claimed`
- claim statuses: `pending`, `approved`, `rejected`
- item types: `lost`, `found`
- match statuses: `suggested`, `dismissed`, `converted_to_claim`, `resolved`
- matching statuses: `pending`, `completed`, `failed`, `skipped`

## Non-Negotiable Constraints

- Do not introduce Firebase anywhere in Phase 1.
- Do not replace MongoDB with another persistence layer.
- Do not let the frontend call the AI service directly.
- Do not let the AI service write directly to MongoDB.
- Do not bypass admin review for claims.
- Do not mark an item `claimed` before handover confirmation.
- Do not add email or SMS delivery in Phase 1.
- Do not add external UI kits in Phase 1.
- Do not invent alternate API shapes when `docs/api-spec.md` already defines them.
- Do not change route, schema, or enum casing conventions from the current docs.

## Required Repository Layout

Create the codebase using this structure:

```text
/
  AGENTS.md
  README.md
  docs/
    requirements.md
    architecture.md
    workflow.md
    framework.md
    ui-tech.md
    api-spec.md
    data-model.md
    ai-matching.md
  apps/
    web/
    api/
  services/
    ai-matching/
  storage/
    items/
```

Do not move the current docs out of `docs/` during initial implementation.

## Phase 1 MVP Goal

Build the first working slice of KhojAI with:

- account registration and login
- authenticated item creation for lost and found reports
- image upload flow
- item list and item detail views
- claim submission flow
- admin claim review flow
- handover confirmation flow
- in-app notifications
- AI service scaffolding and API integration
- admin dashboard counts

## Implementation Order

Follow this order. Do not skip ahead unless a dependency requires a small scaffold.

### 1. Scaffold the Monorepo

Create:

- `apps/web`
- `apps/api`
- `services/ai-matching`
- `storage/items`

Set up:

- npm workspaces if used
- shared formatting and linting config
- environment variable templates
- baseline package scripts for `dev`, `build`, and `test`

### 2. Implement API Auth First

Start in `apps/api`.

Required deliverables:

- `users` model from `docs/data-model.md`
- `sessions` model from `docs/data-model.md`
- register endpoint
- login endpoint
- refresh endpoint
- logout endpoint
- `me` endpoint
- bcrypt password hashing
- JWT access tokens
- refresh session persistence
- auth middleware
- role guard middleware

No feature work should proceed until auth works end-to-end.

### 3. Implement Items and Uploads

Required deliverables:

- `items` model and indexes
- local file upload handling with type and size validation
- thumbnail generation hook or placeholder pipeline
- `POST /api/v1/items`
- `GET /api/v1/items`
- `GET /api/v1/items/:itemId`
- `PATCH /api/v1/items/:itemId`

Rules:

- `found` items require `image` and `depositLocation`
- `lost` items may omit `image`
- new items start as `open`
- API is responsible for all persistence and workflow state

### 4. Implement Claims and Workflow State

Required deliverables:

- `claims` model
- item status history support
- claim creation endpoint
- current user claim list
- admin claim queue
- approve claim action
- reject claim action
- handover confirmation action

Workflow must match `docs/workflow.md` exactly:

- claim creation moves item to `under_review`
- approve does not immediately mark item `claimed`
- handover confirmation marks item `claimed`
- remaining pending claims are auto-rejected on handover

### 5. Implement Notifications

Required deliverables:

- `notifications` model
- list notifications endpoint
- mark-one-read endpoint
- mark-all-read endpoint
- notification creation hooks for match, claim, approval, rejection, and handover events

Notifications are in-app only for Phase 1.

### 6. Scaffold and Integrate the AI Service

Required deliverables:

- FastAPI service skeleton in `services/ai-matching`
- health endpoint
- embeddings endpoint
- search endpoint
- model bootstrap using `open_clip`

API integration rules:

- API calls AI service after item creation when an image exists
- AI failure must never roll back item creation
- API stores embeddings, match records, and notification side effects
- API only compares opposite item types with eligible statuses

### 7. Build the Web App

Start after API auth and core item routes exist.

Required routes:

- `/login`
- `/register`
- `/dashboard`
- `/items`
- `/items/new`
- `/items/:itemId`
- `/claims`
- `/notifications`
- `/admin`
- `/admin/items`
- `/admin/claims`

Required UI coverage:

- login form
- registration form
- lost/found item form
- item list with filters
- item detail with match panel
- claim submission form
- notification center
- admin claim review
- handover confirmation action
- dashboard stat cards

Follow `docs/ui-tech.md` for tokens, layout behavior, and accessibility.

### 8. Add Dashboard Aggregates

Expose and render:

- total items
- lost item count
- found item count
- pending claim count
- successful returns

Use MongoDB aggregate queries in the API. Do not compute these from logs.

## Coding Rules for Agents

### API Rules

- Use the request and response envelopes from `docs/api-spec.md`.
- Keep route handlers thin.
- Put business logic in services.
- Keep validation close to each module.
- Use `camelCase` for fields and payloads.
- Use lower snake case enum values exactly as documented.

### Web Rules

- Use route-level pages plus feature components.
- Use React Hook Form and Zod for all forms.
- Use TanStack Query for remote state.
- Keep UI accessible and mobile-first.
- Use Tailwind only; do not add a component framework.

### AI Rules

- Load the model once on startup.
- Keep the service stateless.
- Return normalized embeddings.
- Do not own business workflow decisions inside the AI service.

## Definition of Done for Phase 1 MVP

Phase 1 is done only when all of the following are true:

- a new user can register and log in
- authenticated users can create lost items
- authenticated users can create found items with image and deposit location
- items can be listed, filtered, and opened in detail view
- claims can be submitted on eligible found items
- admins can approve or reject claims
- approved claims still require handover confirmation
- handover confirmation moves the item to `claimed`
- in-app notifications exist for the documented events
- AI integration runs on image-backed items and stores suggested matches
- AI failures do not block item creation
- dashboard metrics are visible to admins

## Validation Checklist for Every Major Change

Before considering a feature complete, verify:

- route shape matches `docs/api-spec.md`
- schema fields match `docs/data-model.md`
- workflow transitions match `docs/workflow.md`
- role behavior matches `docs/requirements.md`
- service boundaries match `docs/architecture.md`
- UI route and form behavior matches `docs/ui-tech.md`

If a requirement is missing or unclear, update the docs first or ask for clarification before inventing behavior in code.
