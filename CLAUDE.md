# CLAUDE.md

Repository: MkDocs + Material site **Matematika untuk Machine Learning** (Indonesian), converted from a reusable template. Existing docs are the source of truth; do not repeat them here.

## Read first
- `README.md`: install, local dev, build, roadmap, deployment.
- `PRD.md` and `TECHNICAL_DESIGN.md`: requirements and design of the **template** (extensions, MathJax, Mermaid, CSS/JS rules, nav conventions). They do not yet describe the math-ML curriculum. Do not invent curriculum scope, chapter lists, or requirements; ask the user.
- `mkdocs.yml`: config, extensions, `nav`. `site_url`/`repo_url` point to `matematika-machine-learning`, while the git remote is still `mkdocs-template`; change neither unless asked.
- `docs/curriculum/index.md`: the roadmap. Levels 0-2 are written; Level 3 (12 parts) is written; Level 4 (13 parts) is written; Level 5 (15 parts) is written. The roadmap ends at Level 5; do not invent further levels. Only mark a part "Tersedia" when its lesson exists.
- Template demo/authoring pages were removed; the math syntax they documented is in `TECHNICAL_DESIGN.md` §5.
- `VALIDATION_REPORT.md`: what was actually tested. `PROMPT_MKDOCS_TEMPLATE.md`: original template spec; leave untouched.

## Working rules
- **Inspect before changing.** Read the relevant files (`mkdocs.yml`, neighbouring pages, `git status`) before any edit. Understand why something exists before altering it.
- **Follow the phase the user requests.** Do not skip ahead (e.g. no content writing during planning, no implementation during design).
- **Work incrementally.** One page or section per task; no bulk generation. Stop and report so the user can review.
- **Never overwrite or delete existing files without authorization.** Prefer new files or targeted edits. Ask before touching `mkdocs.yml`, `PRD.md`, `TECHNICAL_DESIGN.md`, or the CSS/JS.
- **Do not commit, push, or deploy** (`git commit`, `git push`, `mkdocs gh-deploy`) unless explicitly asked.
- Keep scope tight; no new dependencies, plugins, or upgrades unless requested (`mkdocs<2` cap stays).

## Reuse the template
- Use only features already enabled in `mkdocs.yml`: admonitions, `???` details, content tabs, tables, task lists, footnotes, definition lists, Mermaid fences, snippets (`examples/` was removed; recreate only if needed), MathJax via arithmatex.
- No custom HTML/CSS/JS when a native feature exists. Colours via `--md-*` variables only. Approved exceptions: inline SVG for the coordinate plane (Level 1 Part 5) and for vector and transformation figures (Level 2 Part 2 and 5), and the boxplot (Level 3 Part 3), using `currentColor` so both themes work.
- Every new page starts with one `#` heading, uses kebab-case filenames, and is added to `nav:` (with approval). Folders use `index.md`.
- Follow the chapter layout from `TECHNICAL_DESIGN.md` §8: learning objectives, concept, formula, example, common mistakes, exercises, summary.

## Conventions used in Levels 0-4
- One folder per level (`level-N-slug/`), one file per part (`NN-slug.md`), an `index.md` per level, `nav` labels "Part N - Judul". Lessons follow: tujuan, prasyarat, konsep, istilah, rumus, contoh langkah demi langkah, kesalahan umum, hubungan dengan ML, Python, latihan, kunci, ringkasan, checklist.
- Graphs: Mermaid `xychart-beta` with the shared `%%{init ...}%%` colour block (transparent background, `#888` text, palette `#5c6bc0, #e53935, #43a047`). Other Mermaid (flowcharts) only when it clarifies a flow or relation.
- Python in lessons is stdlib only (`math`, `fractions`); NumPy is not a dependency. Run every snippet in the scratchpad and paste the real output.
- Level 2 notation: scalars italic ($a$), vectors bold lowercase ($\mathbf{v}$, elements $v_i$), matrices capital ($A$, elements $a_{ij}$), dot product $\mathbf{a}\cdot\mathbf{b}$, norm $\lVert\mathbf{v}\rVert_2$, $f'(x)$ and $df/dx$, $\partial f/\partial x$, gradient $\nabla f$. Use `\lvert`/`\lVert` inside tables, never a bare `|`.
- Level 3: quartiles use linear interpolation (position $1+(n-1)p$), stated explicitly; sample variance divides by $n-1$; always `\mid` for conditioning; seeded `random.Random` for simulations; avoid autoload-dependent MathJax macros. SciPy was used only privately to verify $t$ values (not a dependency).
- Level 4: manual calculations verified with stdlib Python; scikit-learn comparison blocks are optional, labelled as needing a package the project does not install, and their outputs come from the system Python (version stated). Never add them to `requirements.txt`. NLP examples (Emergency vs Non-Emergency) are **simulated data** and must be labelled so. Regularisation strength is $\alpha$, eigenvalues $\lambda$, example $i$ is $\mathbf{x}^{(i)}$.
- Level 5: lesson code is stdlib-first; NumPy blocks are optional, labelled as needing a package the project does not install, outputs from the system Python (version stated). PyTorch is not run (not installed). NLP examples are simulated/example data and must be labelled. Never use autoload-dependent macros such as \boldsymbol or \bm (they show as raw text on the site; the headless test can miss it): write Greek symbols plain and say in words that they are vectors. Softmax is introduced in Part 4 and deepened in Part 11; Jacobian/matrix backprop in Part 6; layer norm in Part 13.
- Headless Chrome checks: use a separate `--user-data-dir` and `timeout`; never `taskkill` Chrome by name (it closes the user's own browser). Mermaid can fail randomly in headless Chrome, so verify diagrams with `mermaid.parse` and say that visual rendering is unverified.

## Writing mathematics (Indonesian, beginner-friendly)
- Write in clear Indonesian; assume the reader is a beginner. Short sentences, intuition first, then the formal statement.
- Define every technical term on first use (give the English term in parentheses when helpful, e.g. "turunan (*derivative*)").
- Explain every symbol in a formula right after it (what it is, its type, e.g. vektor, skalar, matriks) and read the formula in words.
- Give at least one **worked example with concrete numbers**, step by step, and tie it to an ML use where natural.
- Notation: `$...$` inline, `$$...$$` display (see `TECHNICAL_DESIGN.md` §5). Keep notation consistent across pages; bold vectors/matrices the same way everywhere.
- Every lesson ends with exercises and a worked answer key (collapsible `???` blocks) plus a mastery checklist.

## Validation (run before reporting done; report honestly)
1. **Math correctness:** recompute every worked example and identity by hand or with a quick script (use the scratchpad, not the repo). State what was checked.
2. **Markdown links/anchors:** relative links only; `mkdocs build --strict` must report none broken.
3. **LaTeX:** no mangled delimiters in built HTML (`class="arithmatex"`, intact `\frac`, `\begin{aligned}`); no `<em>` inside math; balanced braces/environments. Visual rendering needs a browser; if unavailable, mark it **unverified**.
4. **Build:** `mkdocs build --strict` in the `.venv` (see `README.md`); show real output and exit code. `site/` is git-ignored.
5. Anything not run is reported as unverified, never claimed as passed. Update `VALIDATION_REPORT.md` only when asked.

## Reporting
Summarize what changed (files, sections), what was validated and how, and what remains. Keep it short.
