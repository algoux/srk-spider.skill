# Crawler Strategy

Use this reference when selecting an existing converter or designing a new crawler.

## Source Selection

Prefer existing `algoux/rank-spider` scripts before custom work.

| Source shape | First choice | Notes |
| --- | --- | --- |
| xcpcio board | `rank_spider/` Python scripts | Batch crawl xcpcio data, then choose the needed generated items. |
| DOMjudge HTML/PDF | `spidercraft/src/programs/domjudge_*` | Match the source export shape before choosing HTML, old HTML, or PDF tooling. |
| Codeforces Gym | `spidercraft/src/programs/cf-gym.ts` | Use the contest/gym id and inspect CLI help first. |
| PTA | `spidercraft/src/programs/pta-*` | Pick rankings, ranklist, or Excel flow from source format. |
| Nowcoder | `spidercraft/src/programs/nowcoder.ts` | Inspect required ids/cookies/options with `-h`. |
| Hydro event feed | `spidercraft/src/programs/hydro-event-feed.ts` | Prefer event-feed data when available. |
| Unsupported source | Custom JS/Node script | Build a reproducible crawler/converter and run it to emit srk. |

## Existing Script Flow

1. Clone or inspect `https://github.com/algoux/rank-spider`.
2. Read the relevant `README.md` and target script source or `-h` output.
3. Install dependencies in the script workspace if required.
4. Run the converter with explicit input/output arguments.
5. Check the produced srk version and shape. Some legacy scripts may emit older srk fields; upgrade or normalize them with a repeatable post-processing script before delivery.
6. Supplement missing fields only through a repeatable script or a documented post-processing script.

## Custom Crawler Flow

1. Discover data surfaces:
   - Static JSON endpoints and embedded page data.
   - Downloadable exports such as HTML, JSON, XML, CSV, XLSX, or PDF.
   - XHR/fetch calls made by the browser.
   - Modal, drawer, hover, or detail pages exposing submissions, members, photos, location, or organization.
2. Prefer JS/Node for new custom scripts unless the source is already easier in Python.
3. Keep the script deterministic:
   - Accept source URL/file, output path, and optional credentials/cookie path as parameters.
   - Avoid hard-coded local paths.
   - Normalize time zones and elapsed contest times in one place.
   - Write final srk with stable key ordering where practical.
4. Fetch the richest available source data:
   - Contest title, start time, duration, frozen duration, banner, and source links.
   - Problem aliases, titles, links, colors, and statistics when available.
   - User id, name, official flag, organization, location, team members, avatar, photo, markers.
   - Total score, penalty/time, per-problem result, score, time, tries, and full submission histories.
5. Never collapse source detail just because the visible table is enough. If source UI exposes more data through interaction or detail endpoints, crawl it when feasible.

## Media Handling

- Create `assets/` next to the output srk.
- Download or copy `user.photo`, `user.avatar`, `contest.banner`, and similar media there.
- Use relative srk values such as `assets/banner.png`.
- Generate collision-safe filenames from stable ids plus sanitized extensions.
- If a media URL fails, keep a `remarks` note rather than silently dropping the issue.

## When to Ask the User

Ask only for choices that cannot be inferred safely:

- Authentication material or permission to use a browser session.
- ICPC time precision and ranking-time precision.
- How to identify star/unofficial teams when the source is ambiguous.
- Contributor nickname and homepage link.
- Medal boundaries when the source does not expose them.
