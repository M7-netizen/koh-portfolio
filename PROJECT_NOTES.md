# PROJECT_NOTES — Koh's UX/UI Portfolio

> Handoff notes so Claude Code (or any developer) can pick this project up mid-stream.
> Written 2026-07-16. The owner is **Koh (GitHub: M7-netizen)**, a UX/UI designer
> transitioning from an architecture / BIM background, job-seeking now. Communicate
> with her primarily in **Mandarin Chinese**; she prefers direct, practical guidance.

---

## 1. What this project is

A personal UX/UI **portfolio website**, built as plain static files (HTML/CSS/vanilla JS)
with a lightweight Git-based CMS on top so Koh can edit content without touching code.

There is also a **separate** interactive prototype ("Flora") in its own repo, linked from
the portfolio as a project. Do not confuse the two repos.

### Two repos
| Repo | What it is | Live URL |
|---|---|---|
| `koh-portfolio` | The portfolio site (this project) | Netlify (see §4) |
| `Flora` | Standalone hi-fi prototype (React via CDN, single `index.html`) | https://m7-netizen.github.io/Flora/ (GitHub Pages) |

---

## 2. File structure (koh-portfolio)

```
koh-portfolio/
├── index.html              # Landing page: crayon-illustration hero + single-page scroll
├── project.html            # Project detail page (reads ?p=<index> from query string)
├── PROJECT_NOTES.md         # this file
├── .nojekyll               # REQUIRED on GitHub Pages to stop Jekyll breaking the build
├── admin/
│   ├── index.html          # Decap CMS loader
│   └── config.yml          # CMS field definitions (the whole editable schema lives here)
├── content/
│   └── portfolio.json      # ALL site content lives here; CMS reads/writes this file
└── uploads/
    ├── background_crayon.png  # the hand-drawn hero background
    └── (CMS-uploaded images: covers, screenshots, photo, etc.)
```

### How rendering works
- `index.html` and `project.html` are templates. On load they `fetch('content/portfolio.json')`
  and inject content into the DOM via vanilla JS. **There is no build step.**
- All user content (text, image paths, links) is in `content/portfolio.json`.
- `admin/config.yml` defines the CMS form fields. Field `name`s must match the JSON keys
  and the keys the JS reads. **If you add a field to the site, add it to config.yml too**,
  or Koh won't be able to edit it.

### Key JSON keys currently in use
`name`, `aboutParagraphs[]`, `sectionTitles{about,projects,others,skills,contact}`,
`caseStudies[]` (the projects), `others[]` (freelance), `skills[]`,
`email`, `linkedin`, `resume`, `xiaohongshu`,
`illustrationQuote`, `illustrationAttribution`, `heroBackground`, `profilePhoto`.

Each `caseStudies[]` item: `status, title, cover, coverTone(dark|light), description,
problem, process, outcome, link, tagline, heroImage, heroTone, role, timeline, tools,
platform, concept, approach, features[]{title,detail}, webShots[]{image,caption},
mobileShots[]{image,caption}, reflection`.

Each `others[]` item: `title, description, image, link`.

---

## 3. Design direction (important — respect this)

- **Aesthetic:** warm, hand-made storybook feel, inspired by a Saint-Exupéry / *Le Petit
  Prince* watercolour–crayon illustration (sea, sand dunes, hilltop town, palm trees,
  night sky). The landing hero uses Koh's **real hand-drawn crayon PNG** as a full-screen
  background — this is deliberate: earlier attempts with code-drawn SVG looked "too clean /
  unnatural / cheap." Keep the genuine hand-drawn texture.
- **Palette (CSS vars in index.html `:root`):** cream `#F4E8CE`, navy `#274372`,
  sea blue `#2E6DA4`, gold `#D9A62C`, soft plum `#8B7BA8`.
- **Type:** `Caveat` (handwritten — quote, accents, hero nav), `Fraunces` (serif headings),
  `Nunito` (body/UI).
- **Interactions so far:** quote fades in on load; nav links reveal when hovering the quote;
  gentle mouse-parallax on the background; scroll-reveal on sections; sticky top bar appears
  after scrolling past the hero.
- Project cards: 3-per-row, rounded 16px, cover image fills the card, title over cover with
  **tone-based text colour** (`coverTone: dark` → white text, `light` → black text — this was
  a real bug once: heading colour must be set explicitly per tone, NOT inherited, because a
  global `h3{color}` rule overrides inheritance). On hover the card lifts + scales and a
  gradient overlay reveals the short description + "View project". Cards link to
  `project.html?p=<index>`.

