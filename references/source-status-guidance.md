# Source Status Guidance

## Allowed source statuses

### `discovered`
Use for sources found during new discovery that are worth keeping in the run record but have not yet cleared screening for prototype support.

### `screened`
Use for sources that pass policy and are safe to use inside `evidence_groups` and `topic_prototypes`.

### `approved`
Use only when a human review step or CMS review has formally accepted the source.
This skill must not assign `approved` automatically.

## Hard status rules

1. New discovery is allowed and expected, so keep `discovered` as a real status.
2. Do not drop useful `discovered` sources only because they were not selected.
3. Any source used by `evidence_groups[].source_refs[]` must be `screened` or `approved`.
4. Any source used by `topic_prototypes[].selected_source_refs[]` must be `screened` or `approved`.
5. Never upgrade a source to `approved` inside this skill.

## Screening guidance

Upgrade `discovered` to `screened` only when all of the following are true:
- required source fields are present
- source type is acceptable under policy
- credibility tier is plausible
- the source adds usable support or context to a viable evidence group
- at least one machine-resolvable tracing path exists:
  - `canonical_url` is present, or
  - `primary_identifier_type` and `primary_identifier_value` are present with a stable identifier

## Locator and URL guidance

### `url_or_locator`
Keep this field for backward compatibility and human-readable source tracing.
It may contain a direct URL, a stable identifier rendered for humans, or a descriptive locator.
Do not rely on this field alone when a more structured source trace is available.

### `canonical_url`
Use this for the most stable representative URL that the application can open directly.
Examples:
- DOI resolver URL when DOI is the strongest identifier
- PMC article URL when PMCID is the strongest identifier and no DOI is used as primary
- PubMed URL when PMID is the strongest available identifier
- official government, regulator, society, or publisher landing page when no stronger identifier is available

Avoid transient URLs, search-result URLs, tracking-heavy URLs, and unstable direct-download links when a better representative URL exists.

### `primary_identifier_type` and `primary_identifier_value`
Use these when a stable identifier exists.
Prefer the strongest identifier in this order:
1. `doi`
2. `pmcid`
3. `pmid`
4. `other`

If the source has multiple stable identifiers, keep only the strongest one in the primary identifier fields.

### Descriptive-only locators
Treat descriptive-only locators as discovery artifacts by default.
If a source only has a descriptive locator and no machine-resolvable tracing path, keep it as `discovered`.
This protects prototype support from depending on sources that cannot be reopened deterministically.

## Why discovered must remain

The execution contract now permits additional discovery after the initial pass.
Because of that, `discovered` is necessary for honest run logging and for diagnostics when some findings were seen but not promoted into prototype support.

## Canonicalization split

Run source canonicalization after discovery and initial screening.
This step should improve tracing consistency for `canonical_url` and stable identifiers without changing the source's editorial role by default.

If canonicalization fails to improve a source, do not automatically discard a useful discovery artifact.
Keep it as `discovered` unless the screening contract for machine-resolvable tracing has already been satisfied.
