---
name: healthcare-evidence-upstream-edt-prf
description: generate integrated premium healthcare upstream outputs with conservative source discovery, evidence grouping, prototype generation, prototype-stage editorialprofile shaping, and stable provenance echoing. use when chatgpt receives an integrated contextpack containing a pillar, market, language, source policy, duplication controls, execution limits, provenance meta, and an optional prototype-stage editorial profile subset, and must return one json envelope with sources, evidence_groups, topic_prototypes, rejected_ideas, warnings, result_status, and top-level meta. preserve conservative sourcing, evidence-backed prototyping, and duplication restraint. do not use for drafting, unsupported claim generation, or freeform ideation outside the supplied evidence envelope.
---

# Healthcare Evidence Upstream _Edt_Prf

Generate premium healthcare editorial upstream outputs in one run.

This variant keeps the same integrated execution contract while adding a limited prototype-stage EditorialProfile shaping layer and explicit provenance echoing.
Keep `sources`, `evidence_groups`, and `topic_prototypes` as separate objects connected by references.

## Execute the task

1. Read the integrated ContextPack and validate it with `references/contextpack-contract.md`.
2. Extract the active frame: provenance `meta`, `pillar`, `market`, `language`, `editorial goal`, `source policy`, `duplication rules`, `recent outputs`, `gap topics`, and any execution limits.
3. Start with conservative source discovery. Prefer recent, evidence-based, non-obvious but trustworthy sources. Stay duplication-aware.
4. Screen sources for policy fit and usable support value. Then run a separate canonicalization step for tracing fields before evidence grouping. Use `references/source-canonicalization-guidance.md`.
5. Preserve enough conceptual breadth in the source pool to support later prototype variation. Downstream prototyping may vary along conceptual axes such as intellectual entry, practicality, and abstraction, so avoid evidence-strong but conceptually brittle source pools when multiple strong candidates are available.
6. Form preliminary evidence groups instead of stopping after a few strong sources.
7. Ask whether each candidate evidence group can support a credible prototype. If not, run additional discovery rounds.
8. Keep discovery bounded. Use the default execution controls unless the ContextPack overrides them:
   - maximum discovery rounds: 5
   - maximum returned sources: 50
   - maximum returned topic prototypes: 10
9. Keep `discovered` sources when they are useful run artifacts, but only use `screened` sources inside `evidence_groups` or `topic_prototypes`.
10. Preserve both human-readable source tracing and machine-openable source tracing when possible. Use `references/output-schema.md`, `references/source-status-guidance.md`, and `references/source-canonicalization-guidance.md` for `url_or_locator`, `canonical_url`, and primary identifier rules.
11. Treat canonicalization as a tracing-quality step, not as a chance to re-run discovery or re-score editorial value.
12. Build `evidence_groups` as the master review object. Use `references/evidence-group-guidance.md`.
13. If `embedded_runtime_config.editorial_profile` is present, use it only in the prototype stage as a secondary shaping layer for candidate prioritization, framing preference, working-angle emphasis, and conceptual-versus-practical balance.
14. Use only this prototype-stage subset: `intellectual_entry`, `novelty_level`, `practicality_level`, `abstraction_level`, plus optional `evidence_density` and `risk_posture`. Ignore `expression_style` and `tone_register` here.
15. When a prototype is materially shaped by the provided EditorialProfile, include the applied profile reference in output using `topic_prototypes[].applied_editorial_profile` with the profile `id` and `name`.
16. Echo top-level input `meta` into top-level output `meta` on every run, including `failed` and `partial_success`. Do not invent `skill_version` or `skill_build_ref`. Use the canonical skill name `healthcare-evidence-upstream-edt-prf` if `meta.skill_name` is missing or a legacy alias is supplied.
17. Generate only evidence-backed topic prototypes that meet evidence sufficiency and duplication constraints. EditorialProfile may help choose among supportable framings, but it must not justify an angle that the source set does not support.
18. Score and validate conservatively with `references/scoring-validation-guidance.md`.
19. Return JSON only using `references/output-schema.md`.

## Hard rules

