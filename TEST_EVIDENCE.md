# Functional test evidence

**Run date:** 2026-11-14. The application was served from the project on port 3000 and tested in the live Sandbox browser/Preview capture path. This is evidence for the observed local implementation, not for a published site.

## Environment and limits

The supplied build pack contains schema SQL but no Supabase URL/anon key, Supabase Auth users, Gemini API key, or YouTube API key. `js/config.js` is intentionally blank. No test account or privileged credential was created. Therefore no authenticated Signup/Login/password-reset session, role authorization decision against a real profile, database write/read, saved-resource create/delete, dashboard metric query, or live RLS boundary has been reported as tested. No fake user or generated metric was used.

## Checks performed and observed outcomes

| Area | Method and actual observation |
|---|---|
| Supplied theme | `css/main.css` SHA-256 was `13dceff133232c3c4895081ea22148c80f21b0fa45ff5799b5440474747d1e1c`, matching the supplied stylesheet byte-for-byte. Its black-and-gold palette values were not changed. The added design layer uses the original theme variables for primary surfaces and gold accent. |
| Supplied Stitch UI | The supplied design guide, screens, and official logo were applied to the running website: centered landing/auth layouts, a desktop workspace rail, mobile layouts, and dark cards. Actual desktop and mobile page screenshots were captured after the restyle; see `project-screenshots/SCREENSHOT_INDEX.md`. |
| Screenshot inventory | The final screenshot folder contains 46 valid PNG captures across all six requested categories; each file is described in `project-screenshots/SCREENSHOT_INDEX.md`. Search-result captures were taken after the real Open Library response loaded. |
| Supplied schema | `supabase/schema.sql` compared byte-for-byte equal to the source schema. Static inspection found 10 application tables and RLS enablement on all 10. This was a source audit, not a live RLS execution. |
| Routes/manifest | `GET /manus-routes.json` returned HTTP 200 JSON with 19 declared page routes. The 19 declared HTML routes each returned HTTP 200 from the running static server. |
| JavaScript syntax | `node --check --input-type=module` completed without errors for the browser modules after the UI changes. |
| Edge Function TypeScript | `npx --yes -p typescript@5.8.3 tsc --noEmit -p supabase/functions/tsconfig.json` completed with no diagnostics. This checks source typing; it does not deploy or invoke a function. |
| Open Library search | A live `https://openlibrary.org/search.json` query for “computer networks” returned HTTP 200; the latest loaded live page reported 43,032 catalogue matches. The running website displayed actual catalogue results with authors, years, cover art/placeholders, and outbound Open Library records. |
| Filter state | A real application screenshot used query “computer networks” with publication year 2020; the filtered page showed no visible results and its empty-state text. The source may report broader catalogue totals, which do not imply that a visible first-result page contains a matching 2020 item. |
| Login form validation | In the live browser, an actual click on Sign in with the email field empty produced the implemented “Enter a valid email address.” message. The same state was rechecked after the Stitch restyle on 2026-11-14. No credentials were entered and no authentication attempt/session was created. The clean packaged validation screenshots predate the restyle; the browser's current screenshot export added target-outline overlays, so it was not packaged as a product screenshot. |
| Mobile navigation handlers | In the live browser DOM, invoking the actual menu button handler opened the sidebar and set its ARIA state; clicking the overlay closed it; Escape also closed it. This verified the handler state transitions, but not a physical phone or assistive-technology session. |
| Protected UI without service config | The current screenshots show student no-data and admin role-not-verified gates. These show no records/metrics/controls and do not establish a live Student-vs-Admin authorization test. |

## Blocked tests (not passed)

| Feature | Why it could not be tested here |
|---|---|
| Authentication | No Supabase project URL/public key or real test account was supplied. Signup, verified email, successful login, logout, reset email, and password update remain unverified. No successful-login screenshot is included. |
| Role-based authorization | Requires real Auth users and a database-created `profiles.role`. The site renders its no-verified-role gate when no Supabase configuration exists, but no student/admin allow-deny session was tested. |
| Saved resources/history/profile/personal recommendations | Requires a real Supabase session and schema. No resource was saved/removed and no personal records or analytics were read or written. |
| RLS isolation | Requires two independently authenticated student sessions and an owner-assigned admin. No direct SQL/API ownership or role-escalation attempt was run. A safe manual protocol is provided in `supabase/tests/RLS_TEST_PROTOCOL.md`. A frontend gate alone is not treated as RLS proof. |
| Gemini AI response | `GEMINI_API_KEY` and a configured/deployed Supabase Edge Function were not available. The app shows an unavailable state; no answer or AI-response screenshot is fabricated. |
| YouTube tutorial results | `YOUTUBE_API_KEY` and the deployed Edge Function were not available. The app shows an unavailable state; no tutorial API result is claimed. |
| Real student/admin analytics | No configured database or accounts; all metric values remain blank/not loaded. |

## Owner-side follow-up

After configuring the Supabase project, run the schema, configure email redirects, use two real student accounts and one owner-promoted admin, and execute `supabase/tests/RLS_TEST_PROTOCOL.md`. Record actual query results/codes here. Then deploy both Edge Functions with the respective provider secrets and test real Gemini/YouTube responses and failure modes. Do not treat the source/type checks or UI gate screenshots as substitutes for these tests.
