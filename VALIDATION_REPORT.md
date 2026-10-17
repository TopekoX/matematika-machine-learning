# Validation Report

Date: 2026-10-17 · Platform: Windows 11 · Browser: Google Chrome (headless, driven over the DevTools protocol) · Nothing was committed, pushed, or deployed.

## Environment and dependency versions (project `.venv`)

Python **3.14.8** (the only interpreter installed; Python 3.12 was **not** available and **not** tested).

| Package | Version |
|---|---|
| mkdocs | 1.6.1 |
| mkdocs-material | 9.7.7 |
| mkdocs-material-extensions | 1.3.1 |
| pymdown-extensions | 12.3 |
| Markdown | 3.11.1 |
| Pygments | 2.21.0 |
| Jinja2 | 3.1.6 |
| MathJax (CDN, pinned) | 3.2.2 |
| Mermaid (vendored, pinned) | 11.17.2 |

`requirements.txt` ranges: `mkdocs>=1.6,<2`, `mkdocs-material>=9.5,<10`, `pymdown-extensions>=10.8,<13`.

## Commands executed and results

| Command | Result |
|---|---|
| `python -m venv .venv` and `pip install -r requirements.txt` | succeeded |
| `pip check` | "No broken requirements found." |
| `mkdocs build --strict` (final run, after all changes) | **exit 0**, no `WARNING`/`ERROR` lines, 63 files in `site/`, built in ~1.2 s |

**Upstream notice:** every build prints a boxed "Warning from the Material for MkDocs team" about MkDocs 2.0. It is plain output, not a log warning; strict mode still exits 0. It was not suppressed. Project-specific warnings: none.

## Build-level checks (executed)

- Built HTML contains `class="arithmatex"` spans; `\frac`, `\begin{aligned}`, `\begin{bmatrix}` survive intact; no `<em>` inside math.
- Output has `index.html`, `search/search_index.json`, `stylesheets/extra.css`, `javascripts/mathjax.js`, `javascripts/mermaid-config.js`, `assets/vendor/mermaid/{mermaid.min.js,LICENSE,VERSION.txt}`.
- Palette entries `(prefers-color-scheme)`, `light`, `dark` present in the generated HTML.
- `docs/examples/mermaid.md` build output has 10 `class="mermaid"` blocks and the Mermaid source text (e.g. `flowchart TD`).
- Code blocks produce `language-java/c/cpp/python/javascript/sql/bash/yaml/json/typescript/text` classes.
- Internal links/anchors: checked by strict build with `validation` set to `warn`.

## Browser tests (executed in headless Chrome, served from `site/` by a static Node server)

Note: an early run used Python's `http.server`; it stalled/reset connections on Windows and produced false failures (bundle response after 19 s, `ERR_CONNECTION_RESET`). Those results were discarded and all results below come from the Node static server.

**MathJax / pages (36 page loads: 6 pages × 3 widths × light/dark)**
- Math rendered (`mjx-container` present: 30 on the math page, 10 on the sample chapter, 4 on the home page); 0 MathJax error nodes; inline, display, matrix, aligned, softmax, and math inside an admonition and tabs all appeared in screenshots (light and dark viewed).
- Widths 1280 / 768 / 375 px: no page-level horizontal overflow and no overflowing content elements outside intentional scroll containers.
- Copy buttons present on code pages (13 on the code page, 5 on the sample chapter).

**Theme**
- Palette button cycled System → Light → Dark → System (stored `__palette` index 0→1→2→0).
- Emulated `prefers-color-scheme` light/dark gave `default`/`slate` schemes. Screenshots of math, components, code, and Mermaid pages in dark and light were inspected visually. **Contrast was judged visually, not measured with a contrast tool.**

**Search**
- Typing "sigmoid" with real key events returned "2 matching documents" (math page, home page). Note: Material listens for `keyup`, so synthetic `input` events give no results; this was a test-harness issue, not a site defect.

**Mermaid** (`/examples/mermaid/`, 4 loads: light/dark × 1280/375 px)
- Vendored `mermaid.min.js` and `mermaid-config.js` were requested from the local server; no request to `unpkg.com` was observed (request log from the earlier run: external hosts were only `cdn.jsdelivr.net` for MathJax and `api.github.com` from `repo_url`).
- All 9 valid diagrams rendered in every run (flowchart, sequence, class, state, ER, Gantt, pie, Git graph, mindmap), viewed in screenshots in light and dark; mobile width has no page overflow.
- Live theme change (light → dark via the palette button) re-rendered the diagram in dark colours.
- Malformed diagram: stays as source text in its own block; the rest of the page renders; one console `Parse error` exception is thrown per page load. The "Syntax error" graphic Mermaid appends to the page body by default is suppressed (`suppressErrorRendering`).
- Contrast problems found and fixed during testing: pie title/legend and Gantt title in dark mode, Git-graph commit/branch labels (`mermaid-config.js`).

