# GitHub Profile Revamp — Design Spec

**Date:** 2026-06-26
**Status:** Approved — implementing
**Repo:** `medsabbar/medsabbar` (the GitHub profile README repo)
**Owner:** Mohamed Sabbar (CTO at IBTIKAR Technologies)

## 1. Goal

Replace the current cliché-laden profile README with a lean, distinctive,
professionally-toned version that reads like a real CTO wrote it. The
centerpiece is the work: the national e-government platforms Mohamed builds —
**Houwiyeti**, **Khidmaty**, and **Aoun** — framed as the identity, services,
and social-safety-net layers of Mauritania's digital government.

It should:

- Lead with named, real, shipped projects (impact-forward, not aspirational)
- Read as a human (CTO) wrote it, not a template
- Load reliably — no dependency on third-party widget hosts that keep dying
  (Heroku free tier, Cyclic shutdown, flaky Vercel endpoints)
- Age well — minimal decoration, no trending widgets, no stat cards

## 2. Non-Goals (Scope Guard)

Explicitly out of scope:

- A separate portfolio website or blog
- Auto-syncing external content (essays, Spotify, WakaTime)
- A custom domain or GitHub Pages site
- Migrating or modifying any other repos
- Editing the GitHub profile sidebar metadata (name, bio, location, company,
  social links) — done in the GitHub UI, not in this repo
- Updating LinkedIn, Twitter, or other external profiles
- Stat cards / streak cards / activity graphs of any kind (dropped — see §4)
- GitHub Actions automation (dropped — there is nothing to regenerate)

## 3. Approach

**Static README + one hand-authored SVG banner.** Two files, no automation:

```
medsabbar/
├── README.md            # The profile rendered on github.com/medsabbar
└── assets/
    └── banner.svg       # Hand-crafted SVG hero banner
```

No GitHub Actions workflow, no generated assets. Everything either ships in the
repo or is a stable external link (app-store / gov portal). Nothing can break
because a widget host sunset.

Rejected approaches:

- **Self-hosted stats via Actions** (the earlier draft of this spec) — added a
  workflow file and two regenerated SVGs purely to power stat cards. Since the
  decision is now to drop stat cards entirely, the automation has no reason to
  exist. Removed.
- **Hosted stat cards** (github-readme-stats Vercel endpoint) — third-party
  host that's been flagged unreliable by its own maintainer. Rejected.

## 4. README Content

Target: 35–45 lines. Structure: banner → one-line intro → **What I'm building**
(the centerpiece) → stack → location footer.

```markdown
<div align="center">

![mohamed sabbar — CTO at IBTIKAR Technologies. "Building the web, one less-buggy app at a time."](./assets/banner.svg)

</div>

---

I'm Mohamed Sabbar — CTO at [IBTIKAR Technologies](https://www.ibtikartech.com),
where we build the digital public infrastructure Mauritania runs on.
Day to day I'm in Node.js, Next.js, TypeScript, MongoDB, and Firebase.

### What I'm building

Mauritania's digital government, layer by layer:

- **[Houwiyeti](https://play.google.com/store/apps/details?id=com.anrpts.houwiyeti)** —
  the national digital identity. Selfie-based eID that replaces passwords across
  government services and puts civil-registry documents (national ID, birth
  certificates, passports) in every citizen's pocket.
- **[Khidmaty](https://play.google.com/store/apps/details?id=mr.got.citoyens.platforme)** —
  the national e-services portal. One front door to public administrative
  procedures, integrated with Houwiyeti.
- **[Aoun](https://aoun.taazour.gov.mr/en)** —
  the social-support platform behind the national cash-transfer and food-aid
  program, reaching the country's most vulnerable families through the social
  registry.

### Stack

`Node.js` · `Next.js` · `TypeScript` · `MongoDB` · `Firebase`

---

<div align="center">
<sub>Nouakchott, Mauritania</sub>
</div>
```

### 4.1 What got removed (vs. current README)

| Removed | Reason |
|---|---|
| Typing SVG (`readme-typing-svg.herokuapp.com`) | Heroku-route, dated template |
| "🚀 Passionate Developer \| 💡 Innovation Driven \| 🎯 Solution Focused" | Pure cliché |
| `const medsabbar = { ... }` About Me code block | Universal template, recognized instantly |
| Shields badge wall (20+ badges) | Cut to 5 inline code tokens. A tech stack is not a personality. |
| "🎯 Current Focus" emoji bullets | Replaced by the real, named project list |
| Stats card, streak card, activity graph | Dropped entirely — no stat cards (decision) |
| "💭 Quote of the Day" | Noise |
| Profile view counter (komarev) | Past its peak |
| "Thanks for visiting! 😊" | Empty calories |

