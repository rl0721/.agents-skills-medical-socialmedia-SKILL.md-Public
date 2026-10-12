# Warning Codes

Use only warnings that materially describe the current run.
Each warning must include `code`, `severity`, and `message`.

## Core warning codes

### `insufficient_context`
Use when the ContextPack is missing required fields or is too thin for reliable discovery or duplication control.
Suggested severity: `high`

### `limited_high_quality_sources`
Use when only a small number of policy-compliant strong sources could be found.
Suggested severity: `medium`

### `search_space_too_narrow`
Use when the combination of pillar, market, language, timeframe, and duplication rules materially narrows the evidence pool.
Suggested severity: `medium`

### `source_overlap_with_recent`
Use when a kept source overlaps with recently used source families or institutions but remains materially useful.
Suggested severity: `medium`

### `angle_overlap`
Use when a surviving prototype remains close to recent prototypes, capsules, or fingerprints.
Suggested severity: `medium` or `high`

### `weak_evidence`
Use when the evidence base supports only a cautious prototype.
Suggested severity: `medium` or `high`

### `single_source_exception`
Use when a prototype is kept despite relying on one screened source.
Suggested severity: `high`

### `missing_global_anchor`
Use when the target angle would normally benefit from a global anchor but none could be secured.
Suggested severity: `medium`

### `missing_market_anchor`
Use when the target market matters materially but the group lacks a usable market anchor.
Suggested severity: `medium`

### `stale_source_risk`
Use when an older but still authoritative source is kept despite freshness pressure.
Suggested severity: `low` or `medium`

### `editorial_risk_high`
Use when the angle carries elevated risk of exaggeration, ambiguity, or avoidable controversy.
Suggested severity: `high`

### `execution_limit_reached`
Use when additional discovery might have improved evidence sufficiency but configured round or count limits were reached.
Suggested severity: `medium`

### `editorial_profile_under_supported`
Use when the provided prototype-stage EditorialProfile points toward framings that the source set cannot support strongly enough.
Suggested severity: `medium`

### `editorial_profile_limited_effect`
Use when an EditorialProfile is present but has only limited effect because the source set strongly favors one safe framing.
Suggested severity: `low` or `medium`

### `profile_evidence_tension`
Use when the requested prototype-stage EditorialProfile creates noticeable tension with evidence support, requiring the framing to stay more conservative than the profile might otherwise suggest.
Suggested severity: `medium`
