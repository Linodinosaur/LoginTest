# PLAN — Login Test

## Phases (ordered)

1. **Bootstrap** — Create the six required project files and validate alignment with PRD.
2. **Auth app** — Implement `index.html` with Supabase URL and anon key, session handling, sign up / sign in / sign out, and “Hey you are logged in!” / “You are not logged in!”.
3. **Google OAuth** — Add “Sign in with Google” button and `signInWithOAuth`; document Google Cloud + Supabase provider setup in DOCUMENTATION.md.
4. **Deploy** — Document GitHub Pages (root of `main`) in DOCUMENTATION.md.

---

## Current phase: **Phase 4 — Deploy** (complete)

---

## Phase 1 — Bootstrap (complete)

- [x] Create PRD.md, ARCHITECTURE.md, PLAN.md, DOCUMENTATION.md, TIMELINE.md, AGENTS.md
- [x] Validate coherence with PRD and ARCHITECTURE

## Phase 2 — Auth app (complete)

- [x] Add `index.html` at repo root.
- [x] Include Supabase JS v2 via CDN; init client with Project URL and anon key (user fills in their values).
- [x] Implement `getSession()` on load and `onAuthStateChange()` to toggle UI.
- [x] Add sign-up form and handler (`signUp`).
- [x] Add sign-in form and handler (`signInWithPassword`).
- [x] Add Sign out button and handler (`signOut`).
- [x] Show “You are not logged in!” when no session; “Hey you are logged in!” when session exists.

---

## Phase 3 — Google OAuth (complete)

- [x] Add “Sign in with Google” button and `signInWithOAuth({ provider: 'google' })`.
- [x] In DOCUMENTATION.md: one-time setup for Google Cloud (OAuth client, redirect URI) and Supabase Dashboard → Authentication → Providers → Google.

---

## Phase 4 — Deploy (complete)

- [x] In DOCUMENTATION.md: document GitHub Pages from branch `main`, folder `/ (root)`.

---

## Dependencies

- Phase 2 depends on Phase 1 (docs and structure exist).
- Phase 3 depends on Phase 2 (auth page exists).
- Phase 4 depends on Phase 2 (deploy doc refers to `index.html` at root).

## Decisions

- **Anon key only:** No service_role key in frontend; safe to commit URL + anon key (see PRD and DOCUMENTATION).
- **Single file:** No separate CSS/JS files to keep the demo minimal and easy to copy.
