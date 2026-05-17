# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page **creator landing page** for a build-in-public operator who writes teardown analysis. Cream paper background, dark ink text, a thin dark status stripe at the very top, then a yellow ticker tape, then the document itself. The page is **funneling social-media traffic into newsletter signups** — that is its job. Everything else is structural support.

Mood: a printed terminal printout / case file. The visual language is Bloomberg-meets-dossier — it's on-brand for "I analyze businesses," and reinforces the writer's authority as a serious operator (not a hype creator). Yellow is the CTA color. Red is reserved for negative signals (LIVE dot, ✕ KILLED chip).

**It is NOT a portfolio or resume.** Do not re-introduce CV-style framings ("hire me," "available for freelance," skill lists). The works section ("In the lab") is a build-in-public log, not a project showcase.

## Information architecture

The page exists to convert. The IA is ordered for that, top-to-bottom:

1. **Status bar + ticker** — dark + yellow stripes pinned to the top. Real-time furniture (UTC clock, cosmetic reader/session counters) that sells the "live desk" frame.
2. **Hero** — name, **positioning lede** (what they do, who it's for), **pitch paragraph** (what the newsletter delivers), and the **inline subscribe form** (the primary CTA on the entire page).
3. **Authority strip** — "As seen on Code TV" + social channel links. No fake metrics, no fluff testimonials. If something here can't be backed up, remove it; don't soften it.
4. **/01 — In the lab** — table of current builds with status chips (`SHIPPING`, `BUILDING`, `PRELAUNCH`, `✕ KILLED`). Each entry is a startup the writer is building or has killed in public. The killed entries are load-bearing — they prove the writer ships *and* tells the truth.
5. **/02 — The briefing** — bigger newsletter pitch, what subscribers get (5-row bulleted list with yellow inline-chip labels), and a **second subscribe form**.
6. **Footer** — three columns (Desk / Channels / Contact) + EOF strip. Minimal, no recruiter packet.

## Single primary CTA

There are two visible subscribe forms (hero + briefing). They are the same form, repeated. Do not add competing CTAs ("buy the course," "book a call," "download the PDF") unless those things actually exist and the writer chooses to introduce them. Right now everything funnels to **email**.

The form posts to `#TODO-NEWSLETTER-ENDPOINT` — the user needs to swap this for their newsletter provider's form endpoint (Substack `/subscribe`, Beehiiv form URL, Kit / Buttondown / ConvertKit form ID, etc.).

## Theme tokens

Defined in `:root` at the top of `<style>`. Don't introduce ad-hoc colors.

| Token           | Value                      | Use                                                                       |
| --------------- | -------------------------- | ------------------------------------------------------------------------- |
| `--bg`          | `#E8DFCB` (= `--paper`)    | Page background (cream paper)                                             |
| `--text`        | `#1F1A14` (= `--ink`)      | Default text (dark ink)                                                   |
| `--text-soft`   | `rgba(31,26,20,0.62)`      | Soft ink — pitch paragraphs, summaries, footer captions                   |
| `--text-faint`  | `rgba(31,26,20,0.42)`      | Faint ink — table-header labels, list counter, label asides               |
| `--rule`        | `rgba(31,26,20,0.22)`      | Strong rule — table top/bottom borders, chip outlines, authority strip    |
| `--rule-faint`  | `rgba(31,26,20,0.10)`      | Faint rule — row separators, archived chip outline                        |
| `--dark-bg`     | `#1A1410`                  | Status bar background (the only dark surface on the page)                 |
| `--dark-text`   | `#F5EFE6`                  | Status bar text + the dark "FEATURED ON" chip text                        |
| `--red`         | `#D9342B`                  | Negative accents — LIVE dot, ▼ arrow, ✕ KILLED chip                      |
| `--yellow`      | `#F5C518`                  | Muted yellow — highlighter swipe under italic phrases                     |
| `--highlight`   | `#FFD400`                  | PRIMARY YELLOW — ticker bg, subscribe button, punch chips, hover, label bar |
| `--ink`         | `#1F1A14`                  | Same as `--text`; also text on yellow surfaces                            |
| `--teal`        | `#3B5363`                  | Reserved for charts (unused)                                              |
| `--paper`       | `#E8DFCB`                  | Same as `--bg`                                                            |
| `--manila`      | `#D9B779`                  | Reserved (unused)                                                         |

## Fonts (three, no more)

Loaded from Google Fonts via `<link>`. Do not add a fourth.

| Font                  | Weights                | Use                                                                            |
| --------------------- | ---------------------- | ------------------------------------------------------------------------------ |
| **Inter**             | 700, 900               | Punch chip, status-chip labels                                                 |
| **Playfair Display**  | 700, 900, italic 400   | Hero `h1`, lede line, briefing `h2`, italic blurb, project names in table      |
| **JetBrains Mono**    | 400, 700               | Status bar, ticker, pitch paragraph, section labels, table data, subscribe input, briefing list, footer, EOF strip |

## Architecture of `index.html`

Top-to-bottom:

1. `<head>` — meta, Google Fonts `<link>`, then `<style>` with: theme tokens, reset, status bar, ticker, hero, subscribe form, authority strip, section label, builds table, briefing block, footer, responsive + reduced-motion overrides.
2. DOM:
   - `.status-bar` (fixed top, 32px, dark) — LIVE dot + UTC clock + DESK label + cosmetic `READERS` + `SESSION`
   - `.ticker` (fixed below, 26px, yellow) — looped marquee with build-in-public copy
   - `<main>` — normal document flow under the fixed bars
     - `.hero` — `h1`, `.lede` (large display), `.pitch` (mono paragraph), inline `.subscribe` form, `.subscribe-note`
     - `.authority` — "As seen on" + a dark `.featured` chip linking to Code TV + `.socials` link row
     - `.builds` — `.section-label /01` + `.builds-table` (mono table with status chips)
     - `.briefing` — `.section-label /02` + `.briefing-head` (two-col grid: h2 + blurb / bulleted list with yellow inline-chip labels) + `.briefing-form-wrap` (second `.subscribe` form)
     - `<footer>` — three-column grid (Desk / Channels / Contact) + EOF strip with cosmetic build hash + blinking cursor
3. `<script>` at the bottom — live UTC clock, cosmetic readers/session/build-hash. No external JS.

### The subscribe form

The form is the same component used twice. It's a flex row with an ink border, a transparent input, and a solid yellow submit button (yellow is the CTA). On hover the button inverts to ink-on-yellow-text. The form posts to a TODO endpoint — there's no JS handler; relies on the newsletter provider's form action.

### Conventions to keep

- **Single-file, no build.** Resist Vite, bundlers, frameworks, or moving CSS/JS to separate files.
- **Three fonts only.** Inter + Playfair Display + JetBrains Mono.
- **One CTA: subscribe.** Do not add competing CTAs unless the writer actually has new offerings. If a paid course or community gets added later, it gets its own section *below* the briefing — the newsletter stays the headline CTA.
- **No fake authority signals.** Don't add testimonials, follower counts, or "as seen in" mentions for things that aren't real. The page can carry minimal authority — that's fine for an early-stage creator. The dishonesty would break the brand worse than the empty space.
- **Yellow is precious.** Primary yellow (`--highlight`) appears in: the ticker tape, the subscribe button, the hero `.punch` chip, the `.swipe` underline, the section-label bar, the ACTIVE/SHIPPING status chip, the row-hover tint + symbol-chip flip, the briefing-list `▸` arrow and inline `<strong>` chips, the footer-heading underline, the footer-link hover swatch, and the EOF cursor. If you add a new yellow surface, take one away first.
- **Red is for negative signals only** — LIVE dot, ▼ down arrows, ✕ KILLED chips. Don't paint general accents red.
- **Killed builds are a feature, not embarrassment.** The `chip-killed` row in the builds table proves the writer ships *and* tells the truth — load-bearing for credibility. Don't quietly remove the killed example.
- **The cosmetic terminal furniture (clock / readers / session / build hash) is just for vibe.** The clock is real UTC; everything else is randomized on load. Do NOT wire it to real audience metrics until the numbers are something the writer is genuinely proud to display.
- **Reduced motion disables the ticker scroll, the LIVE pulse, and the blink.** Honor it.
- **Responsive: at ≤760px, the builds table collapses to # / BUILD / link** (symbol, status, year columns hide). The hero scales aggressively via `clamp()`.

## Placeholders the user must fill

All marked `<!-- TODO -->` in `index.html`:

- **Name** — hero `h1`, status bar `YOUR NAME // DESK`, footer copyright
- **Positioning lede** — the one-sentence "what I do" in `.lede`
- **Pitch paragraph** — the newsletter sell in `.pitch`
- **Subscribe form action** — both `<form action="#TODO-NEWSLETTER-ENDPOINT">` instances must point to the newsletter provider
- **Code TV link** — the `.authority .featured a` href
- **Social URLs** — both in `.authority .socials` and in the footer `Channels` column
- **Build entries** — the four `<tr>` rows in the builds table (project name, summary, tags, status chip, year, link)
- **Contact email** — footer `mailto:`
- **Title tag** — `<title>Your Name — The Briefing</title>`

## Verification

Open `index.html` in a browser. You should see:
- Cream-paper page with a 32px dark status stripe on top (pulsing red LIVE dot, live UTC clock, cosmetic READERS + SESSION).
- A 26px primary-yellow ticker tape with build-in-public copy.
- Hero in dark ink on paper: large Playfair name → display-serif lede line with a yellow `TEARDOWNS` chip → mono pitch paragraph with a yellow-swiped phrase → inline subscribe form with a solid yellow Subscribe button → micro-note line ("free forever · written by a human · unsubscribe anytime").
- A thin authority strip with "AS SEEN ON" → a dark `★ Code TV` pill → social channel links pushed to the right.
- Builds table with status chips (yellow SHIPPING, outlined BUILDING/PRELAUNCH, red ✕ KILLED). Row hover tints yellow-amber and flips the symbol cell to a solid yellow chip.
- "/02 — The briefing" section: bold display headline + italic blurb on the left, mono bulleted list with yellow `▸` and yellow inline labels on the right, second subscribe form below them.
- Three-column footer in ink, each heading with a small yellow underline; link hovers swatch yellow-on-ink. EOF line ends with a yellow blinking block cursor.
- DevTools → emulate `prefers-reduced-motion: reduce` should freeze the ticker, the LIVE dot, and the blinking cursor.
