# Build and Ship an AI-Assisted Full-Stack App

A practical guide for beginners, based on a workflow that starts with a clear specification, builds a testable frontend first, defines an API contract, adds a backend, and only then introduces persistence and deployment preparation.[cite:1]

## What this approach is trying to achieve

The article’s example app is a collaborative system-design interview tool where an interviewer creates a session, shares a link, and both people edit a diagram in real time through a shared WebSocket room backed by FastAPI and SQLite.[cite:1]

The deeper lesson is not the specific app. It is the delivery pattern: build something interactive early, keep interfaces stable, replace temporary parts one at a time, and make every stage runnable and testable before moving on.[cite:1]

## The core development sequence

The article lays out a staged path:

1. Turn the idea into a written specification.[cite:1]
2. Generate a frontend that uses mocked backend calls.[cite:1]
3. Create an OpenAPI contract from the frontend service layer.[cite:1]
4. Implement a backend to match that contract.[cite:1]
5. Connect frontend and backend and test the end-to-end flow.[cite:1]
6. Replace temporary storage with SQLite and SQLAlchemy for persistence.[cite:1]
7. Prepare the app for deployment work such as containers, CI/CD, integration tests, migrations, and production databases.[cite:1]

This matters because each phase ends with a concrete artifact that can be validated: first the specification, then an interactive prototype, then a connected full-stack app, then a persistent local application ready for deployment.[cite:1]

## Step 1: Start with a specification

The article recommends beginning with a precise description of the app before building anything. In the example, the specification covers who creates a session, how another user joins, what can be placed on the canvas, and how changes appear in real time for both users.[cite:1]

This is presented as specification-driven development. The point is to avoid building something merely functional but misaligned with the real need.[cite:1]

### What a beginner should capture in the spec

Use a short markdown spec that answers:

- Who are the users?[cite:1]
- What is the core workflow from start to finish?[cite:1]
- What data must be created, viewed, updated, or shared?[cite:1]
- Which actions need real-time behavior?[cite:1]
- What should happen when the app starts, fails, or reloads?

A good starter template is:

```md
# App specification

## Goal
One sentence describing the job of the app.

## Users
- Primary user
- Secondary user

## Main flow
1. User starts here
2. User performs action
3. System responds
4. Another user or system sees the result

## Core entities
- Entity A
- Entity B
- Entity C

## Key features
- Feature 1
- Feature 2
- Feature 3

## Real-time or async behavior
- What updates live?
- What can wait?

## Non-goals
- Things not included in version 1
```

The article specifically notes that dictating the idea into ChatGPT can be a fast way to generate the initial specification, as long as enough detail is provided up front.[cite:1]

## Step 2: Build the frontend first

For solo builders and simple projects, the article recommends starting with the frontend because it is the fastest way to see whether the idea and specification actually make sense in use.[cite:1]

The suggested prompt pattern is to create the UI, centralize all backend calls in one service layer, provide a mock implementation of that service, and add tests.[cite:1]

### Why the mock service layer is so important

The article emphasizes one instruction in particular: centralize every backend call in one services layer and create a mock implementation so the app works without a real backend.[cite:1]

That gives you three benefits:

- The frontend is interactive from day one.[cite:1]
- The service layer becomes the future integration point for the real backend.[cite:1]
- You avoid scattering API assumptions throughout UI components, which makes later replacement much easier.[cite:1]

### Suggested frontend structure

```text
frontend/
  src/
    components/
    pages/
    services/
      api.ts
      mockApi.ts
    hooks/
    tests/
```

A beginner-friendly rule is simple: UI components should never call fetch directly if the app is meant to grow. All server interactions should go through the services layer, because that is the seam you will later connect to the real backend.[cite:1]

### What to test at this stage

The article says each stage should be testable, and the frontend prototype should already run with mocked backend calls.[cite:1]

At minimum, test:

