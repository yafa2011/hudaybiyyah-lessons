# Hudaybiyyah Lessons: website you can install on phones

Files
- index.html: the page (works as an installable web app)
- manifest.webmanifest, sw.js, icon-192.png, icon-512.png: make it installable ("Add to home screen")
- functions/api/analyze.js: the small server piece. It keeps your AI key secret and limits usage.

## How it runs
Student -> index.html -> /api/analyze (your server piece) -> AI service -> back to the student.
The AI key lives only on the server side, never in the page.

## Before you start (about 30 to 45 minutes, no coding)
You need three free accounts:
1. GitHub (stores the files)
2. Cloudflare (free hosting and free usage counter)
3. An Anthropic API account (the AI itself). This is NOT free: you add a small amount of credit and every try costs a little. Check current pricing on Anthropic's site before you launch.

## Steps
1. On GitHub, create a new repository and upload everything in this folder (keep the functions/api folder structure).
2. In Cloudflare, create a new Pages project and connect that GitHub repository. Leave the build command empty and the output folder as the root ("/" or blank).
3. In Cloudflare, create a KV namespace (name it anything, for example "limits").
4. In your Pages project settings, add:
   - a KV binding with the variable name exactly: LIMITS (pick your namespace)
   - a secret environment variable named ANTHROPIC_API_KEY with the key from your Anthropic account
   - optional: an environment variable MODEL. The default is claude-haiku-5-5 (cheaper). For better Arabic quality you can set claude-sonnet-5-5 (costs more).
5. Redeploy the project so the settings apply, then open your site and test.
6. On a phone: open the site in Chrome (Android) or Safari (iPhone), then choose "Add to home screen" (Safari: Share, then Add to Home Screen).

Cloudflare changes the names and places of its screens fairly often. If a step does not match what you see, check Cloudflare's current Pages documentation for "Functions" and "KV bindings".

## Protect your budget
In functions/api/analyze.js:
- DAILY_LIMIT: tries per person per day (now 5)
- GLOBAL_DAILY_CAP: total tries per day across everyone (now 300). When it is reached, the app says it is busy until tomorrow. Set it to what you can afford.
Also set a spending limit in your Anthropic account if it offers one.

## Honest limits of this version
- The per-person limit is by network address. People on the same Wi-Fi share a count, and someone can get around it by changing network. Real sign-in is the next upgrade.
- The counter is not perfectly exact when many people click at the same moment. That is fine for a prototype.
- Check the history in the answers against your school lesson before students rely on it.
