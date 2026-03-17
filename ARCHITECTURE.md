# ARCHITECTURE — Login Test

## High-level overview

Single static page; Supabase Auth is the only external service. The browser loads `index.html`, initializes the Supabase JS client with Project URL and anon key, and uses `getSession()` plus `onAuthStateChange()` to drive the UI.

```
┌─────────────────┐     HTTPS      ┌──────────────────┐
│  index.html     │ ◄────────────► │  Supabase        │
│  (GitHub Pages  │   Auth API     │  (Auth + optional│
│   or local)     │                │   Google OAuth)  │
└─────────────────┘                └──────────────────┘
```

## Core components

| Component | Responsibility |
|-----------|----------------|
| **index.html** | Markup, forms (sign up, sign in), status message area, Sign out and “Sign in with Google” buttons. Inline script: Supabase client init, `getSession`, `onAuthStateChange`, handlers for signUp, signInWithPassword, signOut, signInWithOAuth. |
| **Supabase JS (CDN)** | Client library for `supabase.auth.*`; handles tokens and session in memory/localStorage. |

## Data flow

1. **Page load:** Script runs, creates `supabase` client with URL + anon key, calls `getSession()`. If session exists → show “Hey you are logged in!” and Sign out; else show “You are not logged in!” and auth forms.
2. **Auth state changes:** `onAuthStateChange()` listener updates the visible UI (message + which forms/buttons are shown) on sign in, sign out, or token refresh.
3. **Sign up / Sign in:** Form submit → `signUp()` or `signInWithPassword()` → Supabase returns session → listener fires → UI switches to logged-in.
4. **Sign in with Google:** Button click → `signInWithOAuth({ provider: 'google' })` → redirect to Google then back to site → session restored → listener fires → UI switches to logged-in.
5. **Sign out:** Button click → `signOut()` → listener fires → UI switches to not logged-in.

## Technology choices

- **Supabase JS v2 (CDN):** Official client; no build step; supports Auth and (optionally) Google OAuth redirect flow.
- **Single HTML file:** Keeps the demo easy to copy into Linos-ToDo-App and run from any static host (e.g. GitHub Pages).
- **No framework:** Aligns with “incredibly simple” and avoids tooling; logic is inline in one file.

## Directory structure

```
/
├── index.html          # Single-page app and auth logic
├── INSTRUCTIONS.md     # Agent operating guide (existing)
├── PRD.md
├── ARCHITECTURE.md
├── PLAN.md
├── DOCUMENTATION.md
├── TIMELINE.md
└── AGENTS.md
```

## External integrations

- **Supabase Auth:** Email/password (signUp, signInWithPassword, signOut) and optional Google (signInWithOAuth). Redirect URL for OAuth must be allowed in Supabase Dashboard and (for Google) in Google Cloud Console.
- **GitHub Pages:** Serves the repo root (including `index.html`) over HTTPS; no server-side code.
