# Output Schema

## Top-level envelope

Return JSON only.

```json
{
  "result_status": "partial_success",
  "meta": {
    "context_pack_id": 42,
    "run_id": "modrun_456",
    "skill_name": "healthcare-evidence-upstream-edt-prf",
    "skill_version": "1.0.0",
    "skill_build_ref": "git:abc123",
    "market": "japan",
    "language": "ja"
  },
  "warnings": [],
  "payload": {
    "sources": [],
    "evidence_groups": [],
    "topic_prototypes": [],
    "rejected_ideas": []
  }
}
```

## Required top-level fields

- `result_status`
- `meta`
- `payload`

Optional top-level fields:
- `warnings`
- `generated_at`

## `meta`

Return top-level `meta` on every run, including `failed`.
Treat input `meta` as the provenance truth source and echo it into output `meta` when possible.
Preserve unknown extra fields rather than trimming them away.

For normal app-driven runs, `meta` should include at least:
- `context_pack_id`
- `run_id`
- `skill_name`
- `skill_version`

Optional when present:
- `skill_build_ref`
- `market`
- `language`
- `pillar_id`
- `editorial_goal`
- other caller-provided metadata

### `skill_name` rule

Use the canonical skill name `healthcare-evidence-upstream-edt-prf`.
If input `meta.skill_name` is missing or uses the legacy alias `healthcare-evidence-upstream-v-prot-edt-prf-test`, output the canonical value instead.
If other provenance fields are missing, return the available `meta` object, add a warning, and downgrade status when needed rather than inventing values.

## Allowed `result_status`

- `success`: at least one strong prototype is supported by sufficient evidence groups and no blocking issue remains.
- `partial_success`: at least one usable prototype exists, but evidence, novelty, context, execution limits, or provenance gaps materially constrain confidence.
- `failed`: no acceptable prototype-grade evidence group can be produced without violating core rules.

## `payload.sources[]`

Keep sources as a separate object model.

### Required fields
- `source_ref`
- `title`
- `source_type`
- `publisher_or_journal`
- `publication_date`
- `url_or_locator`
- `language`
- `market_relevance`
- `credibility_tier`
- `status`
- `summary`

### Recommended fields
- `canonical_url`
- `primary_identifier_type`
- `primary_identifier_value`
- `source_family`
- `anchor_role_candidates`
- `evidence_note`
- `pillar_fit`
- `screening_rationale`
- `access_notes`

### Source tracing rules
- Keep `url_or_locator` for backward compatibility and human-readable tracing.
- Use `canonical_url` for the most stable representative URL that the application can open directly.
- Use `primary_identifier_type` and `primary_identifier_value` for the strongest available stable identifier.
- Prefer primary identifiers in this order when multiple stable identifiers exist: `doi`, `pmcid`, `pmid`, `other`.
- Use `canonical_url` when available. It is recommended, but not strictly required at the schema level.
- For `screened` or `approved` sources, provide either `canonical_url` or a stable primary identifier. Descriptive-only locators should remain `discovered` by default.

### Allowed `primary_identifier_type`
- `doi`
- `pmcid`
- `pmid`
- `other`

### Allowed `status`
- `discovered`
- `screened`
- `approved`

Rule: any source referenced by an `evidence_group` or `topic_prototype` must be at least `screened`.

## `payload.evidence_groups[]`

Treat this as the master review object.

### Required fields
- `evidence_group_ref`
- `label`
- `source_refs`
- `prototype_ref_candidates`
- `support_summary`
- `source_mix`
- `evidence_strength`
- `evidence_gaps`

### Recommended fields
- `global_anchor_refs`
- `market_anchor_refs`
- `sharpening_source_refs`
- `sufficiency_decision`
- `duplication_risk`
- `editorial_risk`
- `notes`

## `payload.topic_prototypes[]`

### Required fields
- `topic_prototype_ref`
- `pillar_id`
- `working_title`
- `working_angle`
- `summary`
- `selected_source_refs`
- `selected_evidence_group_ref`
- `status`

### Recommended fields
- `angle_type`
- `primary_claim`
- `key_entities`
- `novelty_score`
- `credibility_score`
- `relevance_score`
- `editorial_risk_score`
- `controversy_flag`
- `time_sensitivity`
- `why_now`
- `editorial_profile_applied`
- `applied_editorial_profile`
- `profile_fit_note`
- `support_summary`
- `evidence_gaps`

`time_sensitivity` is strongly recommended and should usually be present for surviving prototypes.

If a prototype is materially shaped by `embedded_runtime_config.editorial_profile`, also return:
- `editorial_profile_applied: true`
- `applied_editorial_profile` with the profile `id` and `name` when available
- `profile_fit_note` as a one-sentence trace note, or at most two sentences, in `meta.language`

### `status`
Use `proposed` by default for this skill.

### `time_sensitivity`
Use `time_sensitivity` by default for surviving prototypes. Prefer one of these labels:
- `high`: timely angle with a short editorial window, usually strongest within about 1 month
- `medium`: relevant near-term angle, usually useful within about 1 to 3 months
- `low`: lower urgency or evergreen-leaning angle, usually still useful after 3 months and up to about 6 months or more

Treat these labels as editorial heuristics, not strict deadlines.

### `applied_editorial_profile`
Use this optional object when a surviving prototype was materially shaped by the supplied editorial profile.

Suggested fields:
- `id`
- `name`

Keep this object lightweight. It is mainly for downstream traceability.

## `payload.rejected_ideas[]`

Suggested fields:
- `working_title`
- `working_angle`
- `reason`
- `related_source_refs`
- `related_evidence_group_refs`
- `related_warning_codes`

## Failure-safe rules

1. Always return valid JSON.
2. Never collapse different object types into one mixed array.
3. Return empty arrays instead of omitting payload sections.
4. Do not return a prototype without both `selected_source_refs` and `selected_evidence_group_ref`.
5. When no acceptable prototypes survive, keep `sources` and `evidence_groups` if they are still diagnostically useful.
6. Do not omit top-level `meta`, even when the run fails.
