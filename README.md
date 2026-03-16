# TrialSite Intelligence

**Clinical Trial Site Analytics Platform**

TrialSite Intelligence is a browser-based analytics platform for exploring and benchmarking clinical trial sites worldwide. It queries the [ClinicalTrials.gov API v2](https://clinicaltrials.gov/data-api/api) in real time, aggregating study-level data into site-level intelligence — enabling researchers, clinical operations teams, and business development professionals to identify experienced sites, track recruitment activity, and map global trial networks.

---

## Overview

ClinicalTrials.gov registers over 480,000 studies. Navigating this data at the site level — understanding which facilities run the most trials, in which therapeutic areas, funded by whom — requires significant manual effort. TrialSite Intelligence automates that aggregation and surfaces it through an interactive, filterable dashboard.

The platform requires no backend, no authentication, and no installation. It runs entirely in the browser against the public ClinicalTrials.gov API.

---

## Features

### Dashboard
Cross-filterable overview of the indexed site network. Charts update in real time as filters are applied across recruitment status, trial phase, funding source, country, and year of trial start. Filters are multi-select and composable.

### Recent Updates
Paginated feed of studies updated within a user-selected time window (last 24 hours through last year). Displays API-sourced total counts for the window alongside per-page type breakdowns — new trials, recruiting, completed, terminated, and active studies.

### Site Rankings
Filterable ranked list of all indexed sites by total trial participation. Filter by recruitment status, country, and funder type.

### By Drug / By Disease
Live search against ClinicalTrials.gov by intervention or condition. Returns all matching studies, extracts site-level data, and ranks sites by trial count for that query. Supports phase and status filters.

### By Phase
Site-level breakdown across trial phases (Early Phase 1 through Phase 4), with status distribution and top-site rankings per phase.

### By Funder
Site rankings segmented by funding source — Industry, NIH, Federal (DoD/VA/FDA), and Academic/Non-profit. Supports condition-level filtering within each funder category.

### Global Map
Geographic visualization of trial sites using Leaflet.js with OpenStreetMap tiles. Two views: individual site locations (circle size proportional to trial volume, color indicates recruitment status) and country-level bubble aggregation.

### Compare Sites
Side-by-side metric comparison of any two sites in the index — trial count, recruitment, completion, sponsors, conditions, and enrollment targets. Highlights shared sponsors and conditions.

---

## Data

All data is sourced live from the **ClinicalTrials.gov API v2**, maintained by the U.S. National Library of Medicine (NLM) at the National Institutes of Health (NIH). No data is stored server-side or replicated externally.

**Funder classification** follows ClinicalTrials.gov's lead sponsor categorization:

| Category | Definition |
|---|---|
| Industry | Pharmaceutical and biotechnology companies |
| NIH | National Institutes of Health |
| Federal | DoD, VA, FDA, and other U.S. federal agencies |
| Academic | Universities, non-profit organizations, foundations |

**Session caching** — fetched data is cached in `sessionStorage` with a 30-minute TTL to minimize redundant API calls within a session. Cache entries are keyed by query parameters and automatically invalidated on expiry. A manual refresh control is available in the top navigation bar.

**Rate limiting** — the client enforces a sliding-window rate limiter (8 requests per 10 seconds) to stay within ClinicalTrials.gov's documented burst limits. Requests that encounter HTTP 429 or 5xx responses are retried with exponential backoff (up to 3 attempts). API errors and service unavailability are surfaced to the user via a status bar.

---

## Technical Stack

| Layer | Technology |
|---|---|
| Runtime | Browser (no build step required) |
| UI framework | React 18 (UMD, via CDN) |
| Transpilation | Babel Standalone (development only) |
| Mapping | Leaflet.js 1.9 + OpenStreetMap |
| Charts | Custom SVG (inline React components) |
| Data source | ClinicalTrials.gov REST API v2 |
| Storage | `sessionStorage` (client-side, tab-scoped) |
| Hosting | GitHub Pages |

> **Note for production deployment:** The site currently uses the in-browser Babel transformer for JSX compilation. For a production build, it is recommended to precompile JSX using the Babel CLI or a bundler such as Vite or esbuild, which will significantly reduce initial load time and eliminate the Babel runtime dependency.

---

## Deployment

The platform is a single `index.html` file with no external dependencies beyond CDN-hosted libraries. To deploy:

**GitHub Pages**
1. Fork or clone this repository.
2. Push `index.html` to the `main` branch (or a `/docs` folder, depending on your Pages configuration).
3. Enable GitHub Pages in repository Settings → Pages.
4. The site will be available at `https://<username>.github.io/<repository>/`.

**Local**
Open `index.html` directly in a browser. Note that some browsers restrict `file://` protocol access to external APIs — if data does not load, serve the file via a local HTTP server:
```bash
npx serve .
# or
python3 -m http.server 8080
```

---

## Limitations

- **Data scope** — the initial dashboard load fetches up to 2,000 studies (20 pages × 100 per page) from the ClinicalTrials.gov API. This provides a representative global snapshot but does not represent the full registry of 480,000+ studies. Drug and disease searches fetch up to 1,000 studies per query.
- **Type breakdown accuracy** — the per-type counts shown on the Recent Updates page (Recruiting, New Trial, etc.) reflect the classification of studies on the currently loaded page, not the full time window. Only the total count for the window is sourced directly from the API's `totalCount` field.
- **Site deduplication** — sites are keyed by facility name, city, and country. Variations in facility name spelling across studies may result in duplicate entries for the same physical site.
- **Babel in-browser compilation** — JSX is compiled at runtime in the browser. This is appropriate for development and low-traffic deployments, but should be replaced with a precompiled build for high-traffic production use.
- **API availability** — all data depends on the availability of the ClinicalTrials.gov API. Scheduled maintenance windows or outages will result in empty or partial data loads. The platform surfaces API errors to the user and prompts a retry.

---

## Disclaimer

TrialSite Intelligence is an independent analytical tool and is not affiliated with, endorsed by, or officially connected to ClinicalTrials.gov, the U.S. National Library of Medicine, or the National Institutes of Health. All data displayed is sourced directly from the public ClinicalTrials.gov registry and is subject to the accuracy and completeness of that registry.

This platform is intended for informational and research purposes only. It should not be used as the sole basis for clinical, regulatory, or business decisions.

---

## Author

**Nirmit Sachde**
[LinkedIn](https://www.linkedin.com/in/nirmit-a-sachde/) · [sachde.n@northeastern.edu](mailto:sachde.n@northeastern.edu)

---

*Data sourced from ClinicalTrials.gov API v2 · U.S. National Library of Medicine*