### 4.2 What got kept / added

- A hand-authored SVG banner (dark + cyan, monospace) — the only decoration.
- The three e-gov platforms, **named, linked, and framed by impact** (identity →
  services → social safety net). This is the centerpiece and the reason the
  profile reads as a serious CTO's.
- A link to the company (IBTIKAR Technologies).
- A tight 5-token stack line.

### 4.3 Voice and tone

- Direct, first-person, no hype words (no "passionate," "rockstar," "ninja,"
  "10x," "seasoned," "veteran").
- Specifics over adjectives: company, real platforms, real stack.
- Impact-forward but factual. Project lines describe what each platform *does*
  at national scale; no invented metrics. Hard numbers may be added later once
  the owner confirms publicly-citable figures.
- The playful tagline lives in the banner; the body stays substantive.

## 5. Banner SVG Spec

### 5.1 Dimensions and palette

- **Dimensions:** 1280×320 (standard GitHub banner ratio)
- **Background:** `#0d1117` (GitHub dark canvas), subtle gradient to `#0b0f14`
- **Foreground text:** `#e6edf3` (off-white), muted `#8b949e`
- **Accent:** `#22d3ee` (cyan) — left bar, shell prompt `❯`, short rule
- **Font:** `JetBrains Mono, SFMono-Regular, Menlo, Consolas, monospace`

### 5.2 Layout (top to bottom, left-aligned)

1. Cyan shell prompt `❯` + large `mohamed sabbar`
2. Muted subline `CTO · IBTIKAR Technologies`, with `@medsabbar` right-aligned in cyan
3. Short cyan rule
4. Tagline `Building the web, one less-buggy app at a time.`
5. Tiny muted footer `// Nouakchott, Mauritania` bottom-right
6. A faint cyan dot-grid texture for depth (very low opacity)

### 5.3 Production rules

- Pure SVG: no rasters, no external fonts, no JavaScript, no external image refs
- All styling via inline presentation attributes (no `<style>` block) so it
  renders identically through GitHub's camo proxy and across sanitizers
- Fixed positioning so layout holds across font fallbacks
- Hand-authored

### 5.4 Alt text

`mohamed sabbar — CTO at IBTIKAR Technologies. "Building the web, one less-buggy app at a time."`

## 6. Testing and Verification

Documentation/asset change — verification is visual + structural.

### 6.1 Structural checks

1. No `http://` URLs — only `https://` or scheme-less
2. Every image has alt text
3. No external JavaScript, no third-party widget hosts (only app-store / gov links)
4. No stat/streak/activity widgets
5. Total line count ≤ 45
6. Banner SVG is valid XML and self-contained (no external refs)

### 6.2 Manual visual checks

1. Banner renders at full size, no broken-image icon, in light and dark themes
2. Text wraps correctly at a 400px viewport (mobile)
3. No horizontal scroll at 1024px
4. All three project links resolve

### 6.3 Acceptance criteria

- [ ] Banner SVG renders correctly in light and dark GitHub themes
- [ ] Three named projects visible, linked, impact-framed
- [ ] No clichés from the current README remain (§4.1 cleared)
- [ ] No stat cards / widgets / third-party hosts
- [ ] Banner alt text present and descriptive
- [ ] ≤ 45 lines

## 7. Decisions Locked

| Decision | Value |
|---|---|
| Role in profile | CTO at IBTIKAR Technologies |
| Centerpiece | Named, linked, impact-forward e-gov projects |
| Projects | Houwiyeti (identity) · Khidmaty (services) · Aoun (social support) |
| Project links | Play Store (Houwiyeti, Khidmaty) · aoun.taazour.gov.mr (Aoun) |
| Scale numbers | None for now — descriptive; add real figures later |
| Tagline | "Building the web, one less-buggy app at a time." (in banner) |
| Hero style | Custom SVG banner, dark + cyan, monospace, shell-prompt motif |
| Stat cards / Actions | None — dropped entirely |
| Revamp aggressiveness | Full gut-and-rebuild |

## 8. References

- Current README: `README.md` in this repo
- Live GitHub profile: https://github.com/medsabbar
- Company: https://www.ibtikartech.com · https://github.com/IBTIKAR-Technologies
- Houwiyeti: https://play.google.com/store/apps/details?id=com.anrpts.houwiyeti
- Khidmaty: https://play.google.com/store/apps/details?id=mr.got.citoyens.platforme
- Aoun (Operation Aoun, Taazour): https://aoun.taazour.gov.mr/en
