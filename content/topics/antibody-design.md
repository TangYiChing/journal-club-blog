---
title: Antibody Design & Engineering
tags: [antibody, topic]
---

# Antibody Design & Engineering

This topic covers computational and experimental advances in antibody discovery, design, optimization, and characterization.

## Core Questions We Track

1. **Structure prediction**: Can we reliably predict antibody-antigen complex structures? (ABodyBuilder, AlphaFold-Multimer derivatives)
2. **Binding prediction**: Can sequence-based models predict binding affinity and specificity?
3. **Generative design**: Are diffusion/LLM-based antibody generators producing developable candidates?
4. **Developability**: ADMET/biophysical properties — are computational filters useful pre-experiment?
5. **Experimental validation**: What assays are being used to validate computationally-designed antibodies?

## Key Methods to Watch

- **Structure prediction**: ABodyBuilder3, AlphaFold-Multimer, IgFold, ESMFold-Ab
- **Binding prediction**: Siamese networks on SAbDab, sequence-based affinity models
- **Docking**: HDOCK, EquiDock, RosettaAntibodyDock
- **Generative**: RFdiffusion-Ab, AbDiffuser, ProteinMPNN for CDR design
- **Datasets**: SAbDab, OAS, Absolut!, CoV-AbDab

## Useful Benchmarks & Databases

- [SAbDab](http://opig.stats.ox.ac.uk/webapps/newsabdab/sabdab/) — Structural Antibody Database
- [OAS](https://opig.stats.ox.ac.uk/webapps/oas/) — Observed Antibody Space
- [CoV-AbDab](https://opig.stats.ox.ac.uk/webapps/covabdab/) — SARS-CoV-2 antibodies

## Issues Covering This Topic

```dataview
LIST
FROM "issues"
WHERE contains(tags, "antibody")
SORT date DESC
```
