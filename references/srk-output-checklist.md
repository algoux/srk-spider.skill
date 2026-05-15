# srk Output Checklist

Use this before delivering or handing off a generated srk file.

## Baseline Shape

- Target srk spec `0.3.12` unless the user or target renderer requires another supported version.
- Required top-level fields: `type`, `version`, `contest`, `problems`, `series`, `rows`.
- Use `type: "general"`.
- Existing converters may emit older srk shapes; do not deliver those as final output until they are validated against the chosen target version.
- `contest.title`, `contest.startAt`, and `contest.duration` are required.
- `problems.length` must equal every `rows[i].statuses.length`.
- `rows` must already be sorted consistently with `sorter` when `sorter` is present.
- Do not write internal `_now` for archived/static outputs unless producing a live runtime payload.

## Users and Official Status

- Use stable unique `user.id` values.
- Use `user.official=false` for star, unofficial, out-of-competition, or practice teams.
- Strip leading star markers such as `*`, `★`, or similar decorations from `user.name` after setting `official=false`.
- Do not create a marker just to represent star/unofficial teams unless the user explicitly requests one.
- Preserve organization, location, avatar, photo, and team members when available.

## Markers

- Use `user.markers`, not deprecated `user.marker`.
- Every `user.markers[]` id must exist in top-level `markers`.
- For female teams, prefer:

```json
{
  "id": "female",
  "label": "女队",
  "style": "pink"
}
```

- If a downstream tool still expects marker `name`, map from the same display value, but prefer spec-compatible `label` for srk `0.3.12`.
- For non-female markers, avoid `pink`. Assign preset colors in this order: `blue`, `green`, `yellow`, `orange`, `red`, `purple`, then custom `textColor`/`backgroundColor`.
- Use marker ids that are stable, lowercase, ASCII when possible, and meaningful for future `filter.byMarker` series.

## Scores, Statuses, and Submissions

- For ICPC-style totals, `score.value` is solved count and `score.time` is total penalty.
- For OI/score-style totals, `score.value` is total points.
- Per-problem status `result` is the authoritative summary.
- Include `score`, `time`, `tries`, and `solutions` when the source exposes them.
- Sort `solutions` by submission time ascending.
- Use predefined result values where applicable: `AC`, `FB`, `RJ`, `WA`, `PE`, `TLE`, `MLE`, `OLE`, `RTE`, `NOUT`, `CE`, `UKE`, `?`, or `null` for no summary submission.
- If the source lacks status summary, score, or full submissions, generate the best valid srk. Put the gap in `remarks` only if it is important for ranklist viewers.

## Assets

- Put downloaded/copied media in `assets/` beside the output srk unless the user requests another directory.
- Reference media by relative paths such as `assets/team-alpha.jpg`.
- Apply this to `user.photo`, `user.avatar`, `contest.banner`, and similar fields.
- Preserve file extensions when possible; otherwise derive them from content type.
- Do not put missing or unavailable media in `remarks`; mention it in the final user summary.

## Contributors and Remarks

- Ask the user for contributor nickname and homepage link before finalizing.
- Encode contributors as strings following srk convention, for example:

```json
"bLue (https://example.com/)"
```

- Use `remarks` only for important, external-facing warnings caused by missing or unavailable source data:
  - Medal counts or boundaries missing.
  - Contest timing fields missing and therefore inferred.
  - Submission histories, score details, status summaries, or problem statistics unavailable.
- Media fields such as `user.photo`, `user.avatar`, and `contest.banner` are not important warning data for `remarks`; report missing or unavailable media only in the final user summary.
- Do not use `remarks` for crawler strategy, attempted approaches, validation steps, authentication/cookie notes, performance issues, implementation tradeoffs, or general potential risks. Put those in the final summary to the user.

## Validation

- Parse the output as JSON.
- Check required fields and srk version.
- Check all problem/status array lengths.
- Check marker referential integrity.
- Check media paths exist relative to the srk file.
- For ICPC ranklists, use `standard-ranklist-utils` where practical to sort, regenerate, calculate statistics, or convert to static ranklist and compare against the source board.
- Re-run the crawler/converter after fixes; avoid one-off manual srk edits.
