# Scoring and Validation Guidance

## Scoring philosophy

Score conservatively.
Do not reward novelty, market fit, or timeliness when evidence sufficiency is weak.
Prefer a lower-count output over inflated scores.

Use integer scores from 1 to 5.

## Prototype scoring

### `novelty_score`
Measure differentiation from recent prototypes, capsules, and fingerprints.
- 1: effectively repetitive
- 2: materially overlaps with recent framing
- 3: somewhat differentiated but familiar
- 4: clearly differentiated
- 5: strongly differentiated and non-obvious

### `credibility_score`
Measure how well the selected evidence group supports the angle.
- 1: weak or indirect support
- 2: thin support with major caveats
- 3: careful prototype is possible
- 4: strong trustworthy support
- 5: exceptionally strong support with little ambiguity

Ceiling rules:
- do not score above 3 for single-source support
- do not score above 3 when no global anchor exists for a claim that should have one
- do not score above 3 when all support comes from one source family

### `relevance_score`
Measure fit to pillar, market, and editorial goal.
- 1: weak fit
- 2: partial fit
- 3: acceptable fit
- 4: strong fit
- 5: directly aligned and editorially useful now

### `editorial_risk_score`
Measure risk from exaggeration, ambiguity, or controversy.
- 1: very low risk
- 2: low risk
- 3: manageable risk
- 4: elevated risk
- 5: high risk, usually reject or hold

### `time_sensitivity`
Treat `time_sensitivity` as an editorial urgency label, not a numeric score. Use it by default for surviving prototypes.
- `high`: short editorial window, usually strongest within about 1 month
- `medium`: near-term window, usually useful within about 1 to 3 months
- `low`: lower urgency or evergreen-leaning, usually still useful after 3 months and up to about 6 months or more

Treat these as approximate windows rather than strict deadlines.


## Prototype-stage EditorialProfile shaping for this test variant

Treat `embedded_runtime_config.editorial_profile` as a secondary shaping layer for the prototype stage only.
It may influence:
- candidate prioritization
- framing preference
- working-angle emphasis
- balance between conceptual and practical orientation

It must not:
- invent unsupported claims
- override evidence quality
- override source support requirements
- override conservative scoring ceilings
- override duplication control
- override market/global anchor logic

### Field intent
- `intellectual_entry`: choose which editorial lens to foreground when multiple supported framings are available
- `novelty_level`: prefer more measured versus more elevated framing within evidence limits
- `practicality_level`: prefer insight-led versus lightly practical framing
- `abstraction_level`: prefer conceptual versus balanced versus concrete framing
- `evidence_density`: when present, favor angles that can be supported more directly if set high
- `risk_posture`: when present, limit how sharp or assertive the framing may become

Use `intellectual_entry`, `novelty_level`, `practicality_level`, and `abstraction_level` as the main profile fields.
Treat `evidence_density` and `risk_posture` as optional modifiers.

## Source-level validation

Validate each source for:
- required fields present
- acceptable `source_type`
- plausible `credibility_tier`
- valid `status`
- valid `url_or_locator`

For `screened` or `approved` sources, also validate that at least one machine-resolvable tracing path exists:
- `canonical_url`, or
- `primary_identifier_type` plus `primary_identifier_value`

If `primary_identifier_type` is present, `primary_identifier_value` must also be present.
If `primary_identifier_value` is present, `primary_identifier_type` must also be present.
If any source fails basic validation, do not use it in an evidence group.

## Evidence-group validation

Validate each group for:
- `source_refs` are non-empty
- all referenced sources exist
- all referenced sources are `screened` or `approved`
- `support_summary` stays within source support
- `source_mix` is coherent
- `evidence_gaps` reflect real weaknesses

## Prototype-level validation

Validate each prototype for:
- at least one selected source
- existing `selected_evidence_group_ref`
- `selected_source_refs` are a subset of the evidence group or explicitly justified
- evidence sufficiency matches `credibility_score`
- source mix and credibility are consistent
- source tracing is strong enough for every selected source to be reopened later
- duplication risk is acceptable
- editorial risk is acceptable
- `time_sensitivity` is present for surviving prototypes unless the caller explicitly suppresses optional editorial labels
- if `time_sensitivity` is present, it uses `low`, `medium`, or `high` consistently with `why_now` and source timing
- if `editorial_profile_applied` is present, it reflects shaping among evidence-supported options rather than unsupported profile-driven invention
- if `profile_fit_note` is present, it accurately describes whether the selected framing aligned with the supplied profile subset or was constrained by the source set
- emit `editorial_profile_under_supported`, `editorial_profile_limited_effect`, or `profile_evidence_tension` when material profile-related constraints affect the run
- status is `proposed` unless the caller explicitly requires another non-final state

## Status selection guidance

### `success`
Use only when at least one prototype is strongly supported and no material warning changes trust in the output.

### `partial_success`
Use when usable output exists but one or more constraints materially reduce confidence.
Common triggers:
- only one prototype survives
- evidence groups are thinner than preferred
- one or more prototypes depend on single-source exceptions
- strong overlap pressure limits novelty
- execution limits prevent filling a known evidence gap

### `failed`
Use when no prototype-grade evidence group survives.
