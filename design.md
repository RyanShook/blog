# design.md — ryanshook.org

Working design spec for the landing page and matching blog refresh.
Reference style: [Superpower (Refero)](https://styles.refero.design/style/5d34568d-4bdc-445d-a527-c6f5249fa8fb)
— "clinical precision, vibrant pulse." High-contrast monochrome canvas with a single sharp accent.

> **Status: this doc tracks the as-built site.** When the design diverges from spec, the spec gets updated to match (this is a living document, not a frozen contract).

## 1. Positioning

> **Ryan Shook is a pastor and strategic AI advisor who partners with churches and Christian organizations to spread the gospel of hope and maximize their impact. He also works with mission-aligned nonprofits when there's a genuine fit.**

- Primary audience: church executive teams (mid-to-large multi-site churches), Christian-organization leadership, ministry tech leaders.
- Secondary audience: mission-aligned nonprofit ED/COO/CTO.
- Buyer's question: *"How do we put AI to work in ways that match our mission, not the other way around?"*
- Positioning words to lean into: **strategic, responsible, mission-aligned, pastoral, calm, coherent.**
- Words to avoid: hype, "revolutionize", "10x", "AI-first" as a slogan, generic "transformation."

### Voice
Direct, plainspoken, lightly technical, pastorally aware. Senior operator who's been in the room — not a vendor. Short sentences. No exclamation points. Use the second person ("your team", "your donors") on the landing page; first person on the blog and bio. No em dashes.

### Lead-gen posture
Site is **not** optimized for lead capture. No primary CTA in the nav. The closing CTA is a low-pressure "Start a Conversation" mailto. Trust beats funnel.

## 2. Information Architecture — Landing Page (`ryanshook.org`)

Single long page, six sections. Sticky header with name-mark + 3 anchor links + Blog. **No CTA in the nav.**

| # | Section | Background | Purpose |
|---|---------|------------|---------|
| 1 | **Hero** | Charcoal `#18181b` | H1 with vermillion-accented keyword + lede paragraph + small credit line + social icons row + headshot (4:5 portrait, capped 340px desktop). Logo strip (4 churches: Woodlands, Life.Church, Elevation, Saddleback) under "Churches I've worked with" sits inside the hero. **No CTAs in this section.** |
| 2 | **Mission** | White | Centered statement: *"I partner with churches and Christian organizations to spread the gospel of hope and maximize their impact."* Vermillion accent on "gospel of hope". One supporting sentence underneath. |
| 3 | **Strategic support every church needs** | Surface gray `#f4f4f5` | Subtitle "Four core competencies…" + 2×2 card grid: *AI strategy & responsible adoption · Donor development & retention · Accounting automation & insight · Digital ministry & engagement.* Each card: monochrome icon in a 12px-radius surface tile, H3, body. White cards on the gray section, subtle shadow. |
| 4 | **Speaking & teaching** | White | 2-col split (image left, content right). Heading + body + "Book Speaking" pill CTA → `mailto:`. |
| 5 | **Testimonial** | Surface gray | Centered pull quote on gray surface with 4px vermillion gradient bar at the top. Mono-styled cite ("TODD BURNS"). |
| 6 | **Final CTA** | Charcoal | "Start a Conversation" + "View LinkedIn Profile" — primary + ghost (on-dark). Subtle vermillion radial glow behind. |
| – | **Footer strip** | Charcoal | Single mono-font line: name + role · email · location. 1px white-on-dark hairline at top. |

### Section rhythm
`Hero (dark) → Mission (white) → How I help (gray) → Speaking (white) → Testimonial (gray) → Final CTA (dark) → Footer (dark)`.
**No explicit hairlines between sections.** Color alternation does all separating work; the only intra-section borders are inside the hero (logo strip top + bottom on dark) and at the top of the footer.

### Mobile reordering
On ≤720px the hero collapses to a single column. The headshot is **inserted between the H1 and the lede paragraphs** (via `display: contents` + flex order on `.hero-grid`). The desktop layout is text-left / portrait-right, untouched.

**Out of scope:** newsletter signup, pricing, services menu, dedicated case-study cards, About section, multi-column footer.

## 3. Information Architecture — Blog (Jekyll/Chirpy)

Goal: keep Chirpy's structure, reskin so the blog reads as the same brand as the landing page.

- Override Chirpy's CSS custom properties (light + dark) with the tokens below.
- Site title and post titles use Inter Tight; code in JetBrains Mono.
- Buttons → 9999px pill; cards / preview images / popups → 16px.
- Sidebar nav: pill chips on hover, charcoal-active state.
- Tag pills: outline → vermillion on hover.
- Code blocks: 16px radius, subtle shadow, JetBrains Mono.
- Post-list cards: soft shadow + lift on hover.
- Inline content links: vermillion underline that intensifies on hover.
- Footer line links back to `https://ryanshook.org/` ("← ryanshook.org").
- "Email" tab in sidebar (`fas fa-envelope`, order 5) redirects `/email/` → `mailto:contact@ryanshook.org` (managed via `_tabs/work.md` → `redirect_to:`).

## 4. Visual System (tokens)

### 4.1 Colors

| Role | Token | Value | Use |
|------|-------|-------|-----|
| Page bg | `--color-bg` | `#ffffff` | Default canvas |
| Surface raised | `--color-surface` | `#f4f4f5` | Section backgrounds, cards on white |
| Hairline | `--color-border` | `#e4e4e7` | Card borders, nav hairline |
| Hairline on dark | (literal) | `rgba(255,255,255,0.1)` | Footer top, logo-strip top/bottom |
| Text primary | `--color-text` | `#18181b` | Headings + body |
| Text secondary | `--color-text-muted` | `#71717a` | Eyebrows, metadata, captions |
| Accent | `--color-accent` | `#fc5f2b` | Headline keyword, CTA, key highlights — **one accent only** |
| Accent soft | `--color-accent-soft` | `#feaf95` | On-dark eyebrow, inline link underline |
| Accent gradient | `--gradient-accent` | `linear-gradient(88deg,#fc5f2b,#ff8b64 90%,#ffffff)` | Decorative bar at top of testimonial |
| Inverse bg | `--color-inverse-bg` | `#18181b` | Hero, final CTA, footer |
| Inverse text | `--color-inverse-text` | `#ffffff` | Type on inverse bg |

**Accent decision:** vermillion confirmed for the brand. Amber and teal alternates are no longer in consideration.

### 4.2 Typography

- **Display + UI:** [Inter Tight](https://fonts.google.com/specimen/Inter+Tight), weights 400 / 600 / 700.
- **Mono:** [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono), 400.
- Currently loaded via Google Fonts CDN (preview-grade). For production, self-host under `/assets/` and preload the display weight only.

Type scale (as built — all heads use negative tracking; this is what gives the brand its precision):

| Role | Size | Line height | Tracking | Where used |
|------|------|-------------|----------|------------|
| caption / eyebrow | 12px | – | 0.12em (positive, uppercased mono) | Eyebrows, footer |
| body | 17px | 1.6 | -0.011em | Default body, `.hero .lede` (19px variant) |
| credit | 15px | 1.55 | -0.013em | Hero credit line |
| heading-sm | 22px | 1.4 | -0.02em | Card H3 |
| heading | 30px | 1.4 | -0.018em | Testimonial blockquote |
| heading-lg | 45px | 1.2 | -0.024em | Section H2, mission H2 |
| display | 60px | 1.05 | -0.025em | Hero H1 (`clamp(40px, 6vw, 60px)`) |

Weight 700 for headlines. Weight 400 elsewhere. **No em dashes anywhere on the site** (use commas, parentheses, or periods).

### 4.3 Spacing & shape

- Base unit: **4px**. Scale: 4 / 8 / 16 / 24 / 32 / 48 / 64 / 96.
- Section vertical padding: **96px desktop / 64px mobile**.
- Element gap inside a section: **24px**.
- Container max-width: **1120px**, centered, **24px** gutter.
- Border radius:
  - Buttons → **9999px** (pill, required)
  - Cards / preview images → **16px**
  - Inputs / icon tiles → **12px**
- Shadows (whitelist — no other values allowed):
  - `--shadow-subtle: 0 2px 2px rgba(0,0,0,0.05)` — default card elevation.
  - `--shadow-lift: 0 8px 24px rgba(24,24,27,0.08)` — card hover.
  - `0 6px 16px rgba(252,95,43,0.32)` — primary button hover (vermillion glow).

### 4.4 Imagery

- Hero portrait: square professional headshot, 4:5 cropped via `object-fit: cover`, 16px-radius wrapper, subtle bottom gradient overlay.
- Section images (Speaking): 4:3 in 16px-radius wrappers, no shadows.
- Logos in hero strip: max-height 36px, transparent backgrounds, monochrome where the church publishes a white variant; original brand colors permitted when no monochrome version exists (currently: Elevation Church).
- Iconography: Lucide-style monochrome line art, 1.5–2px stroke, charcoal on light / vermillion accent in surface tiles.
- Illustrations: avoid.

## 5. Components

### Buttons
- **Primary** — `bg: --color-accent`, white text, pill, padding 14px 22px, weight 700. Hover: `0 6px 16px rgba(252,95,43,0.32)` + 1px lift.
- **Secondary outline (light)** — white bg, charcoal text, 1px border `--color-border`, pill, weight 700.
- **Secondary outline (on dark)** — transparent bg, white text, 1px white-30 border, pill. Hover: white fill, charcoal text.

### Cards (competency)
- White bg, 16px radius, 1px `--color-border`, 28px padding, `--shadow-subtle`.
- Hover: `translateY(-2px)`, `--shadow-lift`, border tints to `--color-accent-soft`.
- Icon tile: 44×44, 12px radius, surface bg, vermillion stroke icon (22×22).

### Logo strip (hero)
- Sits inside the hero band, below the lede + socials.
- Top + bottom hairline at `rgba(255,255,255,0.1)`.
- 4-column grid desktop, 2-col mobile.
- Eyebrow label "CHURCHES I'VE WORKED WITH" centered above.
- Logos at uniform 36px height; `.invert` filter (brightness(0) invert(1)) applied per-logo when source ships with a dark fill.

### Mission band
- White bg, **no hairlines**. Color alternation alone separates from the dark hero above and the gray Work section below.
- Centered, max 880px container, 96px vertical padding.

### Testimonial band
- Surface gray. 4px vermillion-gradient bar at top (decorative; the only "explicit" accent divider on the page).
- Centered pull quote, max 880px.
- Cite in JetBrains Mono, uppercased, muted.

### Nav
- Sticky, white with backdrop-blur, `--color-border` 1px hairline at bottom.
- Left: name-mark "Ryan Shook." (charcoal, weight 700, vermillion period).
- Right: charcoal weight-600 anchors (`Mission · Work · Speaking · Blog`). **No CTA pill.**
- Hover: vermillion text on surface-gray pill background.
- Mobile (≤720px): non-essential anchors hide; only the name-mark stays visible. (No hamburger sheet — small surface area.)

### Footer
- Single charcoal strip. Mono font, 13px, white-65 on charcoal.
- Top hairline `rgba(255,255,255,0.1)`.
- One line of copy: name + role · email · Houston, Texas. Wraps on mobile.

## 6. Live copy (current)

Treat this as the canonical copy for the page. Update in sync with `index.html`.

**Hero**
- H1: *Helping churches leverage AI to **unlock growth**.* (vermillion accent on the keyword)
- Lede: *Mission-driven organizations face real opportunities: deepening donor relationships, scaling operations, reaching people across more channels than ever. AI is changing what's possible in many of those areas but choosing the right tools, integrating them carefully, and protecting what's already working is challenging. I help churches and nonprofits of every size execute their AI strategy.*
- Credit (small): *Pastor and digital strategy lead at Woodlands Church, a multi-site organization across Texas. Fifteen years partnering with churches and Christian organizations to spread the gospel of hope and maximize impact.*
- Logo strip: "Churches I've worked with" → Woodlands · Life.Church · Elevation · Saddleback.

**Mission**
- *I partner with churches and Christian organizations to spread the **gospel of hope** and maximize their impact.*
- *Everything else (the strategy, the AI, the analytics, the systems) is in service of that. I also work with mission-aligned nonprofits when there's a genuine fit, but the heart of this work is the church.*

**How I help — section head**
- *Strategic support every church needs.*
- *Four core competencies I bring to churches and nonprofits. Each matters on its own; together they form a coherent strategy.*

**Speaking & teaching**
- *I regularly speak with church and nonprofit leaders on the practical realities of integrating AI into mission-driven work: what's been useful, what's been overhyped, and the questions worth asking before adopting new technology. Available for conferences, leadership retreats, board sessions, and staff workshops.*

**Final CTA**
- Eyebrow: *Get in touch*
- Headline: *Working through a hard problem?*
- Body: *If you're working on something real (donor retention, analytics, operations, digital engagement) and want a second set of eyes on the strategy, including whether AI belongs in the answer at all, I'm always glad to talk. Initial conversations are free and informal. No deck, no obligation.*

## 7. Implementation notes

- Landing page is a single-file build in `ryanshook.github.io/index.html`. Embedded `<style>`. Zero JS dependencies.
- Blog: Jekyll/Chirpy 7.x. Theme overrides via `assets/css/style.scss` after `@import "main"`. Don't fork.
- Fonts: currently Google Fonts CDN. Self-host before any meaningful traffic — the blocking external request hurts hero LCP.
- Performance budget: hero LCP < 1.8s on 4G; total JS < 30KB on the landing page.
- A11y: all CTAs ≥ 44×44 hit target; vermillion-on-charcoal-button passes AA. `display: contents` is used for the mobile hero re-order; supported in all current browsers.
- SEO: title, meta description, OG, Twitter card, Schema.org Person + ProfessionalService all aligned to the "AI advisor for churches and nonprofits" positioning. `link rel="canonical"` set.
- Favicon: charcoal SVG with white "R" + vermillion "S" mark at `/favicon.svg`.

## 8. Decisions resolved

- ✅ Accent color → vermillion `#fc5f2b`.
- ✅ Hero image → professional headshot (`images/ryan-headshot.jpg`, 1200×1200, ~190KB).
- ✅ Logo strip → Woodlands, Life.Church, Elevation, Saddleback. Label: "Churches I've worked with."
- ✅ Booking tool → plain `mailto:` for v1. No calendar embed.
- ✅ No nav CTA (site is not lead-gen oriented).
- ✅ No "How we work together" process section.
- ✅ No dedicated About section; bio collapsed into hero credit line.
- ✅ Em dashes removed; commas/parens/periods/colons used instead.
- ✅ Blog footer + sidebar tab redirect back to `ryanshook.org/` and `mailto:`.

## 9. References

- Reference style page: `https://styles.refero.design/style/5d34568d-4bdc-445d-a527-c6f5249fa8fb`
- Local copy of full token spec: `.firecrawl/refero-style.md` (gitignored).
