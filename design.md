# design.md — ryanshook.org

Working design spec for the landing page redesign and matching blog refresh.
Reference style: [Superpower (Refero)](https://styles.refero.design/style/5d34568d-4bdc-445d-a527-c6f5249fa8fb)
— "clinical precision, vibrant pulse." High-contrast monochrome canvas with a single sharp accent.

## 1. Positioning

> **Ryan Shook helps churches and non-profits of all sizes responsibly integrate AI into their workflows — to optimize the people experience and grow donor base and depth, tailored to their operating environment and audience.**

- Audience (in order): mid-to-large church executive teams; nonprofit ED/COO/CTO; ministry tech leaders; foundation program officers.
- Buyer's question: *"Can we adopt AI without compromising trust, mission, or our donors?"* The site exists to answer **yes, here's how, and here's the person who's done it.**
- Positioning words to lean into: **responsible, tailored, measurable, mission-aligned, calm.**
- Words to avoid: hype, "revolutionize", "10x", "AI-first" as a slogan, generic "transformation."

### Voice
Direct, plainspoken, lightly technical. Think: senior operator who's been in the room, not a vendor. Short sentences. No exclamation points. Use the second person ("your team", "your donors") on the landing page; first person on the blog and bio.

## 2. Information Architecture — Landing Page (`ryanshook.org`)

Single long page, 6 sections. Sticky header with name-mark + 3 nav anchors + primary CTA.

| # | Section | Purpose | Primary CTA |
|---|---------|---------|-------------|
| 1 | **Hero** | One-line positioning + 1 supporting line + primary CTA + secondary CTA. Full-bleed dark photographic background (people-in-context, not stock tech). Centered white display type. | "Book a working session" |
| 2 | **Who I work with** | 3-column grid: Churches · Nonprofits · Foundations. Each: 1 line of fit, 2-3 example outcomes. White canvas, light dividers. | (none) |
| 3 | **What responsible AI integration looks like** | 4 outcome blocks (people experience, donor base, donor depth, operational lift). Stat-style display number per block in vermillion. Light cards on whisper-gray section. | (none) |
| 4 | **How we work together** | 3-step process: *Listen → Tailor → Implement*. Numbered, plain. Anchors the consultative model. | "See a sample engagement" |
| 5 | **Proof** | Testimonial pull-quote on dark band + small logo strip ("trusted at" / partner orgs). Optional: linked case study cards. | (none) |
| 6 | **About + Final CTA** | Headshot + 4-line bio (Woodlands Church / NYT bestseller / 15+ yrs / writes at the blog). Email + booking link + writing link. | "Book a working session" / "Read the blog" |

**Out of scope for v1:** newsletter signup form, pricing, services menu. The single CTA is a booking/intro call. The blog is the secondary destination.

## 3. Information Architecture — Blog (Jekyll/Chirpy)

Goal: keep Chirpy's structure, but reskin so the blog feels like the same brand as the landing page.

- Replace the default Chirpy palette overrides with the tokens below.
- Site title font and post titles use the display family.
- Author card on each post points back to `ryanshook.org` and the booking CTA.
- Add a "Work with me" link to the blog's left sidebar that deep-links to the landing-page hero CTA.
- Tag the AI/non-profit posts ("ai", "nonprofit", "church-tech") so they surface as the implicit portfolio.

## 4. Visual System (tokens)

### 4.1 Colors

| Role | Token | Value | Use |
|------|-------|-------|-----|
| Page bg | `--color-bg` | `#ffffff` | Default canvas |
| Surface raised | `--color-surface` | `#f4f4f5` | Section backgrounds, cards on white |
| Hairline | `--color-border` | `#e4e4e7` | Dividers, card borders |
| Text primary | `--color-text` | `#18181b` | All headings + body |
| Text secondary | `--color-text-muted` | `#71717a` | Eyebrows, metadata, captions |
| Accent | `--color-accent` | `#fc5f2b` | Primary CTA, active nav, key stats — **one accent only** |
| Accent soft | `--color-accent-soft` | `#feaf95` | Card glow, hover tint |
| Accent gradient | `--gradient-accent` | `linear-gradient(88deg,#fc5f2b,#ff8b64 90%,#ffffff)` | Decorative band on hero CTA card |
| Inverse bg | `--color-inverse-bg` | `#18181b` | Hero band, testimonial band |
| Inverse text | `--color-inverse-text` | `#ffffff` | Type on inverse bg |

**Decision pending:** vermillion is correct for the energy this positioning needs, but verify it reads "calm + trustworthy" in the church/nonprofit context. Alternates to A/B if vermillion feels off: a deep amber `#d97706` (warmer, less "techy"), or a teal `#0d9488` (cooler, more institutional). Default = vermillion.

### 4.2 Typography

Reference uses NB International Pro (licensed). Substitute with the free + production-ready stack:

- **Display + UI:** [Inter Tight](https://fonts.google.com/specimen/Inter+Tight), weights 400 / 600 / 700. Use for everything except code.
- **Mono:** [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono), 400. For code blocks and metadata.

Type scale (minor third, 15px base):

| Role | Size | Line height | Tracking |
|------|------|-------------|----------|
| caption | 11px | 1.5 | -0.01em |
| body | 15px | 1.5 | -0.015em |
| subheading | 19px | 1.4 | -0.018em |
| heading-sm | 22px | 1.4 | -0.02em |
| heading | 37px | 1.25 | -0.022em |
| heading-lg | 45px | 1.2 | -0.024em |
| display | 60px | 1.06 | -0.025em |

Weight 700 for headlines and stat numbers. Weight 400 everywhere else. Negative tracking on all heading sizes — this is what gives the reference its "precise" feel and is non-negotiable.

### 4.3 Spacing & shape

- Base unit: **4px**. Use the 4 / 8 / 16 / 24 / 32 / 48 / 64 / 96 scale.
- Section vertical gap: **96px desktop / 64px mobile**.
- Element gap inside a section: **24px**.
- Container max-width: **1120px**, centered, 24px gutter.
- Border radius:
  - Buttons → **9999px** (pill — required, not optional)
  - Cards → **16px**
  - Inputs → **12px**
- Elevation: a single subtle shadow — `0 2px 2px rgba(0,0,0,0.05)`. No other shadow values.

### 4.4 Imagery

- Hero photography: real people in mission contexts (volunteers, congregations, working sessions). Shallow depth of field, warm/cool natural light. **No stock photos of robots, brains, or chip patterns.**
- Section images: square or 4:3 photos in 16px-radius wrappers, no shadows.
- Iconography: monochrome, 1.5px stroke, charcoal on light / white on dark. Lucide is the default set.
- Illustrations: avoid for v1.

## 5. Components

### Buttons
- **Primary** — `bg: --color-accent`, text: white, pill, padding 15px 22px, weight 700, tracking -0.015em. Hover: lift to `0 4px 8px rgba(252,95,43,0.25)`.
- **Secondary outline** — `bg: white`, text charcoal, 1px border `--color-border`, pill, padding 15px 19px, weight 400.
- **Ghost** — transparent bg, charcoal text, no border, pill, weight 400. For tertiary nav.

All buttons share the pill radius. No square buttons anywhere.

### Cards
- **Default** — white bg, 16px radius, subtle shadow, padding 26px 19px. Used for the "Who I work with" grid and outcome blocks.
- **Stat card** — same shell, but the stat number is `display` size in `--color-accent`, label below in `--color-text-muted` caption.

### Nav
- Sticky, white, `--color-border` bottom hairline at 1px.
- Left: name-mark "Ryan Shook" in heading-sm, weight 700.
- Right: 3 ghost-button anchors (Work · Writing · About) + 1 primary pill ("Book a session").
- Mobile: collapse to hamburger; menu opens to a full-width sheet with the same items stacked.

### Footer
- Charcoal bg, white text, 64px vertical padding.
- Three columns: contact / writing / social.
- Tiny mono-font line at the bottom: "© 2026 Ryan Shook · Built in public."

## 6. Sample copy (drafts, replace before ship)

- **Hero H1 (display, 60px):** *Helping churches and nonprofits adopt AI — without losing the plot.*
- **Hero subline (subheading, 19px, muted):** *I work with mission-driven teams to integrate AI responsibly into the workflows that touch your people and your donors. Tailored to your operating environment, not a vendor's roadmap.*
- **Primary CTA:** Book a working session →
- **Secondary CTA:** See how I work
- **Stat block headlines:** *People experience · Donor base · Donor depth · Operational lift*
- **Process steps:** *1. Listen — to your team, your tools, your audience.* / *2. Tailor — a plan that fits your environment, not a template.* / *3. Implement — alongside your staff, with measurable handoffs.*

## 7. Implementation notes

- Landing page lives in `ryanshook.github.io/` (separate repo/dir, currently a Pixelarity-style template at `index.html`). Plan: replace with a fresh single-file build, then progressively extract partials. Keep the existing structured-data block and meta tags — update copy only.
- Blog lives in this Jekyll/Chirpy repo. Reskin via `_sass/` overrides + custom `_sass/colors/` block. Don't fork the theme; use the supported override path so future Chirpy upgrades stay clean.
- Fonts: self-host Inter Tight + JetBrains Mono via the local `assets/` dir to avoid the Google Fonts request and keep the page fast. Preload the display weight only.
- Performance budget: hero LCP < 1.8s on 4G; total JS < 30KB on the landing page.
- A11y: all CTAs ≥ 44×44 hit target; vermillion-on-white passes AA at 17px+ — use charcoal text inside the pill button (already specified) for the primary CTA's contrast guarantee.

## 8. Decisions still open

- [ ] **Accent color**: vermillion (default) vs. amber vs. teal. Decide before any production work.
- [ ] **Hero image**: stock vs. commissioned vs. abstract gradient (no people).
- [ ] **Booking tool**: Cal.com, Savvy Cal, or a plain email link for v1.
- [ ] **Case studies**: do we have permission to name organizations, or use anonymized outcomes?
- [ ] **Logo strip ("trusted at")**: which orgs are in scope to display?

## 9. References

- Reference style page: `https://styles.refero.design/style/5d34568d-4bdc-445d-a527-c6f5249fa8fb`
- Local copy of full token spec: `.firecrawl/refero-style.md` (gitignored — regenerate via `firecrawl scrape` if needed)
