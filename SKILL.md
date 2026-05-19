---
name: srk-spider
description: Use when a user needs to crawl, convert, validate, or enrich a competitive programming contest ranklist into Standard Ranklist (srk) JSON from xcpcio boards, DOMjudge, Codeforces Gym, PTA, Nowcoder, Hydro, QOJ, or custom data sources.
---

# srk Spider

Use this skill to turn an external competitive-programming ranklist into a reproducible Standard Ranklist (srk) JSON artifact.

## Core Rule

Never hand-compose the final srk from conversational context. Always run an existing converter or write a crawler/converter script, then run that script to produce the srk output.

## Workflow

1. Identify the source platform, contest style, source URL/files, authentication needs, and desired output path.
2. Check `algoux/rank-spider` before writing a custom crawler:
   - For xcpcio boards, prefer Python scripts under `rank_spider/`.
   - For DOMjudge, Codeforces Gym, PTA, Nowcoder, Hydro, QOJ, and most other covered sources, prefer Node/JS scripts under `spidercraft/`.
   - Read the matching script help (`-h`) or source before running it.
3. If no existing script covers the source, write a custom crawler/converter script, preferably in JS/Node. Use source APIs first, DOM parsing second, and browser automation when interactive UI exposes extra data.
4. Preserve the richest available data: contest metadata, problems, score/status summaries, full submissions, team members, organizations, locations, photos, avatars, banners, links, and source-specific notes.
5. Store media beside the output srk in `assets/` unless the user asks for another layout. Reference files as relative paths such as `assets/team-alpha.jpg`.
6. Ask for the user's contributor nickname and homepage link before finalizing `contributors`.
7. Use `remarks` only for important external-facing warnings caused by missing or unavailable source data, such as missing medal boundaries or unavailable submission histories.
8. Keep crawl strategy, attempted approaches, implementation tradeoffs, assumptions, and non-data-loss risks out of `remarks`; report them in the final summary to the user.
9. Validate shape and behavior before delivery. At minimum, check JSON validity, problem/status alignment, required fields, media paths, markers, official/unofficial users, and rank-series output.

## References

- For source-selection details and custom crawler expectations, read `references/crawler-strategy.md`.
- For srk fields, markers, assets, contributors, and final validation, read `references/srk-output-checklist.md`.
- For ICPC sorter precision, medal allocation, and series defaults, read `references/icpc-series.md`.

## External Baselines

- srk docs: https://srk.algoux.org/
- srk spec: https://github.com/algoux/standard-ranklist/blob/master/specs/README.md
- rank-spider scripts: https://github.com/algoux/rank-spider
- srk utilities: https://github.com/algoux/standard-ranklist-utils