- Can a user load the app?
- Can the main workflow be completed against mocked data?
- Do loading, success, and failure states behave sensibly?
- Are backend dependencies isolated behind the service layer?

## Step 3: Move the generated frontend into a real project structure

Once the initial frontend is generated, the article recommends cloning it locally, reorganizing the repository, and committing the rearranged structure before continuing.[cite:1]

The example target layout is:

```text
/backend   # backend application and tests
/docs      # supporting documentation
/frontend  # frontend application
AGENTS.md  # instructions for coding agents
openapi.yaml
```

This is a useful beginner move because it separates product code, documentation, agent instructions, and API contract early, before the project becomes messy.[cite:1]

### Local run loop

The article shows a frontend run flow of:

```bash
cd frontend
npm i
npm run dev
```

The important takeaway is not the exact command. It is that after reorganizing files, the app should still run locally before more changes are made.[cite:1]

## Step 4: Add AGENTS.md to steer coding assistants

The article recommends putting an `AGENTS.md` file at the repository root with practical instructions for coding agents, such as using `uv` for backend dependency management, running code regularly, and committing often.[cite:1]

This file works like a lightweight operating manual for the assistant. It reduces drift, keeps tool choices consistent, and helps maintain a predictable development workflow as the project grows.[cite:1]

### A useful beginner version of AGENTS.md

```md
# AGENTS.md

## General rules
- Keep the app runnable after each change.
- Prefer small, testable changes.
- Write or update tests when behavior changes.
- Do not break the existing folder structure without reason.

## Backend rules
- Use uv for Python dependency management.
- Useful commands:
  - uv sync
  - uv add <package>
  - uv run python -m pytest
  - uv run uvicorn backend.main:app --reload --port 8091

## Frontend rules
- Keep all server calls in the services layer.
- Do not call APIs directly from UI components.
- Update mocks when the contract changes.

## Git workflow
- Commit after each stable milestone.
- Use clear commit messages.
```

The article notes that this file is only a starting point and should evolve with the project.[cite:1]

## Step 5: Create the API contract with OpenAPI

After the frontend exists, the article recommends reading the frontend API client and generating an `openapi.yaml` file that describes every expected endpoint, method, path, request body, response body, and authentication requirement.[cite:1]

This becomes the explicit agreement between frontend and backend. The article argues this step saves tokens, reduces ambiguity, and gives the backend a precise target instead of forcing the assistant to infer behavior from UI code.[cite:1]

### Why this is valuable for beginners

Without a contract, frontend and backend can drift in naming, payload shapes, error handling, and authentication assumptions. An explicit OpenAPI file keeps both sides aligned and makes debugging far easier when things fail.[cite:1]

### What to include in the contract

| Area | What to define |
|---|---|
| Endpoints | Every route the frontend needs.[cite:1] |
| Methods | GET, POST, PUT, DELETE, etc., per route.[cite:1] |
| Request body | Exact input schema for writes and updates.[cite:1] |
| Response body | Exact response schema for UI consumption.[cite:1] |
| Auth | Which endpoints need authentication and what token scheme is used.[cite:1] |
| Errors | Expected failure cases and status codes. |

The article says this step can be skipped, but does not recommend skipping it because of the clarity it adds.[cite:1]

## Step 6: Build the backend against the contract

Once the OpenAPI spec exists, the article moves to implementing a FastAPI backend that matches it, using an in-memory store first, seeded with data, and adding authentication with hashed passwords and bearer tokens where needed.[cite:1]

It also recommends splitting the backend into modules such as routers, models, store, and auth, and writing tests.[cite:1]

### Why start with in-memory storage first

The article uses the same pattern as with the frontend: begin with a temporary implementation that lets the whole stack connect cleanly before worrying about persistence.[cite:1]

That means the first backend milestone is not “production database complete.” It is “frontend and backend speak correctly, the flows work, and the system is testable.”[cite:1]

### Beginner-friendly backend layout