## Tests not performed / unverified

- Python 3.12 install and build.
- Real mobile/tablet devices (only emulated viewports).
- Keyboard-only navigation and focus-indicator appearance (CSS added; not exercised).
- Quantitative contrast measurement (WCAG ratios) and screen-reader behaviour.
- Safari/Firefox/Edge (Chrome only).
- Mermaid rendering of every diagram type in the "System" palette state without emulation, and tablet width for Mermaid.
- Offline behaviour: Mermaid is vendored but MathJax is **not**; offline math is unsupported and untested.
- `mkdocs gh-deploy` and GitHub Pages hosting: intentionally not run. Deployed-path asset loading was not tested (only root-path local serving; paths are relative).
- `mkdocs serve` (live server) was not run to completion.

## Remaining limitations

- MathJax needs network access (CDN).
- Setting `repo_url` makes the browser call `api.github.com` on every page.
- The 3.5 MB Mermaid script is downloaded on every page (cached after the first load).
- Gantt and sequence diagram text is small at phone width; Git-graph label colours are fixed only for the first two branches.
- Material for MkDocs 9.x is in maintenance mode upstream (MkDocs 2.0 notice).
- Test artifacts remain in `.shots/` (git-ignored); delete it manually when no longer needed (automatic deletion was blocked by a safety check).

---

# Level 2 (Aljabar Linear dan Kalkulus): final QA

Date: 2026-10-17 · Scope: `docs/level-2-aljabar-linear-dan-kalkulus/` (index + Parts 1-12). The sections above describe the template-stage tests; the template demo pages they mention were removed later. Nothing was committed, pushed, or deployed.

## Executed

| Check | Result |
|---|---|
| `mkdocs build --strict` (project `.venv`, final run) | **exit 0**, no `WARNING`/`ERROR` lines (only the upstream Material/MkDocs 2.0 notice), 37 pages built. `pip check`: no broken requirements. |
| Internal links and anchors | Covered by the strict build (`validation` set to warn). Forward references to unwritten Level 3 are plain text. |
| Python snippets (stdlib only) | All 63 code blocks in Parts 1-12 run without error; every printed value matches the value in the lesson's output comment. |
| Worked examples and exercise answers | Recomputed by script while writing each batch (Parts 1-12); numbers in tables, worked examples, and answer keys matched. |
| Page structure | Each part has exactly one `#` heading, balanced code fences, the required sections (tujuan, prasyarat, kesalahan umum, hubungan dengan ML, kunci jawaban, ringkasan, checklist), and 10 exercises (Part 12: 22 graded exercises plus a 10-item evaluation). |
| Built HTML, math | No tags inside `arithmatex` spans, no broken tables (`<p>|`), no leftover `$`, no `\boldsymbol` in any page. |
| MathJax 3.2.2 in headless Chrome | Each part's formulas extracted to a test page and typeset: containers rendered = formulas present for all 12 parts (264, 362, 427, 373, 454, 403, 567, 452, 430, 434, 434, 364). **No literal backslash text inside any `mjx-container`** (this detects undefined macros, which render as raw text). |
| Mermaid 11.17.2 `mermaid.parse` | All 8 diagrams in Level 2 parse (Part 4 flowchart; Part 7 two `xychart-beta`; Part 9 two flowcharts; Part 10 bar chart; Part 11 `xychart-beta` + flowchart). |

## Defect found and fixed during Level 2

- Part 8 displayed raw `\boldsymbol` in the user's browser (autoloaded MathJax extension did not load). Replaced by `\mathbf{d}` (13 places). An earlier MathJax check had missed this because it looked for `mjx-merror` instead of backslash text; the check above is the corrected method.

## Not performed / unverified

- **Visual rendering inside the real site** (Material theme, light/dark, phone width) for the Level 2 pages. The MathJax test used extracted formulas on a bare page, not the site shell.
- **Mermaid visual rendering** of the Level 2 diagrams: only syntax (`mermaid.parse`) was verified. Mermaid rendering is intermittently flaky in headless Chrome here.
- **Inline SVG figures** (Parts 2 and 5, four figures) were inspected as screenshots on standalone pages, not within the site, and not in dark mode.
- Keyboard/screen-reader accessibility of the new pages, other browsers, real devices.
- A full scan of Level 0 and Level 1 for undefined macros was started but did not finish (only Level 1 pages up to Part 11 completed); those levels were not changed except for link fixes. `\boldsymbol` is absent from them by text search.
- MathJax still needs network access (CDN).

