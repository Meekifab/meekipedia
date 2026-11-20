# Meekipedia — implementation plan

## Product and implementation

Deliver the complete academic library and tutorial recommendation website from the attached build pack as a maintainable static multi-page site built with semantic HTML, CSS, and vanilla JavaScript ES modules. Use the included homepage sample for layout and preserve the exact `main.css` palette, header, slide-out menu, hero, cards, spacing, typography, responsive intent, and interaction style. The supplied `supabase/schema.sql` is the database contract. Supabase Auth, PostgreSQL, and RLS provide real identity and persistent user/admin data; client configuration contains only the public URL and anon key. No database account, role, metric, saved item, AI response, or successful authentication is simulated. When credentials or a service are missing, pages show explicit unavailable/empty/error states and protected routes fail closed.

Open Library's public Search API supplies academic book results. Gemini and YouTube are called only through Supabase Edge Functions, with secrets stored as Edge Function secrets and never shipped to the browser. Recommendations use explainable subject/interest/activity matching, not claims of machine learning. Include all required pages, responsive navigation, form validation, accessible labels/focus, status messages, destructive confirmations, configuration examples, source SQL, Edge Function code, setup/deployment instructions, test evidence, and genuine screenshots captured from the running implementation.

Apply the user-supplied Stitch Meekipedia UI bundle as the detailed visual reference, using its `aureate_intelligence/DESIGN.md`, screens, and official logo. Keep the existing functional implementation and data states rather than copying generated mock data. Public landing uses a centered hero, gold primary actions, and four feature cards; account forms use a compact centered dark card; learning, student, and admin routes use a fixed charcoal left navigation rail on desktop and the existing accessible drawer/tabs at mobile breakpoints. Use Inter for main typography and JetBrains Mono sparingly for labels/metadata, layered dark charcoal surfaces, subtle gold borders, and clear gold active states. Add the supplied official logo to the shared shell. Retain the previously requested panoramic library photo as a near-black-darkened shared background, while ensuring cards and page surfaces keep the reference's strong contrast and the original theme variables remain intact.

## Structure

- `index.html`, `about.html`, `login.html`, `signup.html`, `forgot-password.html`, `reset-password.html`: public landing, information, and authentication flows.
- `pages/`: library, tutorials, recommendations, and AI assistant.
- `dashboard/`: student overview, search history, saved resources, and profile/settings.
- `admin/`: admin analytics, users, curated resources, and announcements/management.
- `css/main.css`: the supplied authoritative stylesheet, preserved as base; `css/responsive.css` and `css/app.css` add layout and component details without replacing its palette.
- `js/`: shared navigation, configuration, Supabase client, auth/session guards, and feature-specific ES modules.
- `supabase/schema.sql`: supplied RLS schema; `supabase/functions/` contains secure Gemini and YouTube proxies; `supabase/tests/` documents real two-student/admin RLS verification.
- `assets/`: official logo from the supplied Stitch bundle, supplied icon, and small UI assets as applicable.
- `design-reference/stitch/`: user-provided Stitch screens, HTML source examples, and Aureate Intelligence design guide; kept as implementation reference, not used as page mockups.
- `project-screenshots/`: authentic runtime PNG captures organized in the requested six groups; `SCREENSHOT_INDEX.md` explains each image.
- `README.md`, `TEST_EVIDENCE.md`, `TODO.md`: setup, evidence, and completion tracking.

## Design

**Design Movement:** restrained dark editorial interface with the supplied black-and-gold design as the controlling reference.

**Core Principles:** preserve the supplied visual language; favor legible, calm academic discovery; make system state and source attribution explicit; secure private actions before rendering personal data.

**Color Philosophy:** keep the original near-black canvas and charcoal cards for focus, use the specified gold only for emphasis and action, and retain the supplied white/light-grey/muted text hierarchy. No palette substitution or new accent scheme.

**Layout Paradigm:** editorial landing hero followed by wide, breathing feature/process sections; compact, information-dense resource cards and a responsive side navigation for application pages.

**Signature Elements:** a gold-edged wordmark with a small open-book mark; fine gold hairlines and restrained glow on dark surfaces; consistent resource-type icon tiles.

**Interaction Philosophy:** clear direct actions, immediate inline feedback, visible loading/empty/error states, keyboard-operable navigation and forms, and confirmation before destructive changes.

**Animation:** keep supplied short transitions for hover/menu; avoid gratuitous motion, honor `prefers-reduced-motion`, and never rely on animation to communicate status.

**Typography System:** preserve the supplied Arial/Helvetica sans-serif foundation; use a bold, tightly tracked display heading scale, normal high-legibility body copy, and compact muted metadata. Do not introduce a new downloaded font.

**Brand Essence:** a single place for students to discover academic books, tutorials, and responsible AI study support; focused, practical, transparent.

**Brand Voice:** direct, study-oriented, and explicit about limitations. Examples: “Find the next useful source.” “Study the concept, then verify it against your course materials.”

**Wordmark & Logo:** custom simple book-spine glyph and “Meekipedia” wordmark using gold/white in the existing asset treatment, not an unrelated logo.

**Signature Brand Color:** the supplied Meekipedia gold `#d4af37`.

## Dependencies and boundaries

The frontend requires a configured Supabase project URL and public anon/publishable key to exercise authentication, saved resources, preferences, history, or analytics. Gemini and YouTube proxy calls require Edge Function deployment and the corresponding secrets. Open Library needs no API key. No service-role, database, Gemini, or YouTube secret is included in browser configuration. The supplied project does not include external credentials, so tests requiring them will be reported as blocked rather than represented as passing. Do not deploy/publicly publish unless explicitly requested; provide the source and requested ZIP.