---

## 4. Deployment

- Hosted on **Netlify**, connected to the `koh-portfolio` GitHub repo (auto-deploys on push
  to `main`). NOTE: the **first** Netlify account ran out of free build credits; Koh created a
  **second Netlify account** (email signup, not GitHub login) and reconnected the repo. If
  builds stop deploying, check credits / which account owns the site.
- The CMS login uses **Netlify Identity + Git Gateway** (Site config → Identity → Enable;
  Registration = Invite only; Services → Git Gateway → Enable; then invite Koh's email).
- `index.html` includes the Netlify Identity widget so invite/recovery tokens landing on the
  homepage redirect into `/admin/`.
- The CMS admin is at `<site>/admin/`.
- Flora is on **GitHub Pages** (different mechanism, needs `.nojekyll`). Don't move Flora to
  Netlify without reason.

### Gotchas already hit (don't repeat)
- GitHub Pages 404 → usually a **nested folder** (files uploaded inside an extra dir instead
  of repo root), or missing `index.html`, or missing `.nojekyll` (Jekyll build fail).
- GitHub Pages URL is **case-sensitive** to the repo name (`/Flora/` not `/flora/`).
- Netlify "no change" after upload → almost always deploy didn't trigger or ran out of
  credits; check Deploys tab, use "Clear cache and deploy site".
- Decap `/admin` caches `config.yml` hard — after changing it, hard-refresh (Ctrl+Shift+R)
  or use an incognito window.
- Decap has **no built-in image cropping**. We added dimension **hints** on image fields
  instead. Real cropping would need a 3rd-party media library (e.g. Cloudinary) — not set up.

---

## 5. Content status

- **Project 1 — Flora**: fully written up (concept, approach, key features, reflection with a
  "this is a design prototype, not a functioning product" note). Live link points to the
  GitHub Pages Flora site. Screenshots + captions being added via CMS.
- **Project 2 — 月帐 (expense-tracking PWA)**: NOT added yet, and blocked on a privacy issue —
  the live app shows Koh's **real financial data with no login wall**. Decision made: build a
  **demo version with fake data**, deploy it separately, and link that (never link the real
  app). Copy for concept/PPO/features was drafted but the demo build + deploy is still TODO.
- **Others (freelance)**: section + CMS fields exist (web / graphic / logo design). Content
  still to be added.
- **About**: needs Koh's photo (`profilePhoto`) and bio paragraphs via CMS.
- **Résumé** (separate doc, not in this repo): in progress — profile summary, education
  (added Google UX Design Certificate), SAA + DP Architect experience, freelance. Written as
  a career-change narrative emphasising transferable skills.

---

## 6. Open TODOs / next tasks

1. **Opening "doodle forms" animation** (the current active request). Options discussed:
   - (a) Hand-drawn **mask reveal / wipe-in** of the existing crayon PNG — feasible, no assets
     needed. Likely the chosen route.
   - (b) SVG line-drawing (`stroke-dashoffset`) — only works on vector paths, would lose the
     crayon texture. Rejected direction.
   - (c) Real drawing-process **video/frames** provided by Koh — 100% authentic, needs her to
     record. Not decided yet.
   Confirm which route she wants before building.
2. Build the **月帐 demo (fake data)** and deploy it; then add Project 2 to the portfolio.
3. Add **freelance** items to `others[]`.
4. Add Koh's **photo + bio** in the CMS.
5. Ongoing polish: hero quote position, background focal point, animation pacing.

---

## 7. Working style notes

- Reply in **Mandarin**; be concise and practical; explain trade-offs honestly (she values
  knowing what's technically possible vs not before committing).
- She is **non-technical about code** but a capable designer — explain deploy/git steps
  step-by-step, and prefer solutions that don't require her to hand-edit code.
- When changing the site, keep `index.html`, `project.html`, and `admin/config.yml` in sync,
  and **do not overwrite `content/portfolio.json`** (it holds her edited content) unless
  intentionally migrating data.
- To preview locally: any static server works, e.g. `python3 -m http.server` in the repo root,
  then open `http://localhost:8000/`. (The CMS `/admin` needs the deployed Netlify Identity to
  log in; local preview of the public site works fine without it.)
```
