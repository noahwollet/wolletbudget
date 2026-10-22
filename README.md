# Wollet Family Budget — Secure shared edition

Static web app hosted on GitHub Pages, with Supabase Authentication and a shared Postgres database. Two individual email/password accounts share one household budget.

## Setup (about 15 minutes)
1. Create a free project at https://supabase.com/dashboard.
2. In **SQL Editor**, paste and run all of `supabase.sql`.
3. In **Authentication → Providers → Email**, keep email confirmation enabled. Configure your Site URL and Redirect URLs for your eventual GitHub Pages URL under Authentication → URL Configuration.
4. From **Project Settings → API**, copy the **Project URL** and **publishable (anon) key** into `config.js`. The publishable key is intended for browser use. **Never paste a service_role/secret key into GitHub.**
5. Upload `index.html`, `config.js`, `supabase.sql`, and this README to a GitHub repository. Enable GitHub Pages from the main branch root. For private financial apps, use a private repository if supported by your hosting setup, though the deployed static files remain public.
6. Open your GitHub Pages site. Create your account and confirm email if requested; sign in and choose **Create household**. Share the invitation code privately with your wife.
7. Your wife creates her own account, signs in, pastes the code, and selects **Join household**. Both now see the same data.

## Security and limitations
- Authenticated users can read only budgets belonging to their household. Writes use membership-checked RPC functions; other clients cannot write directly.
- The invitation code is a secret; share it privately. A household accepts at most two members.
- Changes synchronize via Supabase Realtime. Simultaneous edits are guarded by a version check; if a conflict happens, the app reloads latest data and asks you to retry. It does not automatically merge concurrent edits.
- Financial data is stored on Supabase servers, not encrypted end-to-end. Do not store account passwords, full credit-card numbers, or bank credentials.
- Payment calculations still use a simplified principal reduction and estimated APR/12 interest. They do **not** calculate exact statement interest or amortization. Update card minimum payments when statements change.
- Export and import backups remain available. Import replaces the shared household budget.
- No automatic bank connections or automated bill payments are included.
- `config.js` includes placeholders; the app cannot log in until you create and configure Supabase.
