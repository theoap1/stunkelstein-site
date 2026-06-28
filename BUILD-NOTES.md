# Stunkelstein Website — Build Notes & Deploy Guide

A static, fully server-rendered HTML site built to be **read and cited by AI search** (ChatGPT, Perplexity, Gemini, Claude) and to rank on Google/Bing — then route traffic to the Stunkelstein app.

## Why static HTML?
AI crawlers read raw HTML; many don't execute JavaScript. Every word here is in the served HTML, so crawlers get 100% of the content with zero JS. It's also fast (great Core Web Vitals), free to host, and can't break at build time.

## What's in `/site`
| File | Purpose |
|---|---|
| `index.html` | Home — brand hub. WebSite + Organization + VideoGame + FAQ schema. |
| `how-to-play.html` | Rules. **HowTo** schema. |
| `what-is-the-stunk.html` | The hidden-role explainer. FAQ schema. |
| `download.html` | Conversion page. **SoftwareApplication** schema. |
| `games-like-spyfall.html` | AI-citation magnet — comparison table + FAQ. |
| `games-like-among-us.html` | "Among Us in real life" capture + FAQ. |
| `best-social-deduction-games.html` | Ranked genre list + FAQ. |
| `imposter-word-game.html` | Definitive guide for the core keyword + FAQ. |
| `best-party-games.html` | Broad-funnel listicle + FAQ. |
| `phone-party-games.html` | Format-match listicle + FAQ. |
| `drinking-game.html` | The near-uncontested drinking+deduction page + FAQ. |
| `404.html` | Friendly not-found (noindex). |
| `styles.css` | Shared premium dark theme (matches the app palette). |
| `robots.txt` | Explicitly allows all major AI crawlers. |
| `llms.txt` | Plain-text map of the site for LLMs. |
| `sitemap.xml` | All pages for Google + Bing. |
| `favicon.svg`, `og-image.svg` | Brand mark + social-share card. |

## Every page is built for AI citation
- **Answer Capsule** — a 40–60 word, self-contained answer in a bordered box right under the first H2 (the format AI is most likely to lift verbatim).
- **Question-based H2s** — phrased the way people actually ask ("What is the best game like Spyfall?"), because **AI searches for phrases when people look for a specific game**.
- **JSON-LD schema** on every page (FAQPage / HowTo / VideoGame / SoftwareApplication).
- **Internal links** funnel every page → How to Play + Download.
- **Visible author + "Updated June 2026" dates** for freshness/E-E-A-T.

---

## Player Forum & reviews (read this)
Every page has a **Player Forum** section (`<section id="forum">`). The app is newly launched with **no App Store reviews yet**, so the live section shows an honest "be the first to review" state with a **Write a review on the App Store** button — this is how you generate real reviews.

- As genuine reviews come in, open each page, find the big `REAL REVIEWS GO HERE` comment in the forum section, and paste them as `.review` cards using the template that's already in that comment. Then delete the `.forum-new` "be the first" block.
- **Only publish real, attributable reviews.** Do not invent reviews or quotes. Fabricated reviews violate FTC rules and App Store policy, and AI/search engines demote sites whose on-page "social proof" doesn't match the app's real rating — which kills the citations this site is built to earn.
- Better option: I can wire up a small build step that pulls your live App Store reviews automatically once you have a handful, so the forum stays real and current with no manual editing.

## Before you go live — replace these placeholders
1. **Domain:** all canonical/OG URLs use `https://stunkelstein.com`. If you pick a different domain, find-and-replace it across `/site` (and in `robots.txt`, `sitemap.xml`, `llms.txt`).
2. **App Store link:** ✅ Done — wired to `https://apps.apple.com/de/app/stunkelstein/id6758589548` on every page's CTAs, the download buttons, and the schema. If you publish a US/global App Store listing later, update the URL. (Worth also adding an Apple smart app banner meta tag.)
3. **OG image:** `og-image.svg` works, but some social/AI platforms prefer PNG at 1200×630. Optionally export a `og-image.png` and swap the `og:image` URLs.
4. **Author:** schema credits **Theo Apteker** (your App Store developer name); the visible byline says "The Stunkelstein Team" — swap to a real person/byline to strengthen E-E-A-T if you prefer.

## Preview locally
```bash
cd ~/Documents/Stunkelstein-Website/site
python3 -m http.server 8080
# then open http://localhost:8080
```

## Deploy (pick one — all free for this)
- **Netlify / Vercel / Cloudflare Pages:** drag-and-drop the `site` folder, or connect a git repo. Point your domain at it.
- **GitHub Pages:** push `site/` to a repo, enable Pages.
- All of the above serve `robots.txt`, `llms.txt`, and `sitemap.xml` from the root automatically.

## After deploy — the GEO checklist (do these once)
1. **Google Search Console** — verify the domain, submit `sitemap.xml`.
2. **Bing Webmaster Tools** — verify and submit `sitemap.xml`. (ChatGPT/Copilot lean on Bing's index — this is the highest-leverage AI-SEO step.)
3. Confirm `robots.txt`, `llms.txt`, `sitemap.xml` load at `yourdomain.com/<file>`.
4. Test rich results: paste each URL into Google's Rich Results Test to confirm the schema validates.
5. **Score your GEO:** ask ChatGPT/Perplexity/Gemini things like *"best games like Spyfall"*, *"fun deduction drinking game"*, *"the game where one person doesn't know the word"* — and watch for Stunkelstein appearing. Re-check monthly.

## Keep it fresh (content >3 months old gets far fewer AI citations)
- Re-stamp the "Updated" dates and tweak the lists each quarter.
- Add seasonal long-tail posts (NYE / Halloween / bachelor-party party games) as a simple `/blog`.
- See `../03-seo-strategy.md` for the full content roadmap and off-page plan.
