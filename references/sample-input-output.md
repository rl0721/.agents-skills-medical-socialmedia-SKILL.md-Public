# Sample Input and Output

## Sample input sketch

```json
{
  "meta": {
    "context_pack_id": 42,
    "run_id": "modrun_456",
    "skill_name": "healthcare-evidence-upstream-edt-prf",
    "skill_version": "1.0.0",
    "skill_build_ref": "git:abc123",
    "market": "japan",
    "language": "ja"
  },
  "task": {
    "pillar_id": "healthy-aging",
    "editorial_goal": "surface premium topic prototypes for evidence-backed aging coverage"
  },
  "embedded_runtime_config": {
    "pillar": "healthy aging coverage for adults 50+ with emphasis on evidence-backed lifestyle, prevention, and clinical-context topics",
    "source_policy": {
      "priority_order": [
        "regulator",
        "guideline",
        "systematic_review",
        "strong_journal_evidence"
      ]
    },
    "duplication_rules": {
      "suppress_recent_fingerprints": true
    },
    "execution": {
      "max_discovery_rounds": 5,
      "max_returned_sources": 40,
      "max_returned_topic_prototypes": 8,
      "preferred_min_sources_per_prototype": 2,
      "allow_single_source_exception": true
    },
    "editorial_profile": {
      "id": 17,
      "name": "clinical nuance balanced practical",
      "intellectual_entry": "clinical_nuance",
      "novelty_level": "measured",
      "practicality_level": "lightly_practical",
      "abstraction_level": "balanced",
      "evidence_density": "high",
      "risk_posture": "balanced"
    }
  },
  "compact_memory_context": {
    "recent_sources": ["who physical activity guidelines"],
    "recent_topic_prototypes": ["walking pace as aging signal"],
    "recent_capsules": ["protein intake in older adults"],
    "topic_fingerprints": [
      {
        "primary_claim": "walking pace predicts healthy aging",
        "key_entities": ["walking pace", "older adults"]
      }
    ],
    "gap_topics": ["screen time guidance for older adults"]
  },
  "briefing": "prioritize angles with clear evidence hierarchy and strong healthcare credibility; avoid generic longevity lifestyle advice"
}
```

## Sample output sketch

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
    "sources": [
      {
        "source_ref": "src-global-screen-time-review",
        "title": "systematic review of sedentary behavior in older adults",
        "source_type": "systematic_review",
        "publisher_or_journal": "journal of aging and physical activity",
        "publication_date": "2025-05-10",
        "url_or_locator": "DOI 10.1123/japa.2025-0001",
        "canonical_url": "https://doi.org/10.1123/japa.2025-0001",
        "primary_identifier_type": "doi",
        "primary_identifier_value": "10.1123/japa.2025-0001",
        "language": "en",
        "market_relevance": "global anchor for older-adult sedentary-behavior evidence",
        "credibility_tier": "high",
        "status": "screened",
        "summary": "provides the strongest synthesis-level evidence base for sedentary behavior risks in older adults.",
        "anchor_role_candidates": ["global_anchor"]
      },
      {
        "source_ref": "src-jp-guidance-frailty",
        "title": "frailty prevention guidance for community-dwelling older adults",
        "source_type": "guideline",
        "publisher_or_journal": "japanese society guidance",
        "publication_date": "2025-09-01",
        "url_or_locator": "https://example.jp/guidance",
        "canonical_url": "https://example.jp/guidance",
        "language": "ja",
        "market_relevance": "japan-specific implementation and care context",
        "credibility_tier": "high",
        "status": "screened",
        "summary": "localizes prevention guidance and screening context for older adults in japan.",
        "anchor_role_candidates": ["market_anchor"]
      }
    ],
    "evidence_groups": [
      {
        "evidence_group_ref": "eg-sedentary-frailty-jp",
        "label": "sedentary behavior and frailty prevention framing for older adults",
        "source_refs": [
          "src-global-screen-time-review",
          "src-jp-guidance-frailty"
        ],
        "prototype_ref_candidates": ["tp-sedentary-frailty-jp"],
        "support_summary": "the global review anchors the risk framing while the japanese guidance localizes why the angle matters in market.",
        "source_mix": {
          "global_anchor_count": 1,
          "market_anchor_count": 1,
          "sharpening_source_count": 0,
          "mix_assessment": "market-aware and globally anchored"
        },
        "evidence_strength": {
          "score": 4,
          "decision_basis": "two screened sources with complementary roles and machine-resolvable tracing.",
          "single_source_exception": false
        },
        "evidence_gaps": []
      }
    ],
    "topic_prototypes": [
      {
        "topic_prototype_ref": "tp-sedentary-frailty-jp",
        "pillar_id": "healthy-aging",
        "working_title": "older adults may need better frailty-screen-time guidance than generic activity advice",
        "working_angle": "foreground clinical nuance and practical frailty-prevention implications in sedentary-behavior coverage, using global synthesis plus japan-specific guidance.",
        "summary": "positions sedentary behavior as a clinically relevant aging issue with a lightly practical and clinically nuanced framing rather than generic wellness advice.",
        "selected_source_refs": [
          "src-global-screen-time-review",
          "src-jp-guidance-frailty"
        ],
        "selected_evidence_group_ref": "eg-sedentary-frailty-jp",
        "status": "proposed",
        "novelty_score": 4,
        "credibility_score": 4,
        "relevance_score": 4,
        "editorial_risk_score": 2,
        "time_sensitivity": "medium",
        "why_now": "recent guidance and synthesis make the angle timely now, while the profile encourages a clinically nuanced and lightly practical framing that should remain useful for the next 1 to 3 months.",
        "editorial_profile_applied": true,
        "applied_editorial_profile": {
          "id": 17,
          "name": "clinical nuance balanced practical"
        },
        "profile_fit_note": "The profile shifted selection toward a clinically nuanced, lightly practical angle without changing the evidence base or support thresholds."
      }
    ],
    "rejected_ideas": [
      {
        "working_title": "walking pace still matters most",
        "working_angle": "reuse walking pace as the core healthy-aging signal",
        "reason": "too close to recent fingerprint and capsule history",
        "related_warning_codes": ["angle_overlap"]
      }
    ]
  }
}
```