```text
backend/
  main.py
  routers/
  models/
  store/
  auth/
  tests/
```

### Key backend goals at this stage

- Match the OpenAPI contract exactly.[cite:1]
- Seed sample data so the frontend has meaningful content to display.[cite:1]
- Add authentication only where the contract requires it.[cite:1]
- Keep code modular enough that storage can later be swapped out.[cite:1]

## Step 7: Add a Makefile and a simple run surface

The article recommends creating a Makefile so the backend can be started with a simple `make run` command instead of remembering the full uvicorn invocation.[cite:1]

This is a small improvement, but it matters for beginners because reducing command friction makes it easier to stay in a tight build-test-fix loop.[cite:1]

The article’s example backend command is:

```bash
cd backend
uv run uvicorn backend.main:app --reload --port 8091
```

After running it, the article suggests opening the FastAPI docs endpoint to inspect the implemented API and compare it with the original specification.[cite:1]

## Step 8: Connect the real frontend and backend

Once the backend is running, the article switches the frontend from the mock client to the real backend client.[cite:1]

It warns that this usually does not work perfectly the first time and specifically mentions CORS errors and other integration problems.[cite:1]

### What to test end to end

The article recommends opening two browser windows, creating a session in one, joining it from the second, and verifying that interactions in one session propagate to the other.[cite:1]

That is an excellent beginner testing pattern because it validates:

- Session creation.[cite:1]
- Shared-link join behavior.[cite:1]
- Real-time synchronization across clients.[cite:1]
- The backend event flow between UI, WebSocket handling, and storage.[cite:1]

### Common issues beginners should expect

- CORS misconfiguration when frontend and backend run on different local ports.[cite:1]
- Payload shape mismatches between frontend expectations and backend responses.[cite:1]
- Missing auth headers on protected requests.[cite:1]
- Real-time events that update one client but not another because of room or session bugs.[cite:1]

## Step 9: Replace in-memory storage with a database

After the app works end to end, the article replaces the in-memory store with SQLite and SQLAlchemy so data survives backend restarts.[cite:1]

The prompt also asks for an environment variable to control the database connection and for a database-agnostic approach so the app can later move to something like Postgres without major rewrites.[cite:1]

### Why SQLite is a strong beginner choice

The article describes SQLite as lightweight and well suited for local testing.[cite:1]

Using SQLAlchemy at the same time gives a path to production databases later, while avoiding SQLite-specific design choices in the first version.[cite:1]

### What changes and what should not change

When this step is done well, storage changes but the overall app flow stays the same. The article explicitly shows SQLite replacing the in-memory store without changing the rest of the application behavior.[cite:1]

That is a major architectural lesson: if earlier layers were separated properly, persistence can be swapped with minimal impact on the frontend and limited impact on the API surface.[cite:1]

## Step 10: Verify persistence and stability

The article recommends testing multiple sessions, confirming cross-session updates, stopping the backend, starting it again, and checking that state is still present after restart.[cite:1]

This is a powerful beginner habit because it tests not only functionality, but also durability. A feature that only works until the process restarts is not yet a finished application.[cite:1]

## Deployment preparation

The article ends with the app working locally and identifies the next deployment-oriented tasks: containerizing frontend and backend, adding integration tests, setting up CI, running Postgres with Docker Compose, adding database migrations, deploying to a public environment, and setting up CD.[cite:1]

So the full lifecycle in this workflow is: local spec, local prototype, local integrated app, local persistence, then production hardening and deployment automation.[cite:1]

### What “ready for deployment” really means here

It does **not** mean the app is already deployed. It means the local app has enough structure and stability that deployment work can begin in a disciplined way rather than as a rescue mission.[cite:1]

## Reusable blueprint for beginners

Use this order for a first AI-assisted full-stack project:

