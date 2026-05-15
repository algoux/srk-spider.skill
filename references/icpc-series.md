# ICPC Series

Use this reference for ICPC-style ranklists, including XCPC regional boards and DOMjudge-like sources.

## Required User Questions

Ask the user for sorter precision before finalizing an ICPC srk:

- `sorter.config.timePrecision`: recommend `"min"` unless the source board clearly calculates penalties in seconds or another unit.
- `sorter.config.rankingTimePrecision`: recommend leaving it absent unless the original board ranks ties at a coarser precision.

If the source does not clearly identify star/unofficial teams, ask how to detect them.

## Sorter Defaults

Use the ICPC sorter unless the contest is not ICPC-scored:

```json
{
  "algorithm": "ICPC",
  "config": {
    "penalty": [20, "min"],
    "timePrecision": "min"
  }
}
```

Only include `rankingTimePrecision` and rounding fields after user confirmation or source verification. Preserve source-specific no-penalty results when known; otherwise rely on srk defaults.

## Default Series

For ICPC ranklists, provide at least:

- `"#"`: primary ICPC ranking.
- `"R#"`: absolute ranking.
- `"S#"`: organization-deduplicated ranking, using `UniqByUserField` with `field: "organization"` when organization data exists.

Use short titles exactly as above unless the user requests localized or descriptive names.

## Medal Allocation

If medal counts are known, encode the source rule with ICPC `count` or `ratio` options and matching `segments`.

If medal counts cannot be crawled:

```json
{
  "title": "#",
  "segments": [
    { "title": "Gold Medalist", "style": "gold" },
    { "title": "Silver Medalist", "style": "silver" },
    { "title": "Bronze Medalist", "style": "bronze" }
  ],
  "rule": {
    "preset": "ICPC",
    "options": {
      "count": { "value": [0, 0, 0] }
    }
  }
}
```

Add a `remarks` note explaining that medal boundaries or medal counts were unavailable and should be set manually.

## Ratio Verification

When using ratio-based medal allocation:

1. Encode the ratio, rounding, denominator, and tie policy from the source.
2. Use `@algoux/standard-ranklist-utils` to convert to a static ranklist.
3. Compare computed segment assignment against the original board.
4. If the source and srk result differ, adjust only after identifying the cause: official filtering, denominator choice, tie handling, rounding, or source-specific medal policy.
5. Put unresolved differences in `remarks`.

## Marker-Specific Rankings

If the user asks for independent rankings for groups such as female teams or regional groups, define separate ICPC series with:

```json
{
  "rule": {
    "preset": "ICPC",
    "options": {
      "filter": {
        "byMarker": "female"
      },
      "count": { "value": [1, 1, 1] }
    }
  }
}
```

Use the actual marker id and medal counts for the requested group.
