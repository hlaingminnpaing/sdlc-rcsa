# SDLC RCSA — Web Edition

A single-file web version of the SDLC Risk &amp; Control Self-Assessment template
(adapted from the Kosli secure-SDLC reference model). No build step, no server.

- **Risk Register** — edit inherent Likelihood &amp; Impact; residual risk is calculated automatically.
- **Risk–Control Matrix** — the risk↔control mapping.
- **Controls** — set implementation status &amp; effectiveness; this drives every mapped risk's residual score.
- **Dashboard** — live residual heat map and posture breakdown.
- **Scoring &amp; Methodology** — the 1–5 scales, rating bands, and the residual formula.

Entries are saved in your browser (localStorage). Use **Export data** to keep a JSON
backup and **Import** to restore it on another machine. **Export CSV** dumps the risk register.

## Deploy to GitHub Pages (free)

1. Create a new repository on GitHub (e.g. `sdlc-rcsa`).
2. Add `index.html` (and this README) and commit to the `main` branch.
3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   pick `main` / `root`, and Save.
4. Your app goes live at `https://<your-username>.github.io/sdlc-rcsa/` within a minute or two.

## Edit / extend

Everything lives in `index.html`. The seed data is the `SEED` object near the top of the
`<script>` block — adjust risks, controls, mappings, or scoring weights there.

readme_content = """# Secure SDLC Risk & Control Self-Assessment (RCSA) Template

An automated, data-driven **Risk & Control Self-Assessment (RCSA)** framework aligned to the **Kosli Secure SDLC model**. This template separates risks from controls, mapping them via a central matrix where residual risk scores are automatically calculated based on the assessed effectiveness of underlying controls.

---

## 📋 Overview & Model Structure

The framework encapsulates the entire software delivery lifecycle across **9 Core SDLC Risks** and **22 Security Controls** distributed across four key stages:
