# Source Canonicalization Guidance

## Purpose

Treat canonicalization as a separate step after source discovery and initial screening.
Its purpose is to improve source tracing consistency without changing the core editorial judgment that selected the source.

Canonicalization should improve `canonical_url` and stable identifier coverage.
It should not re-run source discovery, re-score evidence, or override evidence-group logic unless a clearly invalid tracing path is detected.

## Workflow position

Use this sequence:

1. discover candidate sources
2. screen sources for policy fit and support value
3. canonicalize tracing fields for screened sources
4. build evidence groups
5. select topic prototypes

Keep this step lightweight and non-blocking by default.

## Core rules

- Do not let canonicalization reduce discovery breadth.
- Do not replace a strong source merely because canonicalization is incomplete.
- Do not change summaries, support claims, novelty judgments, or evidence-group decisions solely because a `canonical_url` could not be improved.
- Keep `url_or_locator` untouched for backward compatibility and human readability.
- Prefer `canonical_url` when a stable representative URL is available.
- Prefer stable primary identifiers when they exist.

## Resolution order

Use this conservative order when building machine-openable tracing.

1. If a DOI exists, set:
   - `primary_identifier_type: doi`
   - `primary_identifier_value: <doi>`
   - `canonical_url: https://doi.org/<doi>`
2. Else if a PMCID exists, set:
   - `primary_identifier_type: pmcid`
   - `primary_identifier_value: <pmcid>`
   - `canonical_url` to the PMC article URL
3. Else if a PMID exists, set:
   - `primary_identifier_type: pmid`
   - `primary_identifier_value: <pmid>`
   - `canonical_url` to the PubMed record URL
4. Else if `url_or_locator` is already a stable official or publisher `http` or `https` URL, reuse it as `canonical_url`
5. Else leave `canonical_url` empty and preserve the existing source decision unless the source lacks any machine-resolvable trace required for `screened`

## Stable URL heuristics

Usually acceptable as `canonical_url`:
- DOI resolver URLs
- PubMed record URLs
- PMC article URLs
- official regulator, government, WHO, society, journal, or publisher landing pages

Usually avoid as `canonical_url` when a better representative URL exists:
- search results pages
- redirector URLs
- tracking-heavy URLs
- unstable direct-download links
- descriptive-only locators

## Non-blocking behavior

Canonicalization should be non-blocking in normal runs.
If canonicalization does not improve a source, keep the source's editorial assessment unchanged.
Only block `screened` use when the source lacks both:
- `canonical_url`, and
- `primary_identifier_type` plus `primary_identifier_value`

If a source remains useful for run logging but not for prototype support, keep it as `discovered`.

## Design intent

This split exists to improve tracing reliability while protecting the skill's stronger behaviors in discovery, grouping, novelty control, and conservative prototype generation.
