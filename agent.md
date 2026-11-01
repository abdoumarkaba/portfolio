# AGENT.md — Build Abdou's Portfolio Site

**Executor:** a fast, free-tier implementation model (e.g. Nemotron 3.5 Lightning). This file is a finished spec, not a brief — every content and design decision below is final. Do not invent, rewrite, "improve," or add to any copy. Your job is implementation only.

**Output:** one file, `index.html`, at the repo root. Fully self-contained (inline `<style>` and `<script>` — no build step, no npm, no framework).

---

## Hard constraints

- Single HTML file. No React/Vue/Svelte/Tailwind-the-compiler. Vanilla HTML/CSS/JS only.
- Dark theme only — no light mode, no theme toggle.
- Total page weight under 200KB, excluding the optional headshot image.
- Mobile-first, fully responsive. Must render correctly at 375px width and at 1440px+.
- Only external resources allowed: the Google Fonts stylesheet (`fonts.googleapis.com`) and its font files (`fonts.gstatic.com`). Nothing else loads from the network.
- No animation beyond a simple CSS fade-in on scroll — and only if it adds near-zero complexity. Skip it if in doubt.
- Favicon: a single emoji via a data-URI `<link rel="icon">` (use ⚡). No image file needed.
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>` per block, `<footer>`. Exactly one `<h1>`. All interactive elements keyboard-reachable.
- Do not add sections, projects, metrics, or testimonials beyond what's specified below.

---

## Design tokens — use exactly these values, do not substitute or "refine" them

```css
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;600&family=Syne:wght@400;700;800&display=swap');

:root {
  --bg: #0a0a0f;
  --surface: #111118;
  --surface2: #16161f;
  --border: #1e1e2e;
  --accent: #00ff88;   /* primary: CTAs, links, the `helix game.exe` line */
  --accent2: #ff4466;
  --accent3: #4488ff;
  --accent4: #ffaa00;
  --text: #e2e2f0;
  --muted: #6666aa;
  --mono: 'IBM Plex Mono', monospace;
  --sans: 'Syne', sans-serif;
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  background-image:
    radial-gradient(ellipse 80% 40% at 50% -10%, rgba(0,255,136,0.06) 0%, transparent 70%),
    repeating-linear-gradient(0deg, transparent, transparent 39px, rgba(255,255,255,0.015) 39px, rgba(255,255,255,0.015) 40px),
    repeating-linear-gradient(90deg, transparent, transparent 39px, rgba(255,255,255,0.015) 39px, rgba(255,255,255,0.015) 40px);
}
```

- Headings: `--sans` (Syne), weight 700–800.
- Labels, tags, code, nav links, CLI examples: `--mono` (IBM Plex Mono).
- Every skill/tech tag is a small bordered pill: translucent tint of its color at ~12% opacity for the background, a 1px border at ~30% opacity, the solid color for the text. Rotate pills through `--accent`, `--accent3`, `--accent4` — don't make every pill the same color.

---

## Page structure — exactly 6 sections, in this order, one scrolling page, sticky anchor nav

1. Header/nav — wordmark ("Abdou") + anchor links (Work / Experience / About / Contact) + a "Résumé" button, right-aligned
2. Hero
3. Proof strip
4. Work — two cards: Helix (full width, featured) then Stock Sentiment Screener (smaller, secondary)
5. Experience
6. Skills + About + Contact — may share one closing section/screen

---

## Exact copy — verbatim, do not rewrite

### Hero

**Headline:** Systems-level Rust engineer. Full-stack when the job needs it.

**Body:** I'm Abdou — BSc Computing student at RIT Dubai. I built Helix, a Rust CLI that reaches into the Linux kernel so Windows games run right with zero config, and I've shipped backend integrations (WhatsApp Business API, payment gateway testing) at a live startup. Open to backend and systems engineering internships.

**Buttons:** "See the work" (anchors to `#work`) · "Résumé" (links to `resume.pdf` — see Assets, not yet supplied)

**Availability tag (small, under the buttons):** 📍 Dubai, UAE — open to Malaysia / remote SEA · Available for a 3-month internship, immediate start

### Proof strip (one row of pills, no extra copy)

- KCNA — Kubernetes & Cloud Native Associate (CNCF, Oct 2026) → links to `http://training.linuxfoundation.org/certification/verify`
- Rust · Java · Python · Node.js
- RIT Dubai — BSc Computing
- EN · FR · AR fluent

### Work — Helix (featured card)

**Title:** Helix — Linux Gaming Compatibility Orchestrator
**Prompt line (render as a code block):** `helix game.exe` — and that's the whole user manual.

**Problem:** Running a Windows game well on Linux means manually juggling Wine prefixes, DXVK configs, kernel sysctl values, GPU power states, and anti-cheat compatibility — across half a dozen tools that each solve one piece of it.

**What I built:** A distro-agnostic Rust CLI that detects your hardware and the game binary, decides the right Proton/DXVK/VKD3D-Proton configuration, and applies only the tweaks that game actually needs — through a strict Plan → Confirm → Execute flow, so nothing touches your system without sign-off.

