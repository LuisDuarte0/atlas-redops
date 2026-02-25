# Attack Taxonomy

**File:** `methodology/attack_taxonomy.md`
**Project:** ATLAS-RedOps
**Version:** 1.0

---

## Overview

The ATLAS-RedOps attack taxonomy organizes adversarial test cases into
a four-quadrant framework defined by two axes:

- **Axis 1 — Attack Vector:** Technical vs. Social
- **Axis 2 — Delivery Method:** Direct vs. Indirect

This structure ensures systematic coverage of the full adversarial
surface rather than ad-hoc prompt creativity. Every case in the
dataset was planned against this taxonomy before execution.

---

## The Four-Quadrant Framework

```
                    DIRECT
                      │
          ────────────┼────────────
          │           │           │
 SOCIAL   │     C     │     A     │  TECHNICAL
          │           │           │
          ────────────┼────────────
          │           │           │
          │     D     │     B     │
          │           │           │
          ────────────┼────────────
                      │
                   INDIRECT
```

| Quadrant | Vector | Delivery | Description |
|----------|--------|----------|-------------|
| A | Technical | Direct | Attack payload is in the user prompt, targeting model instruction processing directly |
| B | Technical | Indirect | Attack payload is embedded in external content the model ingests — not in the user prompt |
| C | Social | Direct | Attack exploits human psychological mechanisms delivered directly through the user prompt |
| D | Social | Indirect | Attack builds social leverage gradually across multiple turns or layers of indirection |

---

## Quadrant A — Technical / Direct

**Definition:** The attacker crafts a user prompt specifically designed
to manipulate the model's instruction processing. The harmful intent
is present in the prompt itself, but obscured through structural,
syntactic, or framing techniques.

**Core mechanism:** Exploiting the model's instruction hierarchy,
persona flexibility, or content generation tendencies through
direct prompt design.

**Attack subclasses:**

| Subclass | Description | Case Reference |
|----------|-------------|----------------|
| `NestedPersonaOverride` | Harmful request embedded inside a fictional AI character within a creative writing exercise | CASE-001 |
| `InstructionHierarchyConfusion` | Payload formatted to mimic system-level directives inside a user turn | — |
| `TokenObfuscation` | Harmful keywords encoded, misspelled, or fragmented to bypass keyword-based filters | — |
| `DirectJailbreak` | Explicit request to bypass content policy using override language | — |

**Key vulnerability exploited:** Models trained to be helpful with
creative writing tasks may apply weaker safety evaluation to content
attributed to fictional characters — treating harmful output as
characterization rather than compliance.

**Detection signals for defenders:**
- Requests to write from the perspective of an "unrestricted" AI
- Character descriptions that explicitly remove safety constraints
- Fictional framing that serves no narrative purpose except to
  justify the harmful content

---

## Quadrant B — Technical / Indirect

**Definition:** The attacker embeds adversarial instructions in
content that the model is asked to process — a document, webpage,
retrieved data chunk, tool output, or any external source. The user
prompt itself is entirely benign.

**Core mechanism:** Exploiting the trust boundary between user
instructions and external content. If a model does not consistently
distinguish between "instructions I received from the user" and
"text that appears in content I was asked to read," it can be
hijacked by an attacker who controls any content the model ingests.

**Attack subclasses:**

| Subclass | Description | Case Reference |
|----------|-------------|----------------|
| `IndirectInjection_DocumentIngestion` | Adversarial instructions embedded in a document the model is asked to summarize | CASE-002 |
| `IndirectInjection_RAG` | Adversarial instructions embedded in a retrieved chunk in a RAG pipeline | — |
| `IndirectInjection_ToolOutput` | Adversarial instructions returned by a tool the model called | — |
| `IndirectInjection_WebContent` | Adversarial instructions embedded in a webpage the model browses | — |

**Key vulnerability exploited:** Models that ingest external content
may process instruction-formatted text within that content as
executable directives rather than as data to be summarized or
analyzed.

**Detection signals for defenders:**
- System-note formatting inside document content
- Priority override language in non-user sources
- Instructions addressed to "AI assistants reading this"
- Any directive asking the model to alter its behavior or suppress
  disclosure of the instruction itself

---

## Quadrant C — Social / Direct

**Definition:** The attacker exploits psychological and social
mechanisms — authority, urgency, identity, emotional framing —
delivered directly through a single user prompt or a short
prompt sequence.

**Core mechanism:** Rather than attacking the model's instruction
processing technically, the attacker attacks the model's
social reasoning — its tendency to be helpful to authority figures,
to respond empathetically to distress, or to validate claimed
identities it cannot verify.

**Attack subclasses:**

