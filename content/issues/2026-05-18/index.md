---
title: "Issue 1: May 2026 — A Pipeline Before a Yardstick"
date: 2026-05-18
period: "2026-03-18 to 2026-05-18"
issue_number: 1
tags: [protein-degradation, protac, computational, generative-models, benchmark, ml-clinical]
---

## Field Intelligence Synthesis

**The Moment.** Computational PROTAC design has quietly crossed a threshold: the individual methods of the last five years have hardened into a stack — generator, discriminator, ligase selector — that can in principle run end-to-end without a wet lab in the loop. This issue's three papers sit at exactly those three stations, and read together they show a field that has built the pipeline faster than it has built the evaluation infrastructure to trust it.

**Convergences.** All three papers respond to the same root scarcity — ternary-complex structures are too few to learn from directly — by modeling what cannot be measured: Jin et al. benchmark generative models on a 40-complex set because that is what the PDB will support; Kothakapu et al. lean on ESM sequence embeddings to supply structural context the data lacks; Kumari & Sobhia build an active Parkin conformer in AlphaFold 2 because no experimental one exists. The shared move is computational compensation, and it is now mature enough that the field can attempt CRBN/VHL-alternative E3 ligases entirely *in silico*.

**Tensions.** Two strategies for that compensation are pulling in opposite directions. SE(3)-PROTACs argues you can sidestep missing 3D ternaries by giving the model an equivariant backbone and pretrained protein-language priors; the Parkin work argues geometry is non-negotiable, even if it must be hallucinated. Meanwhile, Kothakapu et al.'s own numbers expose a deeper crack: 80.81% accuracy on a random split collapses to ~65% on cluster and temporal splits — a 15-point gap that suggests the field's default evaluations are quietly overselling generalization. None of these three papers closes the loop with measured degradation; chemical-structure benchmarks, classification accuracy, and MD stability are all standing in for DC50.

**Signal for Practitioners.** If you are building a PROTAC model, anchor your headline metric on cluster- or temporal-split performance, not random-split. If you are reading one, treat AlphaFold-derived active conformers as hypotheses, not substrates. And until a paper reports a degradation readout on a model-prioritized series, treat leaderboard wins as upstream signal — not as evidence the molecule will degrade anything in a cell.

---

## Annotated Bibliography

### Targeted Protein Degradation — Computational Methods