1. Write a short spec before generating code.[cite:1]
2. Generate the frontend first so the idea can be judged early.[cite:1]
3. Force all backend communication through one service layer and mock it first.[cite:1]
4. Export or reorganize the project into a clean repo structure.[cite:1]
5. Add `AGENTS.md` so coding assistants follow consistent rules.[cite:1]
6. Derive an explicit OpenAPI contract from the frontend’s needs.[cite:1]
7. Build the backend to the contract using temporary in-memory storage first.[cite:1]
8. Add tests at each stage, not only at the end.[cite:1]
9. Connect the real frontend and backend and fix integration issues.[cite:1]
10. Replace temporary storage with a real database through an abstraction layer.[cite:1]
11. Confirm the app survives restart and retains state.[cite:1]
12. Only then move into containerization, CI/CD, migrations, and cloud deployment.[cite:1]

## Why this workflow works well with AI coding assistants

The article’s step-by-step approach keeps the assistant focused on one stable milestone at a time, with each step producing something runnable.[cite:1]

That reduces ambiguity, makes debugging more targeted, and lowers the chance that the assistant generates a large but fragile codebase all at once.[cite:1]

For beginners, this is especially effective because it turns “build a full-stack app” into a sequence of narrow, verifiable tasks rather than a single overwhelming request.[cite:1]

## A practical starter stack

The article’s example stack is a sensible template for new builders:

| Layer | Starter choice | Why it helps |
|---|---|---|
| Spec | Markdown plus AI-assisted drafting | Fast to create and easy to revise.[cite:1] |
| Frontend | React/TypeScript generated by a coding tool | Gets an interactive UI running quickly.[cite:1] |
| Frontend integration | Centralized services layer with mock implementation | Makes backend replacement straightforward later.[cite:1] |
| Contract | OpenAPI | Keeps frontend and backend aligned.[cite:1] |
| Backend | FastAPI | Rapid API implementation with docs support.[cite:1] |
| Temp storage | In-memory store | Good for early integration before persistence work.[cite:1] |
| Persistence | SQLite + SQLAlchemy | Lightweight locally, extensible later.[cite:1] |
| Run tooling | Makefile + uv | Simplifies commands and environment management.[cite:1] |

## Beginner mistakes this workflow helps avoid

This method directly helps avoid several common problems:

- Building backend and database too early before the UI and workflow are proven.[cite:1]
- Letting UI components call backend code directly instead of isolating server calls in a service layer.[cite:1]
- Allowing frontend and backend contracts to remain implicit and drift apart.[cite:1]
- Treating persistence, authentication, and real-time behavior as one giant first milestone instead of staged replacements.[cite:1]
- Waiting until the end to test whether the app actually works across two real client sessions.[cite:1]

## Suggested milestone checklist

```md
- [ ] App idea written as a short specification
- [ ] Frontend generated and runs locally
- [ ] All backend calls isolated in a service layer
- [ ] Mock backend implementation works
- [ ] Frontend tests added
- [ ] Repository reorganized into frontend/backend/docs
- [ ] AGENTS.md created
- [ ] openapi.yaml created from frontend needs
- [ ] FastAPI backend implemented from the contract
- [ ] Auth added where required
- [ ] Backend tests added
- [ ] Frontend switched to real backend
- [ ] Two-window end-to-end test passes
- [ ] In-memory storage replaced by SQLite + SQLAlchemy
- [ ] Restart test confirms persistence
- [ ] App is ready for containerization and CI/CD work
```

## Final takeaway

The most important lesson from the article is to replace uncertainty with staged contracts and runnable milestones. Start with a spec, prototype with mocks, define the contract, connect the system, then add persistence and deployment concerns only after the core app works.[cite:1]

That makes AI-assisted full-stack development far more manageable for beginners because every prompt has a narrow purpose, every layer has a clear boundary, and every step ends with something concrete that can be tested.[cite:1]

[cite:1] https://aishippingblog.com/p/build-and-ship-a-full-stack-app-with
