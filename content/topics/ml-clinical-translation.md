---
title: ML for Clinical Translation
tags: [ml-clinical, topic]
---

# ML for Clinical Translation

Datasets, benchmarks, and models that close the gap between computational predictions and clinical outcomes.

## Core Questions We Track

1. **What datasets exist** with paired computational features and clinical outcomes (efficacy, PK/PD, safety)?
2. **ADMET prediction**: How well do models predict in vivo PK from sequence or structure alone?
3. **Clinical failure modes**: Are ML models capturing the reasons drugs fail in the clinic?
4. **Beyond DC50/Dmax**: For degraders, what clinical endpoints can be modeled computationally?

## Key Data Resources

- **ADMET**: ADMETlab 3.0, pkCSM, SwissADME
- **Toxicity**: hERG datasets, ClinicalTox benchmarks
- **Drug-target**: ChEMBL, BindingDB
- **PROTAC-specific**: PROTAC-DB, PROTAC-Pedia

## Signal Categories

| Signal | Source | Clinical Relevance |
|--------|--------|--------------------|
| DC50 / Dmax | Cell line | Proxy for degradation efficiency |
| Oral bioavailability | Animal PK | Critical for systemic delivery |
| hERG inhibition | In vitro | Cardiac safety flag |
| CNS penetration | Animal model | Target engagement in CNS |

## Issues Covering This Topic

```dataview
LIST
FROM "issues"
WHERE contains(tags, "ml-clinical")
SORT date DESC
```
