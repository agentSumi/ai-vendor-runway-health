# PRD — AI-Vendor-Runway-Health

**Slug:** `ai-vendor-runway-health`
**Lane:** tool-for-self (NAB vendor viability assessment)
**Source brief:** `~/second-brain-hermes/taro/weekly-whimsy/2026-11-14-brief-014.md` candidate #2
**Build type:** Single-page HTML calculator + embedded JSON + arithmetic JS (Taro writes directly, $0 OpenRouter)

---

## What it is

A single-page HTML calculator that takes AI vendor publicly-known financials and produces a one-glance health card: runway estimate, repricing risk indicator, ecosystem dependency score, and "what happens if the IPO flops" stress test. For NAB AI vendor procurement context.

## What it does (3-5 bullets)

- Computes runway per vendor: cash on hand / monthly burn → months remaining (with visual bar)
- Shows repricing risk indicator: how far below the last rumoured valuation the actual raise landed (OpenAI: $30B target → $11B actual = 63% shortfall)
- Calculates dependency score per vendor: how many ecosystem entities this vendor depends on (chipmaker, hyperscaler, single-customer concentration)
- Renders a stress-test card: "if Anthropic IPO prices at the low end, what's the cascade for dependent vendors"
- One screen, sortable health table, embedded JSON of known financials (no live API)

## How to verify it works (concrete success criteria)

| # | Criterion | Pass condition | How verified |
|---|---|---|---|
| 1 | Page loads at viewable URL | HTTP 200, no console errors blocking render | `curl -I <url>` returns 200 + browser open |
| 2 | ≥5 vendors shown with health cards | OpenAI, Anthropic, Cohere, Mistral, Inflection visible | DOM check + visual screenshot |
| 3 | Runway computed for each vendor | Numeric "months remaining" displayed per vendor | grep output for digit values |
| 4 | Repricing risk shows for OpenAI/Anthropic | "63%" (OpenAI shortfall) and "30%" (Anthropic haircut) visible | grep for "63" + "30" in served HTML |
| 5 | Dependency score ≥1 per vendor | Each vendor has a non-zero dependency count | DOM check |
| 6 | Stress-test card visible | "If Anthropic IPO prices at low end..." block renders | grep "Anthropic IPO" in HTML |
| 7 | House style: warm neutral canvas, ONE accent (ink-blue), bold sans | CSS contains expected tokens | grep hex values |
| 8 | MD5 of served file matches local | md5 match between local and remote | md5 command line |
| 9 | No external network dependencies | No CDN fonts/scripts | offline test |
| 10 | Single-file HTML | One `index.html`, all inline | `wc -l` + `ls` |

## Build approach

- **Executor:** Taro writes directly (write_file) — static HTML, no Codex needed
- **Build dir:** `~/taro-build/ai-vendor-runway-health/`
- **Deploy target:** Netlify (primary) → fallback GitHub Pages
- **Cost:** $0 (OpenRouter, Taro direct)
- **POC ceiling:** $0.30

## Reference-as-frame

Not applicable — tool-for-self, no appeal-library reference in play. Source is public financial data + Kim's NAB vendor-assessment context. Kim's house style is the look.

## Subtractive discipline (proposed cuts)

1. **Cut: no live data feed** — static JSON updated when raises are reported. POC synthesises the last known data point, not real-time.
2. **Cut: no sentiment analysis/NLP** — inputs are numbers (valuation, raised, burn, headcount), not text. Keep it arithmetic.

## Anti-pattern check

- Not another static HTML aesthetic object — has real interactive utility (numeric calculations, stress test, sortable table)
- Not a re-shape of brief-013 (NAB-Credit-Cycle-Dashboard was different — bank credit cycle, not AI vendor viability)
- Demand gate passed: named user (Kim), named use case (NAB AI vendor procurement), named catalyst (OpenAI repricing Nov 6, Anthropic IPO Nov 11)

## Eval verdict placeholder

To be filled after build: `## Eval verdict: PASS/FAIL per criterion`
