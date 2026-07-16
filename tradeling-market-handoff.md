# Tradeling Market Screen — Project Handoff

**What this is:** A React prototype (single file, `market-screen.jsx`) of the "Market" screen for a MENA B2B wholesale trading platform for consumer electronics (brand: Tradeling). Positioned as a trading-terminal-style tool, deliberately not e-commerce-styled, for a non-tech-savvy, fast-paced, busy user base.

**Attach `market-screen.jsx` (from outputs) to the new chat** so Claude can read/edit it directly — this doc is context, not a replacement for the file.

## Design system (don't relitigate unless asked)
- **Color rules, strictly scoped:** blue = selected/current state · orange (`--amber` #EF5323, real brand color) = actionable buttons only · gold (`--gold`) = live/ambient market signal, ticker only · green = success/positive, no action needed · red (`--red`, added late) = negative/closed (Rejected status) — each color has exactly one job, don't blend them.
- **Type scale:** was originally very small/dense (Bloomberg-terminal-inspired), deliberately enlarged later — labels 13px, data 15px, buttons 14px — because the real user base needs fast scanning, not dense-terminal parsing. Don't shrink it back down.
- **Gray tokens:** `--text-muted` (brighter, ~9:1 contrast) for all real readable text; `--text-faint` (dimmer, ~3:1) reserved *only* for decorative icons/dividers, never for text content. This distinction was a real bug once (mixed up), now fixed and enforced.
- **Fonts:** Space Grotesk (headings), Inter (body/labels), JetBrains Mono (all numbers/data, tabular).
- No emoji anywhere in the UI (flame icon used instead of 🔥 for "Hot cake").

## Business rules that shaped the design
- No pricing/bid visibility between customers — competitive sensitivity in wholesale CE market. "Selling Price" shown is the platform's own indicative reference, not derived from other bids.
- Bidding negotiation (counters) happens over WhatsApp, outside the platform. Only 4 bid statuses exist: **Placed, Rejected, Completed, Approved** (Approved = ready to checkout). No "Countered" status.
- Real per-product data (spec sheet: Model/Network/SIM/Market) is parsed live from product titles via a conservative keyword whitelist (`parseSpecsFromTitle`) — not hardcoded, so it scales to new products automatically. Only shows fields it can actually find; never fabricates.
- "Your Last Bid" card and the market "activity note" (bids in review / last bid / hot cake) are **mock/illustrative data** (deterministically hashed per product, not random) — clearly flagged as placeholders until My Bids / real backend exists.

## Data status (as of last session)
- 12 of 15 real product photos are embedded as base64 (no external hotlinking — CDN/sandbox loading was unreliable).
- **3 products still have broken/external image URLs, not yet replaced:** Samsung Galaxy S25 FE, Galaxy Tab A11 X133N, Galaxy Tab A11 X135G (originally hosted on noon.com, not provided in the last image upload batch).
- Two real bugs were found and fixed by testing, not just guessing — worth knowing the pattern: state (like image-load-failure tracking) wasn't being reset when switching between products, causing a failure on one product to incorrectly "poison" the next. Same class of bug happened twice (ticker rotation hash bias, and hero image fallback state) — check for this pattern if new bugs show up around per-product state.

## Open questions not yet resolved
- Nokia 125 product photo shows "MODEL:110 4G" and a smarter feature-phone design — doesn't match the basic 2G spec (TA-1655). Filename matched exactly what was given, so it was used as-is, but flagged as possibly the wrong photo.
- No real spec data (connectivity, battery, platform) exists yet for non-phone categories (PlayStation, Loewe) beyond what's parseable from titles — decided *not* to fabricate this; left as a gap pending real data.
- My Bids and Orders screens haven't been built yet — only Market exists so far.

## How this conversation worked (process notes for continuity)
- Every change was validated with `esbuild` syntax checks and, where behavior mattered (not just styling), verified with a headless Chrome test harness (Puppeteer) rather than assumed — this caught several real bugs before shipping. Worth continuing this discipline for anything involving state, animation timing, or data correctness.
- The user pushed back productively multiple times (on assumptions, on visual details, on scope) — worth continuing to flag assumptions explicitly rather than silently picking a default when something is genuinely ambiguous.
