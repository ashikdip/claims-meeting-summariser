# Claims Meeting Summariser

AI-powered meeting analysis tool built for the Nordic Claims Unit at If Insurance. Paste a meeting transcript, get back a structured summary with decisions, action items, and follow-up dates — instantly.

Built with the Claude API and styled using the HP design system.

---

## What it does

- **Summarises** meeting transcripts into 4–5 clear sentences
- **Extracts decisions** and tags each as Confirmed, Pending, or Deferred
- **Lists action items** with owner and due date in a clean table
- **Captures follow-up dates** mentioned in the meeting
- **Copy to clipboard** — each section individually or the full summary at once

Supports three meeting contexts: Claims Review, Executive Briefing, and Team Standup. Each context uses a tailored prompt to improve extraction accuracy.

---

## Tech stack

| Layer | Detail |
|---|---|
| UI | React (single-file SPA) |
| AI | Claude Sonnet 4 via Anthropic Messages API |
| Design | HP design system — Manrope font, Electric Blue `#024ad8`, Ink `#1a1a1a` |
| Auth | API key handled by the artifact proxy — no key input required for demo |

---

## Demo

Open `claims_summariser_final.jsx` as a Claude artifact to run the live demo. The Anthropic API is proxied automatically — no setup needed.

---

## Standalone deployment

To deploy independently (outside the Claude artifact environment):

1. Replace the direct Anthropic API call in `callClaude()` with a call to your own backend endpoint
2. Store `ANTHROPIC_API_KEY` as a server-side environment variable
3. Create a serverless function (Vercel / Netlify) that proxies the request

Example Vercel edge function (`/api/summarise.js`):

```js
export const config = { runtime: 'edge' };

export default async function handler(req) {
  const { system, user } = await req.json();
  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 1000,
      system,
      messages: [{ role: 'user', content: user }]
    })
  });
  return new Response(res.body, { headers: { 'Content-Type': 'application/json' } });
}
```

Then update the fetch URL in `callClaude()` from `https://api.anthropic.com/v1/messages` to `/api/summarise`.

---

## Project context

Built as a demo project for the **If Insurance AI Enablement Assistant** role (Nordic Claims Unit, Espoo). The goal was to show a working AI tool that connects directly to a real HR/claims workflow — not a slide deck about AI.

Related projects:
- [hr-desk](https://github.com/ashikdip/hr-desk) — AI-powered HR helpdesk, bilingual EN/FI, covers Finnish labour law (Vuosilomalaki)
- [nagad-recruitment-analytics](https://github.com/ashikdip/nagad-recruitment-analytics) — Recruitment funnel analytics dashboard (Python, SQL, Chart.js)

---

## Built by

**A H Ashik Ahmed**
[linkedin.com/in/ashikahmed11](https://www.linkedin.com/in/ashikahmed11/) · [github.com/ashikdip](https://github.com/ashikdip)
