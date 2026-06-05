# PRD Co-pilot — agentic version (static Vercel deploy)

One HTML page, hosted on Vercel as a static site. A live model drives a real
agent loop. Each user pastes their own Anthropic API key.

## What this version is
This is the **agentic** build. A live model:
- **decides each next step** (search Jira, read style, get metrics, ask, draft,
  review, finish) rather than following a fixed script,
- **gathers context**, then **drafts** the PRD,
- **reviews itself repeatedly** until the rubric score clears the target (up to 3
  passes), and
- **stops for your approval** before each simulated write.

Same UI and safety model as the simpler "live" build — the difference is the
reasoning loop underneath, which is what makes it a genuine agent rather than a
pipeline with AI in the middle.

## Deploy to Vercel (static — no backend)

You need a free Vercel account and the contents of this `prd-copilot-static`
folder (`index.html`, `vercel.json`).

### Route A — GitHub + Vercel dashboard (no terminal)
1. Create a new repo at <https://github.com/new> (private is fine).
2. On the repo page: **Add file → Upload files**, drag in `index.html` and
   `vercel.json` (keep them at the top level), and commit.
3. Go to <https://vercel.com/new>, import the repo, **Framework Preset: Other**,
   leave build settings empty, click **Deploy**.
4. Open the URL it gives you (e.g. `https://your-project.vercel.app`). Done —
   there is no key or environment variable to set, because the key is entered by
   each user in the page.

### Route B — Vercel CLI (terminal)
```
npm i -g vercel
cd prd-copilot-static
vercel --prod
```
The CLI prints your live URL.

## How to use it (and what to tell PMs)
1. Open the URL.
2. Paste an Anthropic API key into the box at the top (get one at
   <https://console.anthropic.com> → API Keys).
3. Type a product problem, click **Generate PRD**.

## About the key (important — this is a per-user-key deployment)
- The key is typed into the page and kept **in that browser tab's memory only**.
- It is **never** stored on the server or written into the page — hosting this on
  Vercel does **not** put a key anywhere shared. The site is static; there is no
  backend holding a key.
- Each person who uses it enters **their own** key, billed to **their own**
  Anthropic account.
- The key is sent only to Anthropic, directly from the user's browser.
- Close the tab and the key is gone.

Because the key is per-user, the public URL is safe to share — no one can spend
against your account through it. (Trade-off: every PM needs their own key.)

## What's real vs. simulated
- **Real:** the agent loop (model decides each step), live-model drafting, the
  iterative self-review and score, the approval gates.
- **Simulated:** Jira / Confluence / analytics data, and the "writes" (labelled
  `MOCK` — nothing is sent to a real system).

## Cost & behaviour notes
- The agent makes **several model calls per run** (one per decision, plus drafting
  and each review pass — roughly 7–10 total), billed to whoever's key is in use.
  Still cents per run, but more than the simpler build.
- The loop is **model-driven, so the path can vary** run to run. That's the point
  (it's reasoning, not a fixed script), but it means two runs of the same problem
  may differ slightly. Run it once before any live demo so you know what to expect.

## If you see an error
- **404 / model:** the model name in the page is out of date — update the
  `const MODEL=` line near the top of the `<script>` to the current model.
- **401 / authentication:** the key is wrong or inactive — check it in the
  Anthropic console.
- **Network / CORS / "Failed to fetch":** some browsers or corporate
  networks/proxies block direct browser-to-Anthropic calls. Try a different
  browser or network. This is the trade-off of the static (no-backend) approach;
  the backend-deployed version avoids it, at the cost of needing a server and a
  shared key.
- **429 / rate:** usage or rate limit — wait, or check account credits.

## Who this version suits
Best for you and PMs comfortable getting a personal API key. If you later need
non-technical PMs to use it with **no key step at all**, that's the backend
deployment (`prd-copilot-web`), where the key lives safely server-side and there's
one shared URL — same agent, different hosting trade-off.
