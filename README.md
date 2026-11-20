# Meekipedia — Academic Library & Learning Hub

Meekipedia is a responsive academic-resource discovery and study-support website built with semantic HTML, CSS, and vanilla JavaScript. The supplied black-and-gold stylesheet is retained unchanged. The site can be previewed without accounts; account-specific features require a Supabase project and the appropriate deployed Edge Functions.

## Current integration status

The app ships with blank Supabase browser configuration and no external API credentials. Open Library search is public and needs no API key. Signup/login, saved resources, history, profile, preferences, student metrics, admin metrics/management, AI responses, and YouTube search cannot operate until the corresponding Supabase account/service is configured. The UI reports those limitations and never invents sessions, analytics, saved records, or AI answers. Gemini and YouTube keys belong in Supabase Edge Function secrets, not in frontend code.

## Setup (in order)

1. Create a Supabase project and save its project URL and **anon/publishable** key.
2. In Supabase SQL Editor, run [`supabase/schema.sql`](supabase/schema.sql). It creates the supplied tables, triggers, RLS policies, ownership/admin checks, and owner-only admin SQL example. Review the SQL and confirm your project backup/change practices before executing it.
3. In Supabase Authentication settings, enable Email/password, choose whether signup requires email verification, and add redirect URLs for the local site and deployed site, including `/login.html` and `/reset-password.html` as needed. Configure recovery/confirmation email templates to return users to the matching site origin.
4. Copy `js/config.example.js` to `js/config.js` and replace only `supabaseUrl` and `supabaseAnonKey` with browser-safe Supabase project values. Never use a service-role key. The schema's RLS policies must remain enabled.
5. Deploy the Edge Functions below and configure their secrets in Supabase Functions settings. For each function set `APP_ORIGINS` to a comma-separated exact list of allowed website origins, e.g. `https://your-site.example,http://localhost:8000`. The defaults allow only local ports 8000. Do not add wildcards for production.
   - `ai-assistant`: add `GEMINI_API_KEY`; optionally set `GEMINI_MODEL` if your Google AI project uses another supported model. The function checks the signed-in Supabase user before calling Gemini.
   - `youtube-search`: add `YOUTUBE_API_KEY`. Restrict the key to YouTube Data API v3 in Google Cloud and monitor quota. The key is used server-side only. The function has in-memory best-effort request limiting; it is not a distributed abuse-prevention system.
   - Run `.\scripts\configure-supabase-secrets.ps1` after linking the Supabase CLI and filling `.env`. The script uploads the values as Supabase secrets; it does not place them in browser files.
6. Run the project from its root with a static HTTP server, for example `python3 -m http.server 8000 --bind 0.0.0.0`, then open `http://localhost:8000`. Supabase Auth redirect URLs must include this origin. Opening files with `file://` is not supported.
7. Create your own account through Signup and complete email verification if enabled. To assign the first administrator, replace the placeholder email in the optional query at the bottom of `supabase/schema.sql` and run that SQL only as the trusted project owner in Supabase SQL Editor. Never expose admin promotion in a public form.
8. Test with two student accounts and one manually assigned admin using the protocol in [`TEST_EVIDENCE.md`](TEST_EVIDENCE.md). Run the RLS checks from separately authenticated sessions. Do not assume that a hidden admin link is a security boundary.
9. Deploy the static files to Cloudflare Pages, Netlify, Vercel, or another static host. Configure the host's fallback/redirect behavior for the real `.html` pages, use HTTPS, add the production origin to Supabase Auth redirects and `APP_ORIGINS`, and keep `js/config.js` limited to the public Supabase values. Deploy Edge Functions independently. Do not commit private credentials.

## Pages

Public: Home, About, Login, Signup, Forgot Password, Reset Password, Search Results, Academic Library, Tutorials, Recommendations, and AI Academic Assistant. Student: Dashboard, Search History, Saved Resources, and Profile/Settings. Admin: Dashboard, User Management, Curated Resource Management, and Announcement Management. Every page shares the same responsive header, menu, and footer.

## Data and security

`supabase/schema.sql` from the build pack is copied without policy/schema changes. The browser obtains the current authenticated user from Supabase; queries supply that user's verified ID and are independently constrained by RLS. New users receive the student role from the Auth trigger. A profile role field is never present in the student profile form. The admin application confirms the database role before issuing admin queries; database RLS remains authoritative. Search, save, profile, and chat operations may fail if schema setup or RLS policies are incomplete. No credentials or passwords are stored in custom application tables.

Open Library results link to their catalogue records. They are discovery metadata, not a promise that the book is free to download. YouTube results link out to YouTube. AI is educational assistance only; verify its claims against course material.

## Project files

- `css/main.css`: supplied stylesheet, preserved as the theme source of truth.
- `css/responsive.css`, `css/app.css`: responsive and component extensions.
- `assets/brand-logo.png`: official Meekipedia logo from the supplied Stitch design bundle.
- `design-reference/stitch/`: provided screen references, example HTML, and Aureate Intelligence design guide; reference material only, not live application data.
- `js/config.js`: blank public Supabase configuration in this package; replace only with public URL and anon/publishable key after setup. Do not put secrets here.
- `js/config.example.js`: placeholder configuration template.
- `js/supabase.js`, `js/app.js`: browser client and page behavior.
- `supabase/functions/ai-assistant/index.ts`: authenticated Gemini proxy.
- `supabase/functions/youtube-search/index.ts`: server-side YouTube API proxy.
- `project-screenshots/`: genuine application captures and the screenshot index.

## Verification and known limitations

See [`TEST_EVIDENCE.md`](TEST_EVIDENCE.md) for what was actually checked in this delivery. Supabase authentication/RLS and Gemini/YouTube API success cannot be established without a configured owner project and credentials; these are explicitly called out rather than presented as passing. The Open Library API is an external service and can rate-limit or be unavailable.