**Technical highlights (bulleted list):**
- Static PE binary analysis (`goblin`) reads DirectX version, anti-cheat, and engine straight from the executable's import table — no running untrusted code to find out what it needs.
- 12+ system tweaks (fsync/esync, `vm.max_map_count`, THP mode, `sched_autogroup`, GPU clock locking) live as typed data in a tweak registry, not branching logic — adding tweak #50 never touches the orchestrator.
- Every persistent system change writes a rollback manifest *before* it's applied; `helix --undo` reverses it. Session-scoped changes restore automatically via Rust Drop guards, even on a crash.
- Privilege separation: the main binary never runs as root. A minimal `helix-helper` companion, invoked through polkit, accepts only a hardcoded whitelist of sysctl/sysfs writes — no shell interpolation anywhere in the codebase.
- Zero `.unwrap()` outside tests. Every failure path returns a typed, actionable error.

**Status line:** Architecture, detection, and planning pipeline complete and tested; verified end-to-end on real hardware; broader title coverage and AMD GPU support are the active roadmap.

**Tags:** Rust · Linux · Systems programming · Security
**Link:** "View on GitHub →" to `https://github.com/abdoumarkaba/Helix`

### Work — Stock Sentiment Screener (secondary card)

**Title:** Stock Sentiment Screener

**Body:** A Rust tool that scores market sentiment in real time — pulling live data from Alpha Vantage and NewsAPI, running news through FinBERT for sentiment scoring, and serving it over a REST API backed by MariaDB via SQLx.

**Tags:** Rust · REST API · MariaDB · ML integration
**Link:** "View on GitHub →" — URL not supplied, see Links below.

### Experience

**Software Development Intern — Markaba** · Dec 2025–Feb 2026 · Remote
Integrated WhatsApp Business API with Twilio for automated customer workflows; shipped a multi-step WhatsApp popup across the production site; ran end-to-end testing across the payment gateway (Moyasar), auth, and reservation flows; improved the car-value-calculator's accuracy and UX.

**Business Analyst Intern — ONCF** (Morocco's national railway operator) · Jul–Aug 2023
Mapped the furniture procurement workflow end-to-end and delivered a structured report with cost and vendor-selection recommendations.

*(Render ONCF visually smaller/lighter than Markaba — one line, not a full block.)*

### Skills (five grouped lines, not a logo grid)

- **Systems & backend** — Rust, Java (Spring Boot), Python, Node.js, REST API design, system architecture
- **Data** — SQL, MariaDB, MySQL, database architecture
- **Web** — Vue.js, HTML5, Tailwind CSS
- **Cloud-native** — Kubernetes & Cloud Native Associate (CNCF certified)
- **Spoken** — Arabic (native), French (native), English (fluent), Spanish & Mandarin (basic)

### About

I'm a BSc Computing student at RIT Dubai who ended up building Linux kernel tweaks for fun. Helix exists because I actually run this setup — Fedora 43, an RTX 3050, modded Cyberpunk 2077 and RDR2 — and got tired of hand-tuning sysctl values every time I wanted to play something. I gravitate toward problems where the constraint is "this has to actually be correct," which is most of why I lean backend/systems over UI.

### Contact / footer

Email · LinkedIn · GitHub · Résumé (PDF)
**Footer line:** Available for a 3-month internship, immediate start. Based in Dubai, open to Malaysia / remote SEA roles.

---

## Links — use these exact URLs, do not guess or shorten them

- Helix repo: `https://github.com/abdoumarkaba/Helix`
- GitHub profile: `https://github.com/abdoumarkaba`
- LinkedIn: `https://linkedin.com/in/a-naitbrahim`
- Email (`mailto:`): `abderrahmane.naitbrahim@gmail.com`
- KCNA verify: `http://training.linuxfoundation.org/certification/verify` (cert ID `LF-4zol24n6ki`)
- Stock Sentiment Screener repo: **not supplied.** Omit the "View on GitHub" link on that card entirely (don't link to `#` or guess a URL) until a real one is provided.

---

## Assets — do not fabricate these

- `resume.pdf` — not supplied. Point the Résumé button to `#`, and add `<!-- TODO: resume.pdf missing -->` directly above it in the HTML.
- Headshot photo — optional, not supplied. Ship without one. Leave a single commented-out `<img>` tag in the About section (`<!-- <img src="headshot.webp" alt="Abdou" class="headshot"> -->`) so adding it later is a one-line change.

---

## Acceptance checklist — confirm every line before calling this done

- [ ] All 6 sections present, in the exact order specified
- [ ] Zero external JS, zero frameworks, zero build step
- [ ] Opens correctly directly from disk (`file://...index.html`) with no console errors
- [ ] Total weight (HTML + CSS + fonts, excluding any headshot) is under 200KB
- [ ] Correct, uncluttered layout at 375px width and at 1440px+ width
- [ ] Only `fonts.googleapis.com` and `fonts.gstatic.com` are referenced externally
- [ ] No copy anywhere deviates from the "Exact copy" section above
- [ ] No sections, projects, metrics, or testimonials exist beyond what's listed
- [ ] `resume.pdf` and the Stock Sentiment Screener repo link are handled exactly as the Assets/Links sections specify (not guessed, not faked)
