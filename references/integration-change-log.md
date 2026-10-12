# Integration change log

## Current integration notes

- Canonical skill name is now `healthcare-evidence-upstream-edt-prf`.
- Legacy alias `healthcare-evidence-upstream-v-prot-edt-prf-test` may still appear in older callers, but output should normalize `meta.skill_name` to the canonical value.
- Top-level output now always includes `meta` and should echo input provenance fields.
- App-managed `skill_version` is the truth source for version tracking and should be echoed, not invented.
- Optional `skill_build_ref` may be echoed when present.