---

# Level 0 and Level 1: QA re-run with the Level 2 method

Date: 2026-10-17 · Scope: `docs/level-0-fondasi-matematika/` (8 parts) and `docs/level-1-aljabar-dasar-dan-fungsi/` (12 parts). No lesson content was changed in this pass. Nothing was committed, pushed, or deployed.

## Executed

| Check | Result |
|---|---|
| `mkdocs build --strict` (whole site, final run) | exit 0, no `WARNING`/`ERROR` lines (only the upstream MkDocs 2.0 notice). |
| Python snippets | Level 0: 31 blocks, Level 1: 62 blocks. All run without error; every printed value matches the value in its output comment. |
| Page structure | Each of the 20 parts has exactly one `#` heading, balanced code fences, and the sections tujuan, prasyarat, kesalahan umum, kunci jawaban, ringkasan, checklist. |
| Built HTML, math | 20 pages: no tags inside `arithmatex` spans, no broken tables (`<p>|`), no stray `$`; no `\boldsymbol` anywhere. |
| MathJax 3.2.2 in headless Chrome | Formulas of each part extracted to a bare test page and typeset: containers rendered = formulas present for all 20 parts (209-564 per part). **No literal backslash text inside any `mjx-container`** (detects undefined macros). |
| Mermaid 11.17.2 `mermaid.parse` | All 23 diagrams in Levels 0 and 1 parse (flowcharts and `xychart-beta`). |

No defects were found in this pass.

## Not performed / unverified

- Visual rendering inside the real site (Material theme, light/dark, phone width): the MathJax test used extracted formulas on a bare page.
- Mermaid visual rendering: only syntax was verified.
- The inline SVG four-quadrant coordinate plane (Level 1 Part 5) was not re-inspected in this pass.
- Worked examples and answer keys were verified by script when each part was written; they were not recomputed again in this pass beyond the Python snippets above.
- Accessibility, other browsers, real devices; MathJax still needs network access (CDN).

---

# Level 3 (Probabilitas dan Statistika): final QA

Date: 2026-10-17 · Scope: `docs/level-3-probabilitas-dan-statistika/` (index + Parts 1-12). Nothing was committed, pushed, or deployed. Level 0-2 lesson content was not changed; only stale forward references to Level 3 were updated (Level 2 `index.md` and two sentences in Level 2 Part 12).

## Executed

| Check | Result |
|---|---|
| `mkdocs build --strict` (project `.venv`, final run) | **exit 0**, no `WARNING`/`ERROR` lines (only the upstream Material/MkDocs 2.0 notice), 50 pages built. `pip check`: no broken requirements. |
| Internal links and anchors | Covered by the strict build. Forward references to the unwritten Level 4 are plain text. |
| Python snippets (stdlib only) | All 39 code blocks in Parts 1-12 run without error; every printed value matches the output comment in the lesson. Simulations use seeded `random.Random`; results are from Python 3.14. |
| Numbers in worked examples and answer keys | Recomputed by script while writing each batch: exact arithmetic with `fractions.Fraction`, `math.comb`, enumeration of all samples (210 and 10,000 samples), numerical integration, and `statistics.NormalDist`. |
| $t$ critical values and $t$-test p-value (Part 10) | Verified with SciPy 1.18.1, which happens to be installed in the system Python. SciPy is **not** a project dependency and no lesson code uses it; the lesson says so. |
| Page structure | Each part has exactly one `#` heading, balanced code fences, tujuan, prasyarat, kesalahan umum, hubungan dengan ML, ringkasan, checklist, a worked answer key, and 10 exercises (Part 12: 22 graded exercises plus a 10-item evaluation). |
| Built HTML, math | 13 pages: no tags inside `arithmatex` spans, no broken tables (`<p>|`), no stray `$`, no `\boldsymbol`. All LaTeX commands used are core MathJax. |
| MathJax 3.2.2 in headless Chrome | Formulas of each page extracted to a bare test page and typeset: containers rendered = formulas present for all 13 pages (3,658 formulas in total; 30 to 487 per page). **No literal backslash text inside any `mjx-container`** (detects undefined macros). |
| Mermaid 11.17.2 `mermaid.parse` | All 20 diagrams parse (flowcharts and `xychart-beta` bar/line). |
| Statistical wording audit | Searched for the common errors: p-value is never described as the probability that H0 is true; "gagal menolak" (not "menerima") H0; confidence-interval interpretation is about the procedure; correlation is not presented as causation; normality is never stated as a requirement for all models; standardization is not presented as mandatory for all algorithms. |

