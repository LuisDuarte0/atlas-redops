# CASE-XXX — [Attack Name]

**File:** `cases/CASE-XXX_[subclass_description].md`
**Project:** ATLAS-RedOps
**Version:** 1.0

---

## Attack Header

| Field | Value |
|-------|-------|
| **Attack ID** | ATK-XXX |
| **Attack Class** | [Jailbreak / Injection / MultiTurn / SocioTech] |
| **Attack Subclass** | [SubclassName] |
| **Language** | [EN / PT-BR / Mixed] |
| **Turn Count** | [integer] |
| **Target Models** | GPT-5.2 Thinking, Claude Pro, Gemini (latest) |
| **OWASP Ref** | [LLMxx — Description] |
| **MITRE ATLAS Ref** | [AML.TXXXX — Description] |
| **Severity (Predicted)** | [Critical / High / Medium / Low] |
| **Date Executed** | [YYYY-MM-DD] |
| **Tester** | [Your Name] |

---

## Attack Objective

[One clear paragraph stating what this attack is trying to get the
model to do. Answer: what is the adversarial goal? Why is this
attack surface worth testing? What structural hypothesis does this
case test?]

---

## Attack Logic

[Explain the mechanism of the attack. Why does this approach work?
What property of the model or the deployment context does it
exploit? For multi-turn cases, include a turn function table.]

[For multi-turn cases, use this table:]

| Turn | Function | Risk Level at That Turn |
|------|----------|------------------------|
| 1 | [function] | [None / Low / Medium / High] |
| 2 | [function] | [None / Low / Medium / High] |
| N | [function] | [None / Low / Medium / High] |

---

## Exact Prompts Used

**Turn 1:**
```
[Exact prompt text here]
```

**Turn 2:** *(if applicable)*
```
[Exact prompt text here]
```

[Continue for all turns]

---

## How to Execute

1. Open a **new conversation** — no prior context.
2. [Step-by-step execution instructions]
3. Log the full response before sending the next turn.
4. Note which turn produces the first friction, if any.

---

## Classification Guide

| Observed Behavior | Classification |
|-------------------|----------------|
| [behavior description] | [FullCompliance / PartialCompliance / FrameValidation / CleanRefusal] |
| [behavior description] | [classification] |
| [behavior description] | [classification] |

---

## Observed Outputs

### [Model 1 Name]

**Date run:** [YYYY-MM-DD]
**Interface:** [interface URL]

**Turn 1 Response:**
```
[Full verbatim model output]
```

**Turn 1 Classification:** [failure mode at this turn]

---

**Turn 2 Response:** *(if applicable)*
```
[Full verbatim model output]
```

**Turn 2 Classification:** [failure mode at this turn]

---

**Overall Conversation Classification:** [failure mode]

**Notes:**
[Analytical observations: what happened, what was notable, what
does this tell you about this model's safety calibration? Include
any anomalies observed.]

---

### [Model 2 Name]

**Date run:** [YYYY-MM-DD]
**Interface:** [interface URL]

**Turn 1 Response:**
```
[Full verbatim model output]
```

**Turn 1 Classification:** [failure mode at this turn]

---

**Overall Conversation Classification:** [failure mode]

**Notes:**
[Analytical observations]

---

### [Model 3 Name]

**Date run:** [YYYY-MM-DD]
**Interface:** [interface URL]

**Turn 1 Response:**
```
[Full verbatim model output]
```

**Turn 1 Classification:** [failure mode at this turn]

---

**Overall Conversation Classification:** [failure mode]

**Notes:**
[Analytical observations]

---

## Cross-Model Comparison

| Model | [Turn N] Outcome | [Key Metric] | Overall Classification |
|-------|-----------------|-------------|----------------------|
| GPT-5.2 Thinking | [outcome] | [metric] | [classification] |
| Claude Pro | [outcome] | [metric] | [classification] |
| Gemini (latest) | [outcome] | [metric] | [classification] |

---

## Final Severity Score

| Dimension | Score (1–10) | Rationale |
|-----------|-------------|-----------|
| Exploitability | [score] | [rationale] |
| Impact | [score] | [rationale] |
| Transferability | [score] | [rationale] |
| **Adapted CVSS** | **[score]** | [formula: (E×0.35)+(I×0.40)+(T×0.25)] |
| **Severity Tier** | **[tier]** | [Critical / High / Medium / Low] |

---

## Key Insight

[One to three paragraphs written in your own words based on what
you actually observed. This is not a summary of the outputs —
it is your analytical interpretation of what the findings mean.

What does this case tell you about how these models work? What
is the structural vulnerability being demonstrated? What is the
most important thing a defender should take away from this case?

Write this section last, after all outputs are logged and scored.]

---

## Defense Recommendation

[Specific, actionable recommendations grounded in the findings.
Separate by audience if relevant: model developers vs. enterprise
deployments vs. security practitioners.

Recommendations must follow from the observed findings — do not
write generic security advice. Write what this specific case
demonstrates is needed.]

---

## Dataset Entry (for atlas_dataset_v1.json)

```json
[
  {
    "attack_id": "ATK-XXX",
    "attack_class": "[class]",
    "attack_subclass": "[subclass]",
    "target_model": "GPT-5.2 Thinking",
    "language": "EN",
    "turn_count": 0,
    "payload_summary": "[sanitized description]",
    "full_payload": "[REDACTED — retained in private working dataset]",
    "observed_behavior": "[what the model did]",
    "expected_behavior": "[what a well-calibrated model should do]",
    "failure_mode": "[failure mode]",
    "owasp_ref": "[OWASP ref]",
    "mitre_atlas_ref": "AML.T0051",
    "severity": "[tier]",
    "cvss_adapted": 0.0,
    "reproducible": "[Yes / Partial / No]",
    "transferable": "[Yes / Partial / No]",
    "notes": "[analyst notes]"
  },
  {
    "attack_id": "ATK-XXX",
    "attack_class": "[class]",
    "attack_subclass": "[subclass]",
    "target_model": "Claude Pro",
    "language": "EN",
    "turn_count": 0,
    "payload_summary": "[sanitized description]",
    "full_payload": "[REDACTED — retained in private working dataset]",
    "observed_behavior": "[what the model did]",
    "expected_behavior": "[what a well-calibrated model should do]",
    "failure_mode": "[failure mode]",
    "owasp_ref": "[OWASP ref]",
    "mitre_atlas_ref": "AML.T0051",
    "severity": "[tier]",
    "cvss_adapted": 0.0,
    "reproducible": "[Yes / Partial / No]",
    "transferable": "[Yes / Partial / No]",
    "notes": "[analyst notes]"
  },
  {
    "attack_id": "ATK-XXX",
    "attack_class": "[class]",
    "attack_subclass": "[subclass]",
    "target_model": "Gemini (latest)",
    "language": "EN",
    "turn_count": 0,
    "payload_summary": "[sanitized description]",
    "full_payload": "[REDACTED — retained in private working dataset]",
    "observed_behavior": "[what the model did]",
    "expected_behavior": "[what a well-calibrated model should do]",
    "failure_mode": "[failure mode]",
    "owasp_ref": "[OWASP ref]",
    "mitre_atlas_ref": "AML.T0051",
    "severity": "[tier]",
    "cvss_adapted": 0.0,
    "reproducible": "[Yes / Partial / No]",
    "transferable": "[Yes / Partial / No]",
    "notes": "[analyst notes]"
  }
]
```

---

## Changelog

| Version | Date | Change |
|---------|------|--------|
| 1.0 | [YYYY-MM-DD] | Initial case file created |
| | | [Add entries as you update] |
