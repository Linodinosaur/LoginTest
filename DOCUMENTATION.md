# DOCUMENTATION — Login Test

## Setup and installation

1. **Clone the repo** (or use your fork):
   ```bash
   git clone https://github.com/Linodinosaur/LoginTest.git
   cd LoginTest
   ```

2. **Supabase configuration**
   - Open your [Supabase Dashboard](https://supabase.com/dashboard) and select your project.
   - Go to **Project Settings** (gear) → **API**. You need:
     - **Project URL** (e.g. `https://xxxxx.supabase.co`)
     - **anon public** key (under “Project API keys” — this is the one safe for the browser; do **not** use the `service_role` key in frontend code).
   - In `index.html`, set the two constants at the top of the script:
     - `SUPABASE_URL` = your Project URL
     - `SUPABASE_ANON_KEY` = your anon public key  
   These are safe to commit to a public repo; Supabase RLS and Auth limit what the anon key can do.

3. **Enable Auth providers in Supabase**
   - **Authentication** → **Providers**: enable **Email** (sign up and sign in with email/password).
   - For Google: **Authentication** → **Providers** → **Google** → enable and add Client ID and Client Secret (see “Google OAuth setup” below).

4. **Run locally (optional)**  
   Serve the folder with any static server (e.g. `npx serve .` or open `index.html` via a server that serves from the project root). Supabase Auth works over HTTPS; for `file://` some flows may be limited, so a local server is recommended.

## Environment variables and configuration

- No build step or env files. Supabase **Project URL** and **anon key** are set directly in `index.html` (see Setup).
- **Never** put the `service_role` key in frontend code or in a public repo.

## Build and deploy

- **Build:** None. The project is static HTML + inline script.
- **Deploy (GitHub Pages):**
  1. Push the repo (including `index.html` with your Supabase URL and anon key) to GitHub.
  2. **Settings** → **Pages** → **Source**: Deploy from branch → Branch: `main` → Folder: `/ (root)` → Save.
  3. The site will be available at `https://<username>.github.io/LoginTest/` (or your repo name).

## Email confirmation link (GitHub Pages)

If the app is deployed at a path (e.g. `https://linodinosaur.github.io/LoginTest/`), the **verification email link** must point to that full URL, not to the root `https://linodinosaur.github.io`. Otherwise the link in the email will open the wrong page and auth may fail.

In Supabase **Authentication** → **URL Configuration**:

- **Site URL:** Set to the full app URL **including the repo path**, e.g. `https://linodinosaur.github.io/LoginTest/` (trailing slash optional). Do **not** use `https://linodinosaur.github.io` if the app lives at `/LoginTest/`.
- **Redirect URLs:** Add the same URL, e.g. `https://linodinosaur.github.io/LoginTest/**`, so Supabase allows redirects back to your app.

After saving, new confirmation emails will use the correct link (e.g. `.../LoginTest/#access_token=...`). The app also passes `emailRedirectTo` on sign-up (the current page URL) so the confirmation email link targets the same origin; ensure that URL is in **Redirect URLs** (e.g. `https://linodinosaur.github.io/LoginTest/**`).

## Google OAuth setup (optional)

To use “Sign in with Google”:

1. **Google Cloud Console**
   - Create or select a project → **APIs & Services** → **Credentials** → **Create Credentials** → **OAuth client ID**.
   - Application type: **Web application**.
   - Add **Authorized redirect URI**: from Supabase Dashboard → **Authentication** → **URL Configuration** copy the “Redirect URL” (e.g. `https://xxxxx.supabase.co/auth/v1/callback`) and paste it here.
   - Create; copy the **Client ID** and **Client Secret**.

2. **Supabase Dashboard**
   - **Authentication** → **Providers** → **Google** → Enable, paste Client ID and Client Secret, Save.

3. **Site URL**
   - In Supabase **Authentication** → **URL Configuration**, set **Site URL** to your app’s origin (e.g. `https://linodinosaur.github.io/LoginTest/` for this repo on GitHub Pages) so redirects after login go to the right place.

## Code conventions

- Single file (`index.html`); minimal inline CSS and one script block.
- Supabase client is created once; auth state is driven by `getSession()` and `onAuthStateChange()`.

## Error handling

- Auth errors (e.g. wrong password, email already registered) are surfaced via Supabase’s returned error; the script displays or logs them so the user can correct input. No custom backend; all errors come from Supabase client calls.

## Common pitfalls

- **Verification email link goes to wrong page:** If the link in the email looks like `https://username.github.io/#access_token=...` instead of `https://username.github.io/LoginTest/#access_token=...`, set **Site URL** in Supabase to the full app URL including the path (see "Email confirmation link" above).
- **Redirect after Google login:** If you get redirect loops or “site can’t be reached,” check Supabase **URL Configuration**: Site URL and Redirect URLs must match where the app is actually served (e.g. GitHub Pages URL).
- **CORS:** Supabase allows browser origins by default; for custom domains, configure in Supabase if required.
- **service_role key:** Do not use in frontend; it bypasses RLS and must stay server-side only.

## External documentation

- [Supabase Auth (JavaScript)](https://supabase.com/docs/reference/javascript/auth-api)
- [Supabase Auth with Google](https://supabase.com/docs/guides/auth/social-login/auth-google)
- [GitHub Pages](https://docs.github.com/en/pages)
