# HCMUS arXiv Jupyter Book — Group 09

The cumulative **Milestone 1: Crawling** and **Milestone 2: Hierarchical Parsing and Standardization** report for Introduction to Data Science, University of Science, VNU-HCM.

- Report repository: [TienThanh1234/arXiV-project](https://github.com/TienThanh1234/arXiV-project), branch `main`.
- Source implementation: [NMKHDL_Project_09, branch 24127121](https://github.com/ndkhoi2407/NMKHDL_Project_09/tree/24127121).
- Verified public report: [Group 09 arXiv Jupyter Book](https://tienthanh1234.github.io/arXiV-project/). Homepage, M1, M2, evidence/Colab pages, structured chart content and referenced assets were verified after deployment.
- Assigned IDs: **2607.03694–2607.08693** (5,000 base IDs).
- Official requirements: [Milestone 1](https://hcmus-cs-ta.github.io/lab-book/intro-ds/statement/crawling/), [Milestone 2](https://hcmus-cs-ta.github.io/lab-book/intro-ds/statement/parsing/).

## Report sections and structure

```text
myst.yml                       # MyST/Jupyter Book 2, book-theme and navigation
index.md                       # overview and headline numbers
m1_crawling.md                  # discovery, source downloads, metadata, references
m2_parsing.md                   # hierarchy, cleanup, normalization, deduplication
crawling_stats.md               # evidence audit, checks and remaining requirements
colab_benchmarks.md             # future CPU-only benchmark protocol
report_utils.py                # loading, formatting and interactive charts
m1_data/, m2_data/              # small historical CSV/JSON report exports
audit_data/                    # historical audit and separately recorded fix evidence
scripts/                       # report validators and safe offline regression checks
src/                           # audited implementation snapshot needed by checks
notebooks/                     # opt-in Colab notebook, excluded from book execution
requirements.txt
package.json, package-lock.json # pinned MyST CLI
.github/workflows/deploy.yml
```

The canonical cumulative implementation is maintained in the separate source repository. The report's `src/` snapshot makes its offline checks and recorded source fingerprints reproducible. No original corpus is needed to build this book.

## Requirements and installation

Validated with **Python 3.14**, **Node.js 24**, and **MyST CLI 1.12.0**. The existing MyST/Jupyter Book 2 architecture is retained. No conda environment is required.

```powershell
git clone --branch main https://github.com/TienThanh1234/arXiV-project.git
cd arXiV-project
python -m venv .venv
$env:PATH = "$PWD\.venv\Scripts;$env:PATH"
python -m pip install -r requirements.txt
npm ci
```

If virtual-environment creation fails during `ensurepip`, use system pip to populate it before setting PATH: `python -m pip --python .venv/Scripts/python.exe install -r requirements.txt`. The report requirements include Jupyter Server/IPython kernel, pandas and Plotly, plus pipeline dependencies used by the mocked offline checks.

## Run locally

With the virtual environment on PATH:

```powershell
npm start
```

Open the URL printed by MyST, normally `http://localhost:3000` (another port is selected when busy). Keep the terminal open; Ctrl+C stops the server.

## Validate and build

```powershell
python scripts/validate_report.py
python scripts/check_parser_logic.py
python scripts/check_pipeline.py
python scripts/check_colab_workflow.py
python scripts/validate_workflow.py
npx myst clean --execute --yes
npm run build
python scripts/validate_html.py
```

`npm run build` executes `myst build --html --execute --strict`. The `--execute` flag produces numerical tables and interactive charts from the real small exports at compilation time. A bare `myst build --html` does not execute fresh Markdown cells. Clear the execution cache after changing data or imported helper code, since those changes may not invalidate MyST's notebook cache automatically.

The static site is written to `_build/html/`. To view that completed artifact:

```powershell
python -m http.server 8000 --directory _build/html
```

Open `http://localhost:8000`. Build-time theme requests may require network access; the book never calls arXiv/Semantic Scholar or starts the full crawler/parser. The executable Colab notebook is excluded from MyST compilation and defaults to plan mode.

## Data provenance and limitations

All numerical figures/tables come from this project's exports. Explanatory method/schema tables are authored documentation rather than fabricated measurements.

- M1 is a report-only snapshot exported on 28 October 2026. Its stored time belongs to a resumed Windows run with discovery skipped, not a cold-start end-to-end benchmark. Original discovery time, peak/average RAM, a single-paper trial and peak required disk are missing.
- M2 is a measured Windows run dated 2 November 2026. All 5,000 publication jobs completed, while 331 versions were empty and 2 failed. Job completion and version parsing success use different denominators.
- The historical corpus still has 17 recorded failed downloads, 14 metadata/discovery date-count mismatches and no retained non-arXiv reference rows. Source-only extraction and provenance improvements in the current code do not repair those historical records automatically.
- CR-only comment handling and duplicate-key BibTeX field merging have regression tests. A temporary copy of `2607.07654v1` now parses; the historical output is unchanged. Plain TeX `2607.05431v1` remains unsupported.
- `audit_data/implementation_checks.json` records current-code verification separately from the historical corpus audit. Export/source fingerprints are checked by the HTML validator.
- Real **Google Colab CPU-only** benchmarks are still required for both milestones. The notebook and runner prepare those experiments; no Colab performance results are claimed.

The original multi-GB corpus is intentionally absent. Small `m1_data/`, `m2_data/` and `audit_data/` inputs must be committed; environments, raw `data/`, downloaded archives, generated tables, caches and `_build/` must not be force-added. `.gitattributes` preserves measured export and pipeline snapshot bytes across Windows/Linux.

`scripts/audit_corpus.py` and `scripts/verify_changes.py` are optional local tools requiring the private corpus. They are not part of the book build or GitHub workflow. To inspect an available corpus, write a new audit destination rather than replacing the historical audit. The code repository documents pipeline execution and report-data export.

## GitHub Pages deployment

The workflow triggers on pushes to `main` or manual dispatch. It installs Python dependencies and the locked MyST package, validates evidence and offline tests, builds HTML with execution enabled, validates generated artifacts, uploads `_build/html/`, and deploys through GitHub Pages. Permissions are `contents: read`, `pages: write` and `id-token: write`.

In the repository's **Settings → Pages**, select **GitHub Actions** as the deployment source. `actions/configure-pages` supplies the Pages base path to MyST through `BASE_URL`; this is needed for the project subdirectory. Review the deployment run and verify the live homepage, both milestone pages and evidence pages before submitting its URL.

Pages is configured to use GitHub Actions. The [deployment workflow](https://github.com/TienThanh1234/arXiV-project/actions/workflows/deploy.yml) completed successfully and public-route/content/asset checks passed. This validates publication; it does not replace the missing Colab performance experiments.

## Submission

For each milestone, the official instructions require the shared public source repository link and the public milestone report link through Moodle. Submit the [source branch](https://github.com/ndkhoi2407/NMKHDL_Project_09/tree/24127121) together with the [M1 report](https://tienthanh1234.github.io/arXiV-project/m1-crawling/) or [M2 report](https://tienthanh1234.github.io/arXiV-project/m2-parsing/) for the corresponding milestone. The cumulative book homepage is https://tienthanh1234.github.io/arXiV-project/. Keep the missing Colab measurements and remaining corpus limitations visible.

Build references: [MyST execution](https://mystmd.org/guide/execute-notebooks), [MyST GitHub Pages deployment](https://mystmd.org/guide/deployment-github-pages).