#### Comprehensive Assessment and Benchmark of Deep Generative Models for Proteolysis TArgeting Chimera (PROTAC) Design
**Source**: J. Chem. Inf. Model. | **Date**: 2026-05-11 | **DOI**: [10.1021/acs.jcim.5c03212](https://doi.org/10.1021/acs.jcim.5c03212)
**Authors**: Jin J, Hou T, Liu H, Yao X (Macao Polytechnic University; Zhejiang University)
**Tags**: #protac #generative #benchmark #computational

The authors argue that despite a proliferation of deep generative models being repurposed for PROTAC design, the field has no shared quantitative basis for comparing them. They evaluate representative architectures from two families — PROTAC-specific design models and general linker-design models — on a benchmark of 40 experimentally resolved protein–ligand complex structures, with a general de novo design model as an ablated control. Their contribution is the comparison protocol itself: a unified benchmark and the side-by-side characterization of where each model class succeeds and fails on PROTAC-shaped objectives.

What the data supports: a structured menu of trade-offs across model families, useful as a starting reference for practitioners choosing a generator.
What remains open: this is a structural benchmark, not a degradation benchmark — none of the generated molecules are tested for actual cellular degradation, ternary-complex formation *in vitro*, or DC50 readouts, so a strong benchmark score cannot be conflated with a strong degrader.

**Verdict**: *A useful first attempt at a public yardstick for PROTAC generators, but until benchmarks pair generation with measured ternary-complex or degradation endpoints, leaderboards risk optimizing for chemical plausibility rather than degradative function.*

---

#### SE(3)-PROTACs: Geometric Deep Learning for PROTAC Degradation Prediction
**Source**: Briefings in Bioinformatics | **Date**: 2026-05-04 | **DOI**: [10.1093/bib/bbag228](https://doi.org/10.1093/bib/bbag228)
**Authors**: Kothakapu AR, Madugula S, Gandeed SBS, et al. (Drugparadigm Research Laboratory; Keshav Memorial College of Engineering)
**Tags**: #protac #deep-learning #equivariant #predictor

The authors propose an SE(3)-equivariant transformer that ingests the three PROTAC substructures (warhead, linker, E3 ligand) as molecular graphs invariant to translation and rotation, while pretrained ESM embeddings supply functional context for the target and E3 ligase directly from sequence — sidestepping the chronic problem of missing ternary structures. A pairwise interaction module computes all-pairs target–E3 residue compatibility conditioned on the PROTAC scaffold. Training and evaluation use 1,979 curated samples from PROTAC-DB across three split regimes.

What the data supports: 80.81% accuracy on a random split with reported gains over baselines, and the architectural choice (sequence-driven embeddings + equivariant geometric encoding) is well-motivated for a regime starved of solved ternary complexes.
What remains open: the headline 80.81% drops sharply to 65.62% (cluster split) and 64.08% (temporal split) — the ~15-point gap is the more honest signal of out-of-distribution generalization, and the framing as a "reliable computational pre-filter" leans on the easy split. No prospective wet-lab confirmation of any prioritized degrader is reported.

**Verdict**: *Architecturally the right direction — equivariance plus pretrained protein language model embeddings is the right basis set for ternary-complex modeling — but the cluster- and temporal-split numbers, not the random-split number, should anchor any claim about real-world prioritization.*

---

#### Beyond CRBN and VHL: Parkin-Driven Lysine-Based Ternary Complexes for Selective β3-Tubulin Degradation
**Source**: Computers in Biology and Medicine | **Date**: 2026-05-15 | **DOI**: [10.1016/j.compbiomed.2026.111660](https://doi.org/10.1016/j.compbiomed.2026.111660)
**Authors**: Kumari S, Sobhia ME (NIPER, Mohali)
**Tags**: #protac #alphafold #e3-ligase #molecular-dynamics

The authors take aim at PROTAC chemistry's near-monoculture on CRBN and VHL by computationally exploring Parkin — an RBR-type E3 ligase known to ubiquitinate cytoskeletal and mitochondrial substrates — as a recruiter for targeted protein degradation. Because no full-length active Parkin structure exists in the PDB, they build a catalytically competent model with AlphaFold 2 and use it to generate ternary complexes with β3-tubulin across PROTACs of varying linker length and composition. Candidates are filtered by lysine proximity, dynamic conformational sampling, interface analysis, and tubulin-isoform-specific lysine accessibility, then probed with MD plus tICA and Markov-state modeling.

What the data supports: a tractable in silico pipeline for nominating ternary-complex hypotheses against a non-canonical E3, and a credible argument that Parkin's active state is geometrically permissive for β3-tubulin ubiquitination.
What remains open: the cornerstone model is an AlphaFold structure of a conformation Parkin is biophysically reluctant to adopt — every downstream MSM rests on that priors choice. No PROTAC was synthesized, no degradation measured, no ubiquitination assay shown.

**Verdict**: *A useful provocation for breaking the CRBN/VHL duopoly, but the work is a hypothesis-generation exercise built on an AlphaFold-derived active state — until a Parkin-recruiting PROTAC degrades β3-tubulin in cells, this is geometry, not pharmacology.*

---

## Quick Scan

| # | Title | Track | Key Claim | Link |
|---|-------|-------|-----------|------|
| 1 | Comprehensive Assessment and Benchmark of Deep Generative Models for PROTAC Design | Generative / Benchmark | First quantitative side-by-side of PROTAC-specific vs general linker-design generators on 40 complexes — but no degradation endpoint | [↗](https://doi.org/10.1021/acs.jcim.5c03212) |
| 2 | SE(3)-PROTACs: Geometric Deep Learning for PROTAC Degradation Prediction | Predictor | SE(3)-equivariant transformer + ESM embeddings hits 80.81% random-split accuracy, but only ~65% on cluster/temporal splits — the harder split is the honest number | [↗](https://doi.org/10.1093/bib/bbag228) |
| 3 | Beyond CRBN and VHL: Parkin-Driven Lysine-Based Ternary Complexes for Selective β3-Tubulin Degradation | E3 Ligase Expansion | AlphaFold 2 builds an active Parkin model; MD + MSM nominate degradation-competent ternary complexes — hypothesis-stage, no wet-lab readout | [↗](https://doi.org/10.1016/j.compbiomed.2026.111660) |

---

## About this issue

Bimonthly issue covering computational approaches in targeted protein degradation, 2026-03-18 → 2026-05-18.
Curation by hand from PubMed, bioRxiv, arXiv, and Semantic Scholar; see project [CLAUDE.md](https://github.com/) for workflow.
