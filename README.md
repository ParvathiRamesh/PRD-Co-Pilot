PRD Co-pilot — single-file live version
One HTML file. Live AI drafting. No deployment, no server, no Vercel.
How to run it
Open `prd-copilot-live.html` in a browser (double-click it).
Paste an Anthropic API key into the box at the top (get one at
https://console.anthropic.com → API Keys).
Type a product problem, click Generate PRD.
That's it. The agent gathers (simulated) context, drafts the PRD with a live
model, self-reviews and scores it, and asks for your approval before each
simulated write.
About the key (important, and worth telling anyone you share this with)
The key is typed into the page and kept in that browser tab's memory only.
It is not saved into the file, so the file itself is safe to share — it
contains no secret.
Each person who uses it enters their own key, billed to their own Anthropic
account.
The key is sent only to Anthropic (directly from the browser), nowhere else.
Close the tab and the key is gone; reopen and you re-enter it.
What's real vs. simulated
Real: the agent flow, the live-model drafting, the self-review score, the
approval gates.
Simulated: Jira / Confluence / analytics data, and the "writes" (labelled
`MOCK` — nothing is sent to a real system).
If you see an error
401 / authentication: the key is wrong or inactive — check it in the
Anthropic console.
Network / CORS / "Failed to fetch": some browsers or corporate
networks/proxies block direct browser-to-Anthropic calls. Try a different
browser or network. (This is the tradeoff of the no-server approach — the call
goes straight from the browser. If a particular environment blocks it, the
backend-deployed version avoids that, at the cost of needing deployment.)
429 / rate: usage or rate limit — wait, or check account credits.
Who this version suits
Best for you and any PM comfortable getting a personal API key. If you need
non-technical PMs to use it with no key step at all, that requires either the
simulated-drafting file (no live AI) or the deployed backend version (live AI,
one shared URL) — different tradeoffs, same agent.