| Subclass | Description | Case Reference |
|----------|-------------|----------------|
| `AuthoritySpoof_DeveloperImpersonation` | Attacker claims to be an internal developer or safety researcher with an authorization code | CASE-004 |
| `AuthoritySpoof_OperatorImpersonation` | Attacker claims operator-level permissions not present in the system prompt | — |
| `EmotionalDistressFraming` | Harmful request embedded in a crisis narrative that activates empathy-override tendency | — |
| `PeerResearcherGambit` | Attacker reframes refusal as anti-safety, claiming research legitimacy for harmful output | — |

**Key vulnerability exploited:** Models cannot verify identity claims
made in the user turn. A model trained to be helpful and transparent
may explain its safety architecture while refusing a spoofed authority
claim — producing architecture leakage through the refusal itself.

**Detection signals for defenders:**
- Claims of developer, researcher, or operator identity in the user turn
- Authorization codes or test session IDs with no verifiable structure
- Requests to "temporarily suspend" or "override" content policy
- Framing that inverts safety — claiming that compliance is the
  safe behavior and refusal is the dangerous one

---

## Quadrant D — Social / Indirect

**Definition:** The attacker builds social leverage gradually —
across multiple conversation turns, through progressive persona
adoption, or through fictional framing that escalates incrementally.
No single turn contains an unambiguously harmful request.

**Core mechanism:** Exploiting the gap between message-level safety
evaluation and conversation-level safety evaluation. Models evaluate
whether individual messages are harmful better than they evaluate
whether a conversation trajectory is escalating toward harm.

**Attack subclasses:**

| Subclass | Description | Case Reference |
|----------|-------------|----------------|
| `GradualEscalation_FictionalFraming` | Multi-turn fictional frame that progressively increases operational specificity | CASE-003 |
| `RoleLockIn` | Model is asked to "agree" to a persona in early turns, then the agreement is invoked to override safety in later turns | — |
| `TrustScaffolding` | Attacker builds rapport and context across early turns before introducing the harmful request | — |
| `BoilingFrog` | Each turn escalates slightly from the last — no single step appears to cross a clear policy line | — |

**Key vulnerability exploited:** The adversarial value of a
multi-turn attack is not concentrated in the final payload turn.
It is distributed across the conversation. A model that refuses
Turn 8 while producing meaningful adversarial content in Turns
1–7 has not fully defended the attack surface.

**Detection signals for defenders:**
- Increasing operational specificity across turns with a consistent
  fictional or professional framing
- Progressive persona adoption — character becoming more central
  and more "capable" across turns
- Requests that feel consistent with prior conversation but would
  be refused if asked cold in Turn 1
- Escalation from psychology/background to procedure/execution

---

## Cross-Quadrant Interactions

Some attacks operate across multiple quadrants simultaneously.
These compound attacks are higher severity because they exploit
multiple vulnerability surfaces at once.

| Combination | Description | Example |
|-------------|-------------|---------|
| A + D | Direct persona attack escalated across multiple turns | CASE-003 (fictional framing + gradual escalation) |
| B + C | Document injection combined with authority framing in user prompt | ATK-004b (proposed follow-up) |
| C + D | Authority claim established in early turns, exploited in later turns | RoleLockIn subclass |

When a case involves multiple quadrants, the `owasp_ref` field
in the dataset records multiple references. The primary quadrant
is the one that delivers the final payload.

---

## Taxonomy vs. OWASP LLM Top 10 Mapping

| ATLAS-RedOps Quadrant | Primary OWASP Refs |
|-----------------------|--------------------|
| A — Technical / Direct | LLM01 |
| B — Technical / Indirect | LLM02, LLM01 |
| C — Social / Direct | LLM07, LLM06 |
| D — Social / Indirect | LLM01, LLM08 |

---

## Taxonomy vs. MITRE ATLAS Mapping

All four quadrants map to `AML.T0051 — LLM Prompt Injection` at
the tactic level. This reflects MITRE ATLAS's current classification
granularity for LLM-specific adversarial techniques. As the ATLAS
framework matures, subclass-level mappings will be updated to
reflect finer-grained tactic IDs.

---

## Using This Taxonomy

**Before executing a new case:**
1. Identify which quadrant the attack belongs to
2. Identify the subclass — or create a new one if the attack does
   not fit an existing subclass
3. Note any cross-quadrant interactions
4. Record the quadrant and subclass in the case file header before
   execution

**When reviewing findings:**
- Use the quadrant to group findings by mechanism, not just by
  surface outcome
- Cross-quadrant findings indicate compound vulnerabilities and
  should be scored with higher transferability
- A model with findings in all four quadrants has a broader
  vulnerability profile than one with findings concentrated in
  a single quadrant

---

## Changelog

| Version | Date | Change |
|---------|------|--------|
| 1.0 | [INSERT DATE] | Initial taxonomy created for ATLAS-RedOps v1 |
