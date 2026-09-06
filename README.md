# Kiteframe deal calculator

A single-page, static HTML tool for talking through pricing and approval levels during an internal deal conversation. No backend, no data storage — everything runs client-side in the browser and resets on reload.

**All figures shown by default (e.g. $60,000 ARR, 10% commission) are fictional practice values from a training/course scenario. They are not Kiteframe's real pricing, real commission plan, or real approval policy.**

## Deployment

This is a single static file (`index.html`) with no build step and no server-side dependencies.

- **Quick local use:** open `index.html` directly in a browser.
- **Static hosting:** deploy `index.html` as-is to any static host (S3 + CloudFront, Netlify, Vercel, GitHub Pages, internal web server, etc.). No environment variables, no API keys, no backend required.
- **Internal-only distribution:** because it discusses discount authority and commission logic, treat it like any other internal sales-ops tool — host it behind whatever access control (VPN, SSO-gated static site, internal wiki embed) your org normally uses for pricing tools, rather than on a public URL.
- Fonts load from Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`) over HTTPS. If deploying in a network that blocks external font CDNs, the page still functions — it falls back to system sans-serif fonts.

## Inputs

| Input | What it means |
|---|---|
| First-year list ARR | The undiscounted annual subscription price for year one. |
| Discount % | The percentage discount applied to first-year list ARR. |
| Contract length (years) | Number of years in the subscription term. |
| Commission rate % | The rep's commission rate, applied to first-year ARR only. |
| One-time implementation fee | A separate, non-recurring fee. Never treated as ARR and never commissionable. |
| What we receive | A free-text field for the rep to record the business justification for the discount (e.g. "2-year term," "faster signature," "case study rights"). This is a documentation field only — it does not feed into any calculation. |
| AE authority threshold (editable) | The maximum discount % an AE can approve without escalation. |
| Manager authority threshold (editable) | The maximum discount % a Manager can approve. Anything above escalates to VP Sales. |

## Formulas

All calculations run live in the browser as you type. No values are hardcoded — they recompute from whatever is currently in the input fields.

- **First-year ARR after discount** = `List ARR × (1 − Discount%)`
- **Total subscription contract value** = `ARR after discount × Contract length (years)`
  - This assumes flat ARR every year of the term — it does **not** model renewal escalation, expansion revenue, or churn. If your real contracts step up or down in later years, this total will be inaccurate for multi-year deals.
- **Implementation fee** — shown as its own line, added to neither ARR nor total contract value, and excluded from every commission calculation.
- **First-year commission** = `ARR after discount × Commission rate%`
  - Calculated on ARR only. The implementation fee is never included in this figure.
- **Annual revenue given up** = `List ARR − ARR after discount`
  - The top-line dollar cost of the discount per year, independent of commission.
- **Commission lost vs. list price** = `(List ARR − ARR after discount) × Commission rate%`
  - The rep's own commission impact of the discount, not the company's revenue impact (see above for that).
- **Approval level:**
  - Discount ≤ AE threshold → AE can approve.
  - AE threshold < Discount ≤ Manager threshold → Manager approval required.
  - Discount > Manager threshold → VP Sales approval required.

## Assumptions a sales team must verify before using this with real deals

This calculator encodes a specific, simplified rule set. Before using it for anything other than practice, confirm each of these against your actual company policy — none of them are guaranteed to match your real business:

1. **Default ARR, commission rate, and thresholds are fictional practice numbers.** The $60,000 default ARR, 10% commission rate, 8% AE threshold, and 15% Manager threshold all come from a training scenario, not your real comp plan or discount policy. Update the defaults (or just overwrite them each time) before relying on this for real deals.
2. **Flat multi-year ARR assumption.** The tool assumes the same ARR repeats every year of a multi-year term. If your real contracts include price escalators, step-downs, or expansion, the "total subscription contract value" figure will not reflect that — verify separately.
3. **Commission basis.** This tool assumes commission is a single flat percentage applied only to first-year ARR, with the implementation fee always excluded. Confirm this matches your actual comp plan — real plans sometimes have accelerators, multi-year commission schedules, clawbacks, or different rules for one-time fees.
4. **Approval thresholds are a three-tier, discount-percentage-only model.** Real approval workflows may also depend on deal size, term length, non-standard terms, or specific contract clauses — not discount percentage alone. Confirm whether percentage-only escalation is actually how your approval process works.
5. **No data is saved or sent anywhere.** The "What we receive" field and all inputs live only in the browser session and are lost on refresh. If your process requires this justification to be logged (e.g. in a CRM or approval ticket), that step must happen manually, outside this tool.
6. **This tool does not check deal legality, contract terms, or discounting policy compliance.** It is a calculator, not an approval system — actual sign-off from the correct approver is still required based on your company's real process, regardless of what this tool displays.

## Support

This is a standalone practice/internal tool with no maintainer contact baked in. If your organization adopts it, assign an internal owner responsible for keeping the default assumptions current.
