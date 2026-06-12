---
title: "Issue 3: June 2026 — Targeted Protein Degradation: permeability"
date: 2026-06-12
period: "2026-05-29 to 2026-06-12"
issue_number: 3
topic: "protac"
tags: [protein-degradation, tpd, computational, ml-clinical, drug-likeness]
---

## Field Intelligence Synthesis

**The Moment**
This set captures a field quietly splitting along a fault line: small-molecule blood-brain barrier (BBB) prediction is racing toward ever-higher benchmark numbers (graphB3, GMC-MPNN), while the hardest and most clinically urgent CNS targets are PROTACs -- bivalent, beyond-rule-of-5 molecules whose permeability those same models were never built to predict.

**Convergences**
All six papers orbit one problem -- moving molecules across membranes and into the brain. Three converge methodologically on graph- and geometry-aware machine learning: graphB3's interpretable graph convolutions, GMC-MPNN's 3D colored-subgraph message passing, and PROTAC-TS's reinforcement-learning-steered linker generation. The PROTAC trio -- the J. Med. Chem. folding study, the Adv. Drug Deliv. Rev. review, and PROTAC-TS -- converges on a mechanistic claim the BBB papers never make explicit: that linker-driven conformational folding, not static polarity, sets permeability.

**Tensions**
The BBB predictors (GMC-MPNN, graphB3) treat permeability as a property of a molecule's structure; the NMR+MD folding study shows it is a property of an environment-dependent conformational ensemble -- the chameleonic behavior the GNNs do not model. And every computational paper here validates in silico only: not one synthesized or assayed a predicted molecule. PROTAC-TS optimizes a predicted score that leaves ~29% of variance unexplained; GMC-MPNN's geometric edge depends on generated conformers it never validates.

**Signal for Practitioners**
If you build BBB models, stop reporting AUC on the MoleculeNet BBBP set and start testing on bRo5 and chameleonic chemistry, where your single-conformer assumptions break -- that is exactly where CNS PROTACs live. If you run assays, the highest-value cheap experiment in this issue is to synthesize even a handful of GMC-MPNN or PROTAC-TS predictions and measure efflux-corrected brain exposure, not passive permeability.

---

## Annotated Bibliography

### Computational Methods & ML

