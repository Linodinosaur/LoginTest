# PRD — Login Test

## Problem statement

Need a minimal, working example of Supabase authentication (sign up, sign in, sign out, session state) in a single HTML page. The result will be a learning artifact and reference for integrating the same auth flow into Linos-ToDo-App.

## Target users

- Developer (you) as the primary user, to understand and replicate Supabase auth in another project.
- Anyone cloning the repo to learn or reuse the pattern.

## Core use cases

1. **Sign up:** Create an account with email and password.
2. **Sign in:** Log in with email and password (or with Google, if configured).
3. **Sign out:** Log out and return to unauthenticated state.
4. **Session persistence:** On load or refresh, the page correctly shows logged-in vs not-logged-in state.

## Functional requirements

- Single `index.html` at repo root, no build step.
- Sign-up form (email, password) and sign-in form (email, password).
- Optional “Sign in with Google” (OAuth) when Supabase + Google are configured.
- When not logged in: show “You are not logged in!” and the auth forms.
- When logged in: show “Hey you are logged in!” and a Sign out button.
- Use Supabase JS client (CDN); only Project URL and anon key in frontend (no service_role key).
- Deployable via GitHub Pages from the root of the `main` branch.

## Non-functional requirements

- **Security:** Only anon key and Project URL in client code; no secrets in repo beyond what Supabase considers public.
- **Simplicity:** No frameworks or build tooling; one file for the demo.
- **Documentation:** Setup and deploy steps documented so the flow can be replicated in Linos-ToDo-App.

## Out of scope

- Backend logic beyond Supabase Auth.
- Password reset, email verification flows (can be added later in the main project).
- Multiple pages or routing.
- Styling beyond minimal, readable layout.

## Success criteria

- User can sign up, sign in, sign out, and see the correct “logged in” / “not logged in” message after each action and on refresh.
- Repo contains the six required project files (per INSTRUCTIONS.md) and a single working `index.html`.
- DOCUMENTATION.md explains how to configure Supabase (URL, anon key, Email + optional Google) and how to enable GitHub Pages.
