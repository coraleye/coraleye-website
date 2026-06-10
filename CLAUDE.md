# CLAUDE.md

Guidance for working in this repository.

## What this is

The marketing landing page for **Coraleye** (an AI customer-success agent,
"Cory"), served as a static site via **GitHub Pages**. There is no framework,
build step, package manager, or test suite — it is hand-written HTML/CSS/JS.

## ⚠️ The deployed site lives in a nested subfolder

The repo has a confusing doubled layout:

```
coraleye-website/            <- repo root
├── CNAME                    <- DEAD: ignored by the Actions deploy
├── README.md
├── .github/workflows/deploy-coraleye-website.yml
└── coraleye-website/        <- THIS folder is what actually ships
    ├── index.html           <- the entire site
    ├── kory.png             <- Cory mascot
    └── CNAME                <- the live custom domain (coraleye.ai)
```

**Edit `coraleye-website/index.html`** (the inner one). The deploy workflow
uploads `path: coraleye-website`, so files at the repo root are NOT published.
The root-level `CNAME` is leftover and has no effect.

## Architecture

`coraleye-website/index.html` is the whole product — a single self-contained
file with all CSS in one `<style>` block and all JS in one `<script>` block at
the bottom. No external assets except `kory.png`. Sections in document order:
nav → hero → about → integration strip → problem → how-it-works → meet-cory →
quotes → CTA → footer.

Notable JS behaviors:
- Sticky nav that gains a `.scrolled` background past 80px.
- `IntersectionObserver` scroll-reveal on `.reveal` elements.
- A hero "live dispatch" ticker cycling canned messages on a 3s interval.
- `removeDarkBg()` strips the navy background from `kory.png` at runtime via a
  canvas corner flood-fill, so the mascot reads as transparent on any backdrop.

CSS is custom-property driven (`:root`) with `clamp()` type scaling. Responsive
breakpoints are at **900px** (tablet) and **600px** (phone) at the bottom of
the `<style>` block — when adding a multi-column grid, remember to collapse it
in the 900px block or it will overflow on mobile.

## Deploy

Pushing to `main` with changes under `coraleye-website/**` triggers
`.github/workflows/deploy-coraleye-website.yml`, which publishes to GitHub Pages
at the custom domain in `coraleye-website/CNAME` (**coraleye.ai**). There is no
preview environment — verify locally by opening the file in a browser, e.g.
`open coraleye-website/index.html`.

## Conventions

- Keep everything inline in the single file unless there's a strong reason to
  split — match the existing structure and the em-dash section-comment style.
- Test responsive changes at ≤600px and 600–900px before committing.
- The canonical domain is **coraleye.ai**; keep links and addresses consistent
  with it.