#### Drug targeting to brain: a systematic approach to study the factors, parameters and approaches for prediction of permeability of drugs across BBB
**Source**: Expert Opinion on Drug Delivery | **Date**: 2013-07 | **DOI**: [10.1517/17425247.2013.762354](https://doi.org/10.1517/17425247.2013.762354)
**Authors**: Nagpal et al.
**Track**: Computational Methods

The authors review the landscape of BBB biology, drug transport mechanisms, the physicochemical factors governing permeability, in vitro BBB model systems, and the computational and experimental approaches available for predicting whether a drug crosses the barrier. As a narrative review it does its cataloguing job well: it lays out passive diffusion, carrier-mediated and receptor-mediated transport, the role of efflux, and the determinants captured by the permeability coefficient (Pe), and its expert opinion explicitly calls for systems biologists, network biologists and computational technologists to integrate transporter and physiological complexity into a high-throughput BBB screen. It establishes the conceptual vocabulary the later ML papers operationalize. What it does not provide is any new predictive model, benchmark, or quantitative performance figure; its conclusions are qualitative and, at thirteen years old, predate every modern graph or deep-learning method discussed here. The perfect in vitro model it calls for remains unrealized.

**Verdict**: A useful historical anchor that frames BBB permeability as an integrative systems problem, but it offers no method or data a 2026 modeler can benchmark against -- it poses the question the rest of this issue tries to answer.

---

#### graphB3 - an interpretable graph learning approach for predicting blood-brain barrier permeability
**Source**: Briefings in Bioinformatics | **Date**: 2025-11 | **DOI**: [10.1093/bib/bbaf679](https://doi.org/10.1093/bib/bbaf679)
**Authors**: Kumar et al.
**Track**: Computational Methods

The authors propose graphB3, a parameter-efficient graph convolutional network that predicts BBB permeability directly from atom-level molecular graph features and uses GNNExplainer to surface the substructures driving each prediction. graphB3 reportedly outperforms prior deep-learning baselines, including the multimodal Deep-B3, on BBB permeability classification while using fewer parameters. Its t-SNE embeddings and substructure-frequency analyses show the model separating BBB-permeable from BBB-impermeable molecules and recovering chemically plausible motifs, and the authors ship a public web server plus a standalone GitHub tool -- a real lowering of the barrier to use. The provided account leans heavily on interpretability and relative improvement: it gives no external prospective validation and no experimental confirmation that the highlighted substructures causally govern permeability rather than correlating with training-set labels on the known-imbalanced BBBP dataset. Interpretability here means human-readable, not verified.

**Verdict**: graphB3 makes BBB prediction interpretable and genuinely deployable via a web server, but its substructure explanations remain correlational and untested prospectively before they can be trusted to guide medicinal-chemistry decisions.

---

#### Geometric multi-color message passing graph neural networks for blood-brain barrier permeability prediction
**Source**: Molecular Systems Design & Engineering | **Date**: 2026 | **DOI**: [10.1039/d5me00175g](https://doi.org/10.1039/d5me00175g)
**Authors**: Nguyen et al.
**Track**: Computational Methods

The authors introduce GMC-MPNN, a 3D-aware geometric message-passing GNN that builds weighted colored subgraphs by atom type to encode spatial and long-range atomic interactions for BBB permeability prediction. Under scaffold-based splitting across three benchmark datasets, GMC-MPNN reportedly sets a new state of the art -- AUC-ROC up to 0.947 (and 0.9212 on a second classification set), with regression RMSE 0.5628 and Pearson correlation 0.6947 -- clearly outperforming topology-only MPNN baselines that generalize poorly, one of which showed near-zero correlation. An ablation attributes the gains to both common and rare-but-chemically-significant atom-pair interactions, arguing the 3D signal is doing real work. The gains are demonstrated only on curated public BBBP benchmarks. There is no prospective wet-lab validation, no evaluation on beyond-rule-of-5 chemistry, and the geometric features depend on generated conformers whose accuracy for flexible molecules is never assessed, leaving open whether the 3D advantage holds where conformation actually matters.

**Verdict**: GMC-MPNN convincingly shows that 3D geometry improves in-distribution BBB prediction, but whether that advantage survives on conformationally flexible, bRo5 molecules -- where it would matter most -- remains untested.

---

### TPD: PROTACs, Molecular Glues, Degraders

#### Linker-dependent folding rationalizes PROTAC cell permeability
**Source**: Journal of Medicinal Chemistry | **Date**: 2022-10 | **DOI**: [10.1021/acs.jmedchem.2c00877](https://doi.org/10.1021/acs.jmedchem.2c00877)
**Authors**: Poongavanam et al.
**Track**: TPD

The authors use NMR spectroscopy and molecular dynamics (MD) simulations independently to explain why three closely related, flexible cereblon PROTACs differ sharply in passive cell permeability. Both methods converge on the same mechanism: PROTACs that populate folded conformations with a low solvent-accessible 3D polar surface area in an apolar environment are the more permeable, and the chemical nature and flexibility of the linker govern this folding through intramolecular hydrogen bonds, pi-pi and van der Waals contacts. The independent agreement between an orthogonal experimental ensemble (NMR/NAMFIS) and a computational one (MD) is the paper's real strength, and it motivates the claim that MD can prospectively rank permeability. The conclusion rests on only three analog PROTACs sharing a single chemotype, so generality across E3 ligands and linker classes is unestablished. Permeability is a cell-based surrogate, not measured brain exposure, and the proposed prospective MD ranking is argued but not demonstrated on a blind set. An earlier version appeared as a ChemRxiv preprint in May 2022 (DOI 10.26434/chemrxiv-2022-2gg11).

**Verdict**: This work mechanistically ties linker-dependent folding to PROTAC permeability with rare experiment-simulation agreement, but on three analogs only -- prospective MD ranking still needs a blind, structurally diverse test.

---

#### Engineering brain-penetrant PROTACs: Bridging molecular design and CNS delivery
**Source**: Advanced Drug Delivery Reviews | **Date**: 2026 | **DOI**: [10.1016/j.addr.2026.115876](https://doi.org/10.1016/j.addr.2026.115876)
**Authors**: Li et al.
**Track**: TPD

The authors review strategies for engineering brain-penetrant PROTACs, spanning noninvasive BBB-crossing delivery systems -- viral vectors, engineered exosomes, functionalized nanocarriers, biomimetic cell-membrane vehicles, and intranasal routes -- and rational molecular engineering, including E3 ligase selection, linker polarity and rigidity modulation, ligand optimization, cell-penetrating peptides, and prodrugs. As an open-access review it synthesizes a broad and current cross-section of the CNS-PROTAC delivery problem, correctly centering the field's core obstacle: the bivalent beyond-rule-of-5 structure of PROTACs yields poor BBB permeability and suboptimal pharmacokinetics. Its value is organizational -- it places delivery-system and medicinal-chemistry solutions on the same map and flags AI-accelerated design and targeted protein degradation as routes to otherwise undruggable CNS targets. Being a review, it generates no new data, no head-to-head comparison of delivery platforms, and no quantitative benchmark of which strategy achieves the best brain exposure; efficacy claims rest on the cited primary literature rather than independent evaluation.

**Verdict**: A timely, comprehensive map of brain-penetrant PROTAC strategies that frames the design problem well, but it offers no comparative evidence on which delivery or linker strategy actually wins for a given CNS target.

---

#### Data-driven design of PROTAC linkers to improve PROTAC cell membrane permeability
**Source**: JACS Au | **Date**: 2026-02 | **DOI**: [10.1021/jacsau.6c00033](https://doi.org/10.1021/jacsau.6c00033)
**Authors**: Murakami et al.
**Track**: TPD

The authors develop PROTAC-TS, a linker generative model that couples a chemical language model with reinforcement learning to design PROTAC linkers optimizing cell membrane permeability while preserving PROTAC-likeness. They first build a permeability prediction model reaching R2 = 0.710, then integrate it as a reward to steer generation, reportedly producing novel linkers with high predicted permeability. The contribution is genuine in framing: it is presented as the first linker-design method to explicitly target membrane permeability rather than only synthetic accessibility or generic drug-likeness, addressing the property that most often kills PROTACs. The catch is that success is defined entirely by the model's own predicted permeability, and an R2 of 0.710 leaves roughly 29% of variance unexplained, so the reward is gameable. No generated linker is synthesized, no PROTAC is experimentally assayed, and no degradation or ternary-complex activity is reported -- permeability is optimized in isolation from the function it is meant to enable.

**Verdict**: PROTAC-TS is a clever permeability-aware linker generator, but until its designs are synthesized and assayed it demonstrates optimization of a predicted score, not of real PROTAC permeability or degradation.

---

## Panel Discussion

Panel: Domain Expert A (Medicinal Chemistry, beyond-Rule-of-5) - Domain Expert B (ML / Molecular Property Prediction) - Domain Expert C (CNS Delivery & Pharmacokinetics)

Domain Expert A: Every small-molecule predictor in this set -- graphB3, GMC-MPNN -- implicitly treats permeability as a fixed attribute of a structure. But the J. Med. Chem. NMR-plus-MD study shows the same PROTAC presents a polar, extended face in water and a folded, low-PSA face in apolar media. So when GMC-MPNN computes geometric features from a conformer, whose conformer is it, and does the whole approach quietly assume a single dominant geometry -- an assumption that collapses precisely in the bRo5 space these tools will be asked to triage?

Domain Expert B: graphB3, GMC-MPNN and PROTAC-TS each report strong numbers, but against different labels: binary BBBP class, continuous log-permeability, and a PROTAC permeability surrogate, respectively, on non-overlapping datasets. None share a benchmark or an experimental endpoint. What does 'state of the art' even mean here -- and how much of the reported performance is the field collectively overfitting the curated, salt-stripped MoleculeNet BBBP set, which excludes almost exactly the chemistry of interest?

Domain Expert C: Five of these six papers conflate 'permeability' with 'brain exposure.' But BBB penetration is rate-limited by P-gp efflux, plasma protein binding and unbound brain partitioning -- not passive permeability alone. Only the Adv. Drug Deliv. Rev. review engages active transport and delivery systems. Are we building beautiful predictors of the wrong variable?

Convergence

The common thread is unmistakable: all six papers are trying to get a molecule across a membrane into the brain, and the modeling papers are converging, independently, on graph- and geometry-aware representations as the way to predict it. graphB3 adds interpretability through GNNExplainer; GMC-MPNN adds 3D geometry through colored subgraphs; PROTAC-TS inverts the predictor into a generator. Read together, they trace a clean arc from the 2013 Expert Opinion review's call for integrative, computational BBB screening to its partial fulfillment a decade later. There is an implicit consensus forming that atom-level graph structure, enriched with geometry, is the right substrate for permeability prediction.

But the panel's three questions expose an incompatible assumption running straight through the corpus. The BBB predictors (graphB3, GMC-MPNN) encode a molecule as a structure with a permeability; the J. Med. Chem. NMR-plus-MD result encodes it as a conformational ensemble whose permeability is a property of the environment it finds itself in. These are not compatible ontologies. A model that ingests one RDKit-generated conformer cannot, even in principle, represent chameleonicity -- and chameleonicity is the documented mechanism by which the field's most important CNS molecules cross membranes. GMC-MPNN's geometric edge, never validated on flexible molecules, is therefore most suspect exactly where it is most needed.

The emergent insight -- visible only when these papers are stacked -- is that the field has bifurcated into two communities that share a vocabulary but not a method, and they are drifting apart. The BBB-prediction community optimizes leaderboard metrics on rigid, drug-like public datasets; the PROTAC community hand-builds mechanistic, conformation-aware models on tiny analog series. PROTAC-TS is the one attempt to bridge them, and it inherits the weaker half of each: an in-silico-only generator rewarded by a prediction model with 29% unexplained variance, never experimentally closed. Nobody in this set synthesized and assayed a single predicted molecule. The entire issue is a loop of predictions validated against other predictions.

That sets the open questions sharply. None of these papers measures efflux or unbound brain partitioning, which Expert C correctly names as rate-limiting -- so none predicts brain exposure, only membrane crossing. None tests a BBB GNN on bRo5/PROTAC chemistry, so the generalization gap Expert A and Expert B circle is unquantified rather than closed. And none establishes a shared, experimentally grounded benchmark, without which 'state of the art' is a claim about a dataset, not about biology. The prerequisite for progress is mundane and absent: a common set of synthesized, efflux-characterized molecules -- spanning rigid drugs and flexible degraders -- that every one of these models is forced to predict blind.

---

## Quick Scan

| # | Title | Track | Verdict | Link |
|---|-------|-------|---------|------|
| 1 | Drug targeting to brain: a systematic approach to study… | Computational Methods | A useful historical anchor that frames BBB permeability as an integrative s… | [↗](https://doi.org/10.1517/17425247.2013.762354) |
| 2 | graphB3 - an interpretable graph learning approach for … | Computational Methods | graphB3 makes BBB prediction interpretable and genuinely deployable via a w… | [↗](https://doi.org/10.1093/bib/bbaf679) |
| 3 | Geometric multi-color message passing graph neural netw… | Computational Methods | GMC-MPNN convincingly shows that 3D geometry improves in-distribution BBB p… | [↗](https://doi.org/10.1039/d5me00175g) |
| 4 | Linker-dependent folding rationalizes PROTAC cell perme… | TPD | This work mechanistically ties linker-dependent folding to PROTAC permeabil… | [↗](https://doi.org/10.1021/acs.jmedchem.2c00877) |
| 5 | Engineering brain-penetrant PROTACs: Bridging molecular… | TPD | A timely, comprehensive map of brain-penetrant PROTAC strategies that frame… | [↗](https://doi.org/10.1016/j.addr.2026.115876) |
| 6 | Data-driven design of PROTAC linkers to improve PROTAC … | TPD | PROTAC-TS is a clever permeability-aware linker generator, but until its de… | [↗](https://doi.org/10.1021/jacsau.6c00033) |