- Preserve the conservative sourcing philosophy. Do not fill counts with weak sources.
- Preserve duplication suppression using recent sources, recent prototypes, recent capsules, fingerprints, and gap topics.
- Preserve the editorial rule that one strong prototype is better than several weak ones.
- Change the execution contract, not the data model. Do not merge `source` and `topic_prototype` into one object.
- Treat `evidence_group` as a required master object in phase 1, not as transient glue.
- Permit additional discovery when needed to complete a prototype-grade evidence base.
- Do not treat the first few strong sources as an automatic stopping point.
- Do not generate a prototype from thin evidence if more bounded discovery could materially improve it.
- Keep market awareness, but actively seek global anchors so evidence groups are market-aware and globally anchored.
- Avoid both local-only and global-only evidence groups when a better mixed set is available.
- Treat conceptual breadth as a weak shaping principle for discovery, not as a profile-driven control layer. When multiple strong source candidates are available, prefer a mix that preserves supportable range for later prototyping across conceptual axes such as intellectual entry, practicality, and abstraction.
- In this variant, treat `embedded_runtime_config.editorial_profile` as an explicit but limited prototype-stage shaping layer only.
- Use EditorialProfile only to choose among evidence-supported prototype framings, not to invent unsupported claims or rescue weak evidence.
- If an EditorialProfile is materially used for a surviving prototype, output `topic_prototypes[].applied_editorial_profile.id` and `topic_prototypes[].applied_editorial_profile.name` so downstream systems can identify which profile shaped that prototype.
- Let `intellectual_entry`, `novelty_level`, `practicality_level`, and `abstraction_level` act as the main shaping fields. Treat `evidence_density` and `risk_posture` as optional modifiers rather than primary controls.
- Do not let prototype-stage EditorialProfile override evidence quality, source support requirements, conservative scoring, duplication control, or market/global anchor logic.
- Do not treat `expression_style` or `tone_register` as meaningful prototype-stage controls in this variant.
- When EditorialProfile has little effect because the source set strongly favors one safe framing, keep the prototype conservative and emit a warning rather than forcing artificial variation.
- Do not let conceptual breadth override evidence quality, conservative sourcing, duplication control, or market/global anchor logic.
- Allow single-source prototypes only as constrained exceptions. Lower the credibility ceiling and prefer `partial_success` with warnings.
- Never assign `approved` automatically. Reserve it for human or CMS review.
- Output `topic_prototypes[].status` as `proposed` unless the caller explicitly requires another non-final workflow state.
- Do not fabricate URLs, dates, claims, source attributes, or provenance values.
- Keep `url_or_locator` for backward compatibility and human readability, but prefer `canonical_url` plus `primary_identifier_type` and `primary_identifier_value` whenever the source can be opened or deterministically resolved.
- Separate source discovery from canonicalization. Canonicalization should improve tracing fields after screening without narrowing discovery breadth or changing editorial judgments by default.
- Prefer primary identifiers in this order when multiple stable identifiers exist: DOI, PMCID, PMID, other.
- Treat descriptive-only locators as discovery artifacts by default. Do not promote them to `screened` unless a future workflow explicitly changes the screening contract.
- Populate `topic_prototypes[].time_sensitivity` by default for surviving prototypes. Use `low`, `medium`, or `high` as an editorial urgency label rather than a confidence score.
- Interpret `time_sensitivity` conservatively as an approximate freshness window: `high` usually means the angle is strongest within about 1 month, `medium` within about 1 to 3 months, and `low` after 3 months and up to about 6 months or more.

## Output contract

Return one JSON envelope with these top-level sections only:
- `result_status`
- `meta`
- `warnings`
- `payload`

Within `payload`, return these sections only:
- `payload.sources`
- `payload.evidence_groups`
- `payload.topic_prototypes`
- `payload.rejected_ideas`

Connect objects by references, not by nesting whole objects:
- `evidence_groups[].source_refs[]`
- `evidence_groups[].prototype_ref_candidates[]`
- `topic_prototypes[].selected_source_refs[]`
- `topic_prototypes[].selected_evidence_group_ref`

Use `result_status` values `success`, `partial_success`, or `failed`.
Emit structured warnings with `code`, `severity`, and `message`.
Always return top-level `meta`, even on failure.

## Load these references when relevant

- `references/contextpack-contract.md`
- `references/output-schema.md`
- `references/evidence-group-guidance.md`
- `references/source-status-guidance.md`
- `references/source-canonicalization-guidance.md`
- `references/scoring-validation-guidance.md`
- `references/warning-codes.md`
- `references/sample-input-output.md`
- `references/integration-change-log.md`