## Defects found and fixed during Level 3

- Part 8, worked example comparing $z$-scores: the original numbers (Rina 80 on a test with sigma 10, Budi 85 on a test with mean 80 and sigma 4) made Budi's raw score higher than Rina's, so the example did not show why raw scores mislead. Replaced by Budi 78 (mean 70, sigma 4, $z = 2$) versus Rina 80 ($z = 1$); the exercise and key were updated.
- Part 1, answer to exercise 7: reworded a sentence about the average of all sample means.
- Part 2: a typo ("berrata-rata").

## Honest caveats in the content

- Part 10, coin test (60 heads in 100): the Normal approximation gives p = 0.0455 but the exact Binomial test gives p = 0.0569, i.e. a different decision at alpha = 0.05. This is stated in the lesson on purpose.
- Maximum likelihood appears only as an intuitive introduction (Bernoulli) in Part 10, as agreed; full treatment is left to Level 4.
- Quartiles use linear interpolation; the lesson states that other methods give slightly different values (12.5 and 16.5, or 12.25 and 17.25, for the same data).

## Not performed / unverified

- **Visual rendering inside the real site** (Material theme, light/dark, phone width) for Level 3 pages. The MathJax test used extracted formulas on a bare page, not the site shell.
- **Mermaid visual rendering**: only syntax was verified (`mermaid.parse`); rendering is intermittently flaky in headless Chrome here.
- **Inline SVG boxplot** (Part 3): inspected as a screenshot on a standalone page in light mode only, not inside the site and not in dark mode.
- The simulated figures (CLT tables, coverage of confidence intervals) depend on the seeds and Python version stated; other versions of `random` may differ in the last digits.
- Accessibility, other browsers, real devices; MathJax still needs network access (CDN).

---

# Level 4 (Matematika untuk Machine Learning Klasik): final QA

Date: 2026-10-17 · Scope: `docs/level-4-matematika-ml-klasik/` (index + Parts 1-13). Nothing was committed, pushed, or deployed. `requirements.txt` and `.venv` were not changed. Level 0-3 lesson content was not changed; only stale forward references to Level 4 were updated (Level 2 `index.md`, Level 3 `index.md`, and two sentences in Level 3 Part 12).

## Executed

| Check | Result |
|---|---|
| `mkdocs build --strict` (project `.venv`, final run) | **exit 0**, no `WARNING`/`ERROR` lines (only the upstream MkDocs 2.0 notice), 64 pages built. `pip check`: no broken requirements. |
| Internal links and anchors | Covered by the strict build. Level 5 is referred to in plain text (not written). |
| Python snippets | All 49 code blocks in Parts 1-13 run without error (Python 3.14). Lesson code is stdlib only. A comment-vs-output comparison was run automatically on blocks whose output counts aligned (no real mismatches); the remaining blocks were compared manually while writing. |
| Optional scikit-learn blocks | Outputs taken from the system Python (scikit-learn 1.9.0, NumPy 2.5.2, SciPy 1.18.1). These packages are **not** project dependencies and are not installed in `.venv`; every such block is labelled as optional in the lesson. |
| Numbers in worked examples and answer keys | Recomputed by script when each batch was written: exact `fractions.Fraction`, enumeration (e.g. 45 K-Means initial pairs, exact Clopper-Pearson bounds), and seeded simulations (`random.Random`). |
| Page structure | Each part has exactly one `#` heading, balanced code fences, tujuan, prasyarat, kesalahan umum, hubungan dengan ML/NLP, 10 exercises with `???` answer keys, ringkasan, checklist. "Simulasi" labels are present wherever NLP figures appear. |
| Built HTML, math | 14 pages: no tags inside `arithmatex` spans, no broken tables (`<p>|`), zero stray `$` (after removing math, code, and tags). Extra LaTeX commands used are core MathJax only. |
| MathJax 3.2.2 in headless Chrome | Formulas of each page extracted to a bare test page and typeset: containers rendered = formulas present for all 14 pages (**4,239 formulas** in total; 31 to 440 per page). **No literal backslash text inside any `mjx-container`** (detects undefined macros). |
| Mermaid 11.17.2 `mermaid.parse` | All 15 diagrams in Level 4 parse (flowcharts and `xychart-beta`). |
| Content audit | Every "selalu unggul/lebih baik" hit is negated ("tidak selalu"). No claim that any algorithm always wins. |

