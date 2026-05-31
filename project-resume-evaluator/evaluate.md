# Resume Worthiness Review: Agent Instructions

**Use your reasoning model / thinking tokens for this skill.** Do not give surface-level answers. Reason through each criterion deliberately — weigh evidence, spot contradictions, and justify every verdict with specific observations from the project. A lazy evaluation is worse than none.

Evaluate whether a given project is strong enough to put on a resume.

**User context:** Fresher, targeting backend/software engineer roles. All projects are fullstack web applications. The user wants brutally honest feedback — praise what's genuinely good, grill what's weak, and recommend abandoning weak projects outright.

**Backend/frontend priority split (hidden constraint):** Projects are fullstack, but effort and attention follow a 3/4 backend : 1/4 frontend ratio. The user spends their time this way IRL, and the evaluation must reflect the same — 75% of scrutiny goes to backend depth, API design, data, architecture, testing, and production readiness. Frontend is noted but is not where time is spent digging deep. Do not inflate frontend critique; keep it proportional.

Use the framework below to judge.

---

## Gate 1: Is There a Valid Reason for This Project?

First filter. Kill the project immediately if it fails all criteria.

A project passes Gate 1 if it meets **at least one** of:

- **Solves a real problem for other users** — actual people (not hypothetical) would find this useful.
- **Solves a real problem for yourself** — you personally use this daily. Dogfooding is a strong signal.
- **Teaches something valuable for day-to-day job skills** — the process of building this taught you something directly applicable in a backend engineering role (e.g., learning how to design a rate limiter, understanding database indexing, building a CI pipeline).
- **Teaches a concept considered complex** — you built this to understand a genuinely hard topic (e.g., consistent hashing, distributed locking, real-time consensus, event sourcing). The concept itself is the point.

Flag as weak if:
- The project is generic (todo app, blog, chat app, e-commerce clone, tutorial project).
- The user cannot describe the purpose in one sentence without buzzwords.

**If Gate 1 fails:** Verdict is "This is a bad project. Build something else." Do not proceed further.

---

## Gate 2: Best Practices Audit

"Best practices" means two things: general backend conventions AND framework/library-specific idioms. A Next.js app should follow Next.js conventions (app router, server components, data fetching patterns). An Express app should follow Express conventions (middleware pattern, error middleware). A Spring Boot app should follow Spring Boot conventions (dependency injection, annotation-driven config, layered architecture). Same concepts, different idioms. Judge accordingly.

First, identify the frameworks/libraries used. Then check the relevant items below.

### Framework/Library Specific

Identify what frameworks and libraries the project uses (not limited to the examples below). Then check conventions specific to each one.

**Examples by framework (use these patterns to derive checks for whatever the project actually uses):**

*Next.js (App Router) — idiomatic checks:*
- Routes follow file-system conventions (`page.tsx`, `layout.tsx`, `route.ts`, `loading.tsx`, `error.tsx`)
- Server Components used where possible, `"use client"` only when needed
- Data fetching uses native `fetch` with caching/revalidation or server actions
- API routes are in `app/api/` with proper route handlers
- No unnecessary client-side state when server-side could work

*React + Vite — idiomatic checks:*
- Proper component composition (no giant components)
- Hooks follow rules of hooks (no conditional hooks, called at top level)
- State lifted where needed, kept local where possible
- No prop drilling beyond 2–3 levels (use context or composition)

*Express / Node.js — idiomatic checks:*
- Middleware chain is logical and ordered (auth → validation → handler → error)
- Error-handling middleware is the last middleware
- Async handler wrappers or a framework like Express 5 for async error catching
- Routes are in separate files, not all in `server.js`/`app.js`

*Spring Boot — idiomatic checks:*
- Layered architecture (controller → service → repository)
- Dependency injection via constructor injection, not field injection
- Proper exception handling with `@ControllerAdvice`
- Database access via JPA repositories, not raw JDBC everywhere

**General approach (apply to any framework):**
- Does the project follow the framework's recommended directory structure and conventions?
- Does it avoid anti-patterns specific to that framework? (e.g., mutating state directly in React, calling hooks inside loops, mixing server/client boundaries incorrectly in Next.js)
- Does it use framework-native features instead of reinventing them? (e.g., Next.js image optimization vs manual `<img>`, Express error middleware vs try/catch in every route)
- Does the code look like it was written by someone who understands the framework, not just copied from a tutorial?

### Backend (general)

- REST API conventions (plural nouns, proper status codes, consistent error shape)
- Input validation on every endpoint
- Proper auth: hashed passwords, JWTs/sessions, no custom crypto
- SQL injection protection: parameterized queries or ORM, no raw string concatenation
- Database migrations exist (no manual schema changes)
- Structured error handling (not crash-and-burn)
- Logging with levels (not just console.log)
- Environment config with secrets in env vars
- CORS configured properly (not wide-open)
- Rate limiting or basic abuse protection
- Tests exist (at least unit tests for core logic)
- Organized code — separation of concerns, not one giant file

