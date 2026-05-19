---
title: Targeted Protein Degradation
tags: [protein-degradation, topic]
---

# Targeted Protein Degradation

PROTACs, molecular glues, and next-generation degrader modalities — with computational emphasis.

## Core Questions We Track

1. **Ternary complex modeling**: Can we computationally predict PROTAC-induced E3:target ternary complex geometry?
2. **Degradation prediction**: Can ML models predict DC50/Dmax from structure?
3. **Linker design**: What computational strategies are emerging for linker optimization?
4. **E3 ligase landscape**: What new E3 ligases are being recruited, and how are computational tools assisting?
5. **DMPK for degraders**: Beyond-rule-of-5 ADMET — how are computational models handling this?

## Key Methods to Watch

- **Ternary complex**: MEGA PROTAC, PRosettaC, AlphaFold-based ternary modeling
- **Degradation prediction**: PROTAC-ML, DeepPROTACs
- **Linker optimization**: Fragment-based approaches, generative linkers
- **E3 ligase tools**: Structure-based E3 ligase selection

## Key E3 Ligases

| Ligase | Ligand | Notes |
|--------|--------|-------|
| CRBN | Thalidomide, lenalidomide, pomalidomide | Most studied; molecular glues |
| VHL | VH032, PT-179 | Second most used |
| IAP | LCL161-based | Mitochondrial |
| DCAF11/16 | Covalent ligands | Emerging |
| Viral (VIPER-TAC) | — | Disease-specific; 2025 development |

## Issues Covering This Topic

```dataview
LIST
FROM "issues"
WHERE contains(tags, "protein-degradation")
SORT date DESC
```