## Defects found and fixed during Level 4

- Part 2: spaced closing delimiter (`$= $ Bandung`) broke math pairing and left raw `$`; three sentences rewritten.
- Control characters (bell, tab) introduced by backslash handling in scripted edits (CLAUDE.md, Part 10); found by a control-character scan and fixed. Final scan clean (only the vendored `mermaid.min.js` matches).
- Output comments that disagreed with real output were corrected (Parts 4, 5, 6, 9, 12).
- Wording or logic errors corrected: Part 8 (which points stump 2 gets wrong), Part 10 (assignment step; "dua kata teratas" listed three), Part 12 (ridge vs degree 3 error sentence).

## Honest caveats in the content

- All NLP figures (Emergency vs Non-Emergency) are **simulated example data**, not real experiments. Part 13 uses a generated 600-message corpus whose generator favours Naive Bayes; the page says so, and results must not be read as a general ranking of algorithms.
- Part 13 uses a stdlib stratified split, which differs from scikit-learn's split; all Part 13 tables were recomputed with the stdlib split so they are internally consistent.
- SVM/kernel, ROC/AUC, and PCA are introductory; SVD is excluded.
- The 10 messages of the shared corpus are tiny: perfect cross-validation scores (Part 11) are shown with an exact lower bound (0.6915) to avoid overclaiming.

## Not performed / unverified

- **Visual rendering inside the real site** (Material theme, light/dark, phone width) for Level 4 pages. The MathJax test used extracted formulas on a bare page.
- **Mermaid visual rendering**: only syntax was verified; rendering is intermittently flaky in headless Chrome here.
- The text-based decision tree in Part 7 and the charts were not checked in dark mode.
- Simulated results depend on the seeds, Python 3.14, and scikit-learn 1.9.0; other versions may differ in the last digits. The scikit-learn blocks were not tested in the project environment.
- Accessibility, other browsers, real devices; MathJax still needs network access (CDN).

---

# Level 5 (Matematika Lanjutan untuk Deep Learning): final QA

Date: 2026-10-17 · Scope: `docs/level-5-matematika-lanjutan-deep-learning/` (index + Parts 1-15). Nothing was committed, pushed, or deployed. `requirements.txt` and `.venv` were not changed; `mkdocs.yml` changed only in `nav`. Outside Level 5 the following were touched: roadmap/README/CLAUDE.md status lines; the status lines in the Level 2, 3, and 4 `index.md`; the "Persiapan Menuju Level 5" and closing sections of Level 4 Part 13 (forward references that were now stale); and, at the user's request, one rendering defect in Level 4 Part 9 (see below).

## Executed

| Check | Result |
|---|---|
| `mkdocs build --strict` (project `.venv`, final run) | **exit 0**, no `WARNING`/`ERROR` lines (only the upstream MkDocs 2.0 notice). |
| Internal links and anchors | Covered by the strict build. The "Prasyarat" section of every part links only to earlier parts or earlier levels (static check: no forward links). |
| Python snippets | All 19 code blocks in Parts 1-15 run without error and their output equals the following `text` block exactly (automatic comparison). Lesson code is stdlib only (`math`, `random`, `itertools`). The Part 15 program (autograd + mini-Transformer + 10-seed loop) runs in about 15 s. |
| Optional NumPy blocks (Parts 2, 3, 12) | Outputs come from the system Python (3.14, NumPy 2.5.2). NumPy is **not** a project dependency and no block is run by the site; each is labelled. PyTorch is not installed and was not run (Part 15 only mentions it). |
| Numbers in worked examples and answer keys | Recomputed by script when each batch was written: exact arithmetic, finite-difference gradient checks (Part 6: all 11 parameters of the 3-2-1 network; Part 15: 6 parameters spread over embedding, W_Q, W_O, W_1, LayerNorm gamma, and the head, agreeing to 6 decimals with autograd), seeded simulations (Parts 8, 12, 15), and an independent simulation for the shrinkage MSE (Part 8: 0.498 vs 0.5). |
| Page structure | Each part has exactly one `#` heading, balanced code fences, and the sections tujuan, prasyarat, kesalahan umum, latihan, kunci, ringkasan, checklist, prasyarat berikutnya (Part 15 ends with "Penutup Level 5" instead). 158 collapsible answer keys: 10 per part for Parts 1-14 and 18 in Part 15. |
| Built HTML, math | 16 pages: no tags inside `arithmatex` spans, no broken tables (`<p>|`), no stray `$`, no control characters. |
| Autoload-dependent macros | Static check (boldsymbol, bm, cancel, color, physics, mathtools macros, ...): none in Level 5. All other macros used are core MathJax. |
| MathJax 3.2.2 in headless Chrome | Formulas of each part extracted to a bare test page and typeset: containers rendered = formulas present for all 15 parts (**3,883 formulas**; 91 to 372 per part). **No literal backslash text inside any `mjx-container`.** |
| Mermaid 11.17.2 `mermaid.parse` | All 15 diagrams in Level 5 parse (flowcharts and `xychart-beta`). |
| Claims audit | Every "selalu"/"pasti" hit is negated or listed as a common mistake. No claim that any optimizer, regulariser, representation, or architecture always wins. NLP examples are labelled simulated/example data; vectors and weights that are hand-picked say so. |

