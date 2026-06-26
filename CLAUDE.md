# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website for Hamidreza Yaghoubi (Ph.D. student, UMD GAMMA Lab). Built on the [Academic Pages](https://academicpages.github.io/) Jekyll theme, which is itself a fork of Minimal Mistakes.

The site is deployed to GitHub Pages at `https://hamidrezayaghoubi.github.io`.

## Local development

```bash
bundle install          # install Ruby dependencies (first time or after Gemfile changes)
jekyll serve -l -H localhost   # serve at localhost:4000 with live reload
```

Or via Docker:
```bash
docker build -t jekyll-site .
docker run -p 4000:4000 --rm -v $(pwd):/usr/src/app jekyll-site
```

## Architecture

**Two rendering paths coexist:**

1. **`index.html`** — The homepage is a hand-crafted, self-contained HTML/CSS/JS file with a dark GitHub-style design. It does **not** go through Jekyll's layout system or use Liquid templates. All styles are inline in `<style>` tags. This is intentional — it bypasses the Academic Pages theme entirely for the landing page.

2. **Jekyll-rendered pages** — Everything under `_pages/` (about, CV, publications, talks, etc.) uses the Academic Pages theme. Layouts live in `_layouts/`, partials in `_includes/`, and styles in `_sass/`. The main content page is `_pages/about.md`, which also contains the publications list and work experience.

**Key config files:**
- `_config.yml` — Site-wide settings: author info, social links, plugin list, collection definitions, permalink structure.
- `_data/navigation.yml` — Controls which links appear in the masthead navigation. Currently only "CV" is active; Publications/Talks/Teaching are commented out.

**Content collections** (defined in `_config.yml`):
- `_publications/` — Individual publication markdown files
- `_talks/` — Talk files
- `_teaching/` — Teaching files
- `_portfolio/` — Portfolio items
- `_posts/` — Blog posts

Note: the `_pages/about.md` page maintains its **own** publications list as HTML, separate from the `_publications/` collection. The collections exist from the template but are not currently linked in navigation.

**Custom styling:**
- `_sass/_custom.scss` — All site-specific CSS overrides (layout widths, typography, component classes like `.publications`, `.work-experience`, `.academic-service`). Edit here for theme customization.
- `_pages/about.md` includes inline `<style>` for page-specific overrides.

**Markdown generators** (`markdown_generator/`): Python scripts and Jupyter notebooks to batch-generate publication/talk markdown files from TSV data. Run `publications.py` or `talks.py` to regenerate collection files from `publications.tsv` / `talks.tsv`.

## Content editing

- **Homepage** (`index.html`): Edit directly as plain HTML. No Jekyll processing.
- **About/bio** (`_pages/about.md`): Main content page with bio, publications list, work experience, and academic service. Uses HTML divs with CSS classes for structured sections.
- **Author sidebar**: Controlled by `author:` block in `_config.yml` (name, bio, social links, avatar).
- **Navigation links**: Edit `_data/navigation.yml` to add/remove/reorder header links.
- **CV link**: Currently points to a Google Drive PDF in `_data/navigation.yml`.
- **Files** (PDFs, etc.): Place in `files/` directory; accessible at `/files/filename.pdf`.
- **Images**: Place in `images/`; profile photo is `images/profile.png`.

## Research papers (ground truth for website copy)

Papers are in `papers/` (not committed to git). Do not claim things about these papers that are not stated here.

**Recti-Q** (IROS 2026) — `Recti-Q.pdf`
- Full title: "Feature-Space Rectification for Out-of-Distribution-Robust Quantized Perception in Edge Robotics"
- Identifies the "Quantization-Induced Robustness Gap": PTQ models preserve clean-input accuracy but degrade significantly under distribution shift (sensor noise, weather, novel environments)
- Solution: freeze the quantized backbone, train a small LoRA adapter on the classifier head using source data only. Less than 1% parameter overhead (as small as 6 KB)
- Architecture-agnostic (CNNs and Transformers); evaluated on ImageNet-C and PACS
- Authors: Hamidreza Yaghoubi*, Parastoo Pilevar*, Ming C. Lin

**PolySona** (IROS 2026, also under review at ICLR 2026) — `PolySona.pdf`
- Full title: "Parameter-Efficient and Modular Latent Behavior Modeling for Traffic Simulation"
- Mixture-of-Experts (LoRA) framework to extract latent driving styles in trajectory prediction models
- Addresses rare/safety-critical scenarios where driving style diversity produces multi-modal trajectory outcomes
- Enables counterfactual simulation and style-consistent predictions
- NOT about temporal understanding in the VLM/video sense; NOT about real-time on-vehicle inference specifically
- Authors: Laura Zheng, Hamidreza Yaghoubi, Tony Wu, Tianyi Zhou, Ming C. Lin

**IROS 2025** — `2503.04994.pdf`
- Full title: "Quantifying and Modeling Driving Styles in Trajectory Forecasting"
- Analyzes driving style quantification in real-world trajectory datasets; proposes style-based resampling to improve forecasting of rare/edge-case driving behaviors
- NOT about temporal understanding or on-vehicle real-time operation
- Authors: Laura Zheng*, Hamidreza Yaghoubi Araghi*, Tony Wu, Sandeep Thalapanane, Tianyi Zhou, Ming C. Lin

**Decompose-and-Compose** (CVPR 2024) — `Decompose and Compose CVPR 2024 Paper.pdf`
- Addresses spurious correlation via compositional image manipulation
- Uses class activation maps to separate causal from spurious components; creates counterfactual training data by combining image elements
- Achieves better worst-group accuracy without group labels
- Authors: Fahimeh Hosseini Noohdani, Parsa Hosseini*, Aryan Yazdan Parast*, Hamidreza Yaghoubi Araghi, Mahdieh Soleymani Baghshah

**Annotation-Free Group Robustness** (OOD-CV @ ICCV 2023) — `2312.04893.pdf`
- Proposes LFR (Loss-Based Feature Re-Weighting): infers data groupings from ERM model losses, resamples to create a balanced set, retrains only the last layer
- No group annotations required; outperforms prior annotation-free methods, especially under high spuriosity
- Authors: Mahdi Ghaznavi, Hesam Asadollahzadeh, Hamidreza Yaghoubi Araghi, Fahimeh Hosseini Noohdani, Mohammad Hossein Rohban, Mahdieh Soleymani Baghshah