### Frontend (1/4 share of attention, keep brief)

- State management is intentional
- API calls centralized
- Error and loading states handled
- No secrets in client code

Do not expand this section or add more frontend criteria. Keep frontend critique concise — note gaps but do not dwell.

### Project Hygiene

- README explains what, why, and how to run
- Clean dependency file (no unused deps)
- `.gitignore` exists and is correct
- Another developer can clone and run in under 5 minutes

**Gate 2 verdict:**
- 0–3 failures: Pass
- 4+ failures: Project is not resume-ready. Recommend the user fixes these first or builds a new project that gets them right from day one.

---

## Evaluation Criteria (Score 1–5 per dimension)

Follow the 3/4–1/4 priority split: backend dimensions (2–6) get thorough scrutiny. Frontend aspects are covered implicitly through completeness but do not get their own dimension or deep analysis.

| Score | Meaning |
|-------|---------|
| 1 | Missing / broken / embarrassing |
| 2 | Below average |
| 3 | Solid for a fresher |
| 4 | Strong, stands out |
| 5 | Exceptional (rare at this stage) |

### 1. Project Purpose & Value
- Which of the four Gate 1 criteria does this meet?
- If dogfooding: does the user use it daily? How often?
- If learning: what specific job-relevant skill or complex concept did they learn? Can they explain it?
- Is the purpose clear and defensible? Can it be described without buzzwords?

### 2. Backend Depth
- Beyond basic CRUD — are there real backend decisions (async processing, scheduled jobs, caching, webhooks, real-time events)?
- Database schema design — indexes, relationships, query optimization?
- Has the user thought about concurrency, race conditions, data integrity?

### 3. API Design & Quality
- Are endpoints intuitive and consistent?
- Error responses are useful, not just `{ "error": "Something went wrong" }`
- Pagination, filtering, sorting where applicable?
- Idempotency where it matters?

### 4. Code Quality & Architecture
- Clean layer separation (routes, controllers, services, data access)?
- No god classes or god functions?
- Readable code that doesn't need comments to understand?
- Would the user be comfortable pairing on this code in an interview?

### 5. Testing & Confidence
- Do tests exist? Unit tests for business logic?
- Do they test behavior, not just line coverage?
- Can they run in one command?

### 6. Production Readiness
- Can it be deployed and just work?
- Graceful error recovery (not crashing on bad input)?
- Basic monitoring / observability (even simple logging)?
- Performance — handles real usage without falling over?

### 7. Completeness
- Every claimed feature works end-to-end?
- No half-implemented pages, dead buttons, or `// TODO` in production code?
- Can it be demoed in under 2 minutes without hitting a bug?

---

## Scoring Table

Record scores and specific evidence for each. Dimensions 2–6 are backend-heavy and carry more weight in the final verdict.

| Dimension | Score (1–5) | Evidence |
|-----------|-------------|----------|
| 1. Project Purpose & Value | | |
| 2. Backend Depth | | |
| 3. API Design & Quality | | |
| 4. Code Quality & Architecture | | |
| 5. Testing & Confidence | | |
| 6. Production Readiness | | |
| 7. Completeness (includes frontend) | | |
| **Total** | **/35** | |

---

## Direct Verdict

Use one of three verdicts:

**"This is a bad project. Build something else."**
- Gate 1 failed (no real problem, not dogfooded)
- 4+ best practices broken
- Total score below 15/35
- User cannot explain one technical tradeoff

**"Fix this, then it's ready."**
- Core idea is solid but execution is sloppy
- 1–3 best practice gaps
- Score 15–25/35

**"This is resume-worthy. Put it front and center."**
- Meets at least one Gate 1 criterion strongly
- Score 26+/35
- Best practices all checked or nearly all
- User can discuss every design decision confidently

---

## Summary Output

| Area | Verdict |
|------|---------|
| Strongest point | |
| Biggest weakness | |
| One-sentence resume hook | |
| Gate 1 (valid purpose?) | Pass / Fail |
| Gate 2 (best practices?) | Pass / Fail |
| Final verdict | Kill / Fix / Keep |

---

## Gap Analysis & Next Project Directive

Identify which backend skills this project does **not** demonstrate:

- [ ] Real-time / WebSockets
- [ ] Background jobs / queues
- [ ] Caching strategy
- [ ] CI/CD pipeline
- [ ] Containerization (Docker)
- [ ] Vertical scaling / performance tuning
- [ ] Third-party API integration
- [ ] Complex database queries / reporting
- [ ] Auth flows (OAuth, SSO, MFA)
- [ ] File processing / uploads

Based on missing skills, output a next project recommendation:

```
Next Project:
- Gap it fills: [which missing skill(s) from above]
- Target scope: 3–4 weeks
- Must include at least 2 missing skills from above
- Must solve a real problem the user has
- One-sentence pitch:
```

Do not recommend the next project unless the current project passes Gate 1 and has at most 3 Gate 2 failures.