## Defects found and fixed during Level 5

- Part 12 and 13: Greek symbols written with `boldsymbol` showed as raw text on the site (reported by the user). The headless test did not catch it because MathJax autoload succeeded there. Replaced by plain symbols (alpha_{i,:}, gamma, beta, with a sentence saying they are vectors), and a static check plus a CLAUDE.md rule were added.
- Level 4 Part 9 (not Level 5): the same `boldsymbol` use in the soft-margin SVM objective, replaced by the explicit slack variables (xi_1, ..., xi_n) at the user's request; the page still has 439 formulas that all typeset and the built site contains no `boldsymbol` outside the vendored Mermaid file.
- Part 7: the optimum written as `w^\*` is not valid in MathJax (5 containers showed backslash text); changed to `w^{\star}`.
- Part 3: whitespace in the pasted NumPy output did not match the real output; corrected.
- Numerical or wording corrections found while checking: tanh'(-1.5) = 0.1807 (not 0.1808); entropy 0.6390; the factor 0.01766 in the MSE gradient; the generalisation gap at the best epoch (0.935, not "almost 0"); Adam memory ("several times", not "twice"); character vs word length (more than five times); a claim that momentum "returns" later in Part 7 (only six steps were computed).

## Honest caveats in the content

- All NLP data are simulated: the 4 + 2 messages of Part 15, the example vectors of Parts 9-10, 12-14, and the labelled `[CLS]` vectors of Part 14. Hand-picked vectors and weights are stated to be examples, not trained values.
- Part 15 trains a randomly initialised mini-Transformer (210 parameters, no pretraining) on 4 messages. It memorises the training set (loss 0.005) and gets 15 of 20 test answers right over 10 seeds; this is reported as a demonstration of the data flow and of gradient correctness, **not** as accuracy evidence.
- Part 8 results (8 training examples, seed 6, and 20 seeds) depend on seed and size; choosing the epoch with the validation set is optimistic and is stated as such.
- BERT-base facts (d=768, 12 heads, d_ff=3072, 12 layers, vocabulary 30,522, 109,482,240 parameters) are the original English configuration. The parameter total was recomputed from the structure and equals the commonly reported value, but it was **not** checked against a downloaded model. IndoBERT is described only as a family of BERT models trained on Indonesian text whose variants differ; no variant-specific numbers are given.
- Fine-tuning learning-rate and epoch ranges are described as commonly reported, not as rules.
- The roadmap ends at Level 5; no further levels were invented.

## Not performed / unverified

- **Visual rendering inside the real site** (Material theme, light/dark, phone width) for Level 5 pages. The MathJax test used extracted formulas on a bare page, and, as the `boldsymbol` case showed, a bare page can behave differently from the site; only a static macro check covers that gap.
- **Mermaid visual rendering**: only syntax was verified (`mermaid.parse`); rendering is intermittently flaky in headless Chrome here.
- Simulated figures depend on seeds and Python 3.14 (`random.Random(...).gauss`); other versions may differ in the last digits.
- NumPy blocks were not run inside the project environment; no PyTorch code was run.
- Accessibility, other browsers, real devices; MathJax still needs network access (CDN).
- Three stale status sentences from earlier stages (homepage note in `docs/index.md`, closing line of Level 1 Part 12, README "Peta belajar" sentence) were found during this QA and corrected afterwards at the user's request; they now state that Levels 0-5 are available and link to Level 2.
