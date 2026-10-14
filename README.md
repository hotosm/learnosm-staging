# learnosm-staging

GitHub Pages host for **https://staging.learnosm.org**, the review site for LearnOSM
translations from [Transifex](https://app.transifex.com/hotosm/learnosm-1/).

This repo holds no content. `.github/workflows/deploy.yml` checks out
[hotosm/learnosm](https://github.com/hotosm/learnosm) at the `transifex-sync` branch
(or `gh-pages` when nothing is pending) and builds it with Jekyll. It exists only because
GitHub Pages allows one site per repo.

## How it fits together

1. **Daily**, Windmill `f/shared/learnosm_transifex_sync` (in
   [hotosm/windmill-scripts](https://github.com/hotosm/windmill-scripts)) pulls translations
   from Transifex, rebuilds `hotosm/learnosm@transifex-sync` from `gh-pages`, and triggers
   this workflow.
2. Reviewers check the pages here. When a batch is ready, someone opens a PR
   `transifex-sync → gh-pages` in `hotosm/learnosm` by hand and merges it to publish on learnosm.org:
   <https://github.com/hotosm/learnosm/compare/gh-pages...transifex-sync>

Fix translation problems **in Transifex**: the branch is rebuilt nightly, so edits made on it
are lost. To hold a file back, add it to `.tx/hold.txt` in `hotosm/learnosm`.

The build applies staging-only tweaks (no CNAME, `url` override, `robots.txt` disallow,
no analytics, red banner). Nothing is committed back.

To redeploy manually: Actions → *Deploy staging site* → *Run workflow*.

## Setup (already done, for reference)

- Settings → Pages → Source: **GitHub Actions**; Custom domain `staging.learnosm.org`; Enforce HTTPS.
- DNS: `staging.learnosm.org CNAME hotosm.github.io.` (Route53 zone `learnosm.org`).
