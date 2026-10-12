# Evidence Group Guidance

## Role of the evidence group

Treat `evidence_group` as the central quality-control object.
It is the unit for:
- evidence sufficiency
- prototype support
- review flow
- run logging
- UI inspection
- import readiness

Do not treat it as temporary glue.

## Core philosophy

A topic prototype should be proposed only when there is an evidence group that can support it credibly.
The evidence group exists to make that support legible.

## Minimum evidence expectation

### Default rule
Use at least 2 screened sources to support one prototype.

### Preferred rule
Use 3 or more screened sources when a stronger mix is available.

### Exception rule
Allow a single-source prototype only when the source is unusually strong and the editorial value is still high.
If used:
- lower the prototype credibility ceiling
- emit warnings
- usually downgrade to `partial_success`
- set `single_source_exception` in the evidence assessment

## Target source mix

Aim for this mix when possible.

### `global_anchor`
A source that provides internationally credible grounding, such as a regulator, major guideline, systematic review, meta-analysis, or strong journal synthesis.

### `market_anchor`
A source that localizes relevance to the target market, such as a local regulator, local clinical guidance, national surveillance data, or market-specific policy signal.

### `sharpening_source`
A source that adds specificity, mechanism, implementation nuance, controversy context, or timing relevance without acting as the only evidence anchor.

## Group formation workflow

1. Cluster screened sources by claim architecture or supportable editorial angle.
2. Test whether the cluster has enough evidence diversity to survive duplication and editorial risk checks.
3. Check whether the cluster is market-aware and globally anchored.
4. Identify missing anchor roles or evidence gaps.
5. Run another bounded discovery round if the gap looks fillable.
6. Promote the cluster to an evidence group only when it is usable for review and prototype support.

## Sufficiency heuristics

Evidence groups are usually stronger when they have:
- at least one global anchor
- at least one market anchor when the topic materially varies by market
- at least one sharpening source when nuance matters
- mutually reinforcing but non-duplicative sources
- a support summary that explains the angle without over-claiming

## Evidence gaps

Record real gaps explicitly.
Typical gaps include:
- no market anchor
- no global anchor
- evidence too old
- only one screened source
- source family concentration
- unresolved controversy
- claim stronger than support

## Suggested labels

Keep labels compact and angle-like.
Examples:
- `screen-time guidance shift in older adults`
- `market uptake vs evidence lag in continuous glucose monitoring`
- `protein supplement messaging and sarcopenia evidence quality`
