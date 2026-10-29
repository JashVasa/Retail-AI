# Kirana Ordering Assistant

WhatsApp-style ordering assistant for kirana stores. Hinglish message in, checked order out. Nothing is dispatched until the shopkeeper confirms the summary and total.

## How it works
1. **LLM (api/parse.js):** Claude turns the message into structured lines `{text, product, qty, unit, size}`. It never picks catalog products.
2. **Catalog matcher (index.html):** deterministic fuzzy retrieval over the 19-item catalog. Ties become follow-up questions. Unknown items are reported as "not in catalog" and never invented.
3. **Reorder nudge:** per-store exponentially weighted ordering frequency. Max 2 nudges, nothing added without a tap.
4. **Human checkpoint:** quantity sanity warnings, then a review sheet with the total. Only "Haan, order bhej do" confirms.
If `/api/parse` is unreachable (no key, static hosting), the page falls back to a rule-based parser and says so in the Demo controls.

## Deploy on Vercel (free)
1. Push this folder to a GitHub repo.
2. vercel.com > Add New Project > import the repo (no build settings needed).
3. Settings > Environment Variables: add `ANTHROPIC_API_KEY` (see `.env.example` for optional `MODEL`, `DAILY_LIMIT`, `PER_IP_PER_MIN`). Redeploy.
4. Open the link. Demo controls should show "Parser engine: Claude (LLM) + catalog matcher".
5. Open the Evaluation panel and press "Run the same tests with Claude" to get real accuracy and hallucination numbers for your report.

Local test: `npm i -g vercel && vercel dev` with a `.env` file copied from `.env.example`.

## Before going live
- Real order history replaces the synthetic data (the model currently partly learns its own generator).
- Rate limits here are in-memory per server instance. Use a shared store (e.g. Upstash Redis) and set a spending cap on the API account.
- Add authentication per store, and do not log phone numbers or order text without consent.
- Keep the test gold labels updated. The row "1 peti maggi ya 2 peti parle g" expects a follow-up, so a parser that correctly splits it into two lines is scored wrong by that row.

Never commit `.env` or paste the key into index.html.
