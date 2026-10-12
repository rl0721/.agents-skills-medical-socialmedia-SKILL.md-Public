# ContextPack Contract

## Purpose

Use this integrated ContextPack to run source discovery and topic prototype generation as one bounded upstream module for premium healthcare editorial work.

## Required fields

Treat these as required for reliable execution.

### Provenance meta
- `meta.context_pack_id`
- `meta.run_id`
- `meta.skill_name`
- `meta.skill_version`

### Core run meta
- `meta.market`
- `meta.language`

### Task fields
- `task.pillar_id`
- `task.editorial_goal`

### Runtime config
- `embedded_runtime_config.pillar`
- `embedded_runtime_config.source_policy`
- `embedded_runtime_config.duplication_rules`
- `embedded_runtime_config.execution`

### Memory and brief
- `compact_memory_context.recent_sources`
- `compact_memory_context.recent_topic_prototypes`
- `compact_memory_context.recent_capsules`
- `compact_memory_context.topic_fingerprints`
- `briefing`

## Optional but strongly useful fields

### Provenance meta
- `meta.skill_build_ref`
- any extra caller-provided tracing keys

### Task and memory
- `compact_memory_context.gap_topics`
- `task.market_focus`
- `task.priority_gaps`
- `task.time_horizon`
- `task.requested_anchor_mix`

### `embedded_runtime_config.editorial_profile`
Optional profile subset for prototype-stage shaping in this variant.

Recommended fields:
- `id`
- `name`
- `intellectual_entry`
- `novelty_level`
- `practicality_level`
- `abstraction_level`

Optional modifier fields:
- `evidence_density`
- `risk_posture`

Do not treat `expression_style` or `tone_register` as meaningful prototype-stage controls in this variant.

## Provenance-meta rule

Treat input `meta` as the truth source for run provenance.
Echo the full input `meta` back as top-level output `meta` on `success`, `partial_success`, and `failed`.
Preserve unknown extra meta fields.
For this skill, the canonical `meta.skill_name` is `healthcare-evidence-upstream-edt-prf`.
If `meta.skill_name` is missing or carries the legacy alias `healthcare-evidence-upstream-v-prot-edt-prf-test`, replace it with the canonical value in output and warn if the mismatch is material.
Do not invent `skill_version`, `run_id`, `context_pack_id`, or `skill_build_ref`.

## Interpretation rules

1. Follow explicit source policy over inferred editorial ambition.
2. Follow duplication rules even when an angle feels strong.
3. Use gap topics to direct discovery, not to justify weak sourcing.
4. Keep market awareness, but explicitly check whether each strong group has enough global anchoring.
5. Permit additional discovery when the first evidence pass is insufficient for a credible prototype.
6. Preserve enough conceptual breadth to support later prototype variation when multiple strong source candidates are available. Treat intellectual entry, practicality, and abstraction as possible downstream variation axes, but use them only as weak shaping reminders rather than explicit control variables during research.
7. In this variant, allow the prototype stage to use `embedded_runtime_config.editorial_profile` explicitly as a secondary shaping layer for candidate prioritization, framing preference, working-angle emphasis, and conceptual-versus-practical balance.
8. Do not let either conceptual-breadth reminders or prototype-stage EditorialProfile override source policy, evidence thresholds, conservative sourcing, duplication rules, scoring ceilings, or market/global anchor logic.
9. When EditorialProfile and the supported source set pull in different directions, keep the evidence-supported framing and emit a warning if the tension is material.
10. Stop when the best remaining discovery is unlikely to improve evidence sufficiency materially or when execution limits are reached.

## Minimum viable context behavior

If one or more required fields are missing, still return valid JSON.
Use `insufficient_context` and downgrade to `partial_success` or `failed` based on how much safe work remains possible.
Always return top-level `meta` with all available input meta fields, even when provenance is incomplete.
