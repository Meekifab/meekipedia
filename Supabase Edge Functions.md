# Supabase Edge Functions

Deploy from the project root using Supabase CLI after linking this folder to your project:

```sh
supabase functions deploy ai-assistant
supabase functions deploy youtube-search
supabase secrets set APP_ORIGINS="https://your-site.example,http://localhost:8000"
supabase secrets set GEMINI_API_KEY="<AQ.Ab8RN6Jo3cgLwSTxV1XpJC5WRsqqyVoJjh1NPT0cuzzJ83BbDw>"
supabase secrets set YOUTUBE_API_KEY="<AIzaSyCbNl5zxrD9K4q1rYIUk2PX7eVEsWFHXMo>"
```

Set only the secrets for functions you actually deploy. Never place them in `js/config.js`. The Gemini function requires a signed-in Supabase user and allows at most ten requests per user per warm instance per minute; its in-memory throttle is best-effort, not a durable global rate limiter. The YouTube function uses a best-effort per-IP limit and can be publicly invoked through the public Supabase key, so restrict Google API key usage, monitor quota, and consider a durable gateway limiter before broad public deployment. `APP_ORIGINS` must list exact HTTPS site origins (comma-separated, no trailing slash). Local defaults allow `http://localhost:8000` and `http://127.0.0.1:8000`.

The Gemini function returns provider errors rather than a fabricated answer. It stores no chat history itself; the browser writes actual user and assistant messages into the supplied tables under RLS. The YouTube function returns only IDs, snippet metadata, thumbnails, and outbound YouTube URLs; it does not download or embed video content.
