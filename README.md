# Media Vote Tracker

NBA award voting, broken down by media voter. Every ballot for the major NBA
awards since 2013-14, showing which players each voter championed and snubbed
relative to the rest of the panel.

Live: https://jsierrahoopshype.github.io/media-vote-tracker/

## What this repo is

This is the **public, deploy-only** repo. It contains the static site and the
precomputed JSON that powers it. It does **not** contain the source ballot
dataset or the data-cleaning pipeline — those live in the private
`media-vote-tracker-build` repo.

```
docs/
  index.html              Reporter directory (landing page)
  reporter.html           Per-voter profile page
  player.html             Per-player page (coming next)
  data/
    reporters.json        Directory rows
    meta.json             Awards, seasons, player search index
    reporter/<slug>.json  One per voter: champions/snubs board, alignment, outlets
    player/<slug>.json     One per player: supporters/detractors, career arc
  .nojekyll
.github/workflows/deploy.yml
```

## Deploy

GitHub Pages is configured to deploy via GitHub Actions (Settings → Pages →
Source: GitHub Actions). Any push to `main` rebuilds and publishes `docs/`.

There is **no build step on CI** — the JSON is committed already. To update the
data, regenerate it in the private build repo, copy the new `docs/data/` here,
and push.

## Updating data

1. In the private `media-vote-tracker-build` repo, run the pipeline (see its README).
2. Copy the regenerated `docs/data/` folder into this repo's `docs/data/`.
3. `git add docs/data && git commit -m "Refresh data" && git push`.

The site updates automatically.

## Metric definitions

- **Support** — a voter's points for a player minus the average the rest of
  that season's panel gave the same player, summed across every shared ballot.
  Comparison is always within the same award-seasons, so a 2014 voter is judged
  against the 2014 panel, not against all time.
- **Outlier rate** — share of a voter's picks that <=10% of that season's panel
  also made.
- **Alignment** — average cosine similarity of two voters' ballots across the
  award-seasons they both voted in.

Point weights: MVP 10/7/5/3/1; other individual awards 5/3/1; All-NBA 1st/2nd/3rd
= 3/2/1; All-Defensive and All-Rookie 1st/2nd = 2/1.

Fan voting is excluded. Coaches appear under Coach of the Year.
