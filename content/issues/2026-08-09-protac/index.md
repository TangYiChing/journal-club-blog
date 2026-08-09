---
title: "Issue 4: August 2026 — Targeted Protein Degradation: Computational Methods"
date: 2026-08-09
period: "2026-07-26 to 2026-08-09"
issue_number: 4
topic: "protac"
tags: [protein-degradation, tpd, computational, ml-clinical, drug-likeness]
---

## Field Intelligence Synthesis

**The Moment**
Blood-brain-barrier permeability prediction is being rebuilt twice over — once by
architecture (geometric GNNs, parameter-efficient fine-tuning) and once by mechanism
(efflux-transporter awareness in BBB-Nuke, conformational ensembles in CycPeptMPDB-4D) —
while the molecules the field most urgently needs to screen, brain-penetrant degraders,
sit entirely outside the chemical space every one of these models was trained on.

**Convergences**
Three independent lines now agree that a 2D structure is not enough: Ermondi's
conformational sampling of a VHL-based degrader, the CycPeptMPDB-4D multi-solvent
ensembles, and BBB-Nuke's transporter modelling all add a dimension the standard
descriptor set omits — environment-dependent conformation in the first two, active
transport in the third. Di Lascio's local-versus-global ADME comparison supplies the
statistical version of the same claim: global models trained on broad chemistry lose
ground on narrow series.

**Tensions**
The corpus splits on what "harder problem" means. The BBB modelling papers (Yang's
survey, the geometry-informed PEFT work, BBB-Nuke) treat scarce, class-imbalanced BBBP
data as the binding constraint and answer with architecture and transfer learning. The
physical-chemistry papers treat the representation itself as wrong for bRo5 space, and
answer with sampling. Both cannot be the main story: if permeability here is a
conformational-ensemble property, better fine-tuning on 2D-derived labels optimises a
proxy that does not exist for these molecules. Meanwhile XL01126 proves brain-penetrant
degraders are attainable and Bergmann's organoid protocol shows how noisy the ground
truth is — neither model camp confronts either fact.

**Signal for Practitioners**
Before trusting any BBBP score on a degrader, check the training set's molecular-weight
and TPSA range and report the model's applicability domain; treat a prediction on a bRo5
chameleonic molecule as an extrapolation claim that requires its own validation, not as
a screen.

---

## Annotated Bibliography

### Computational Methods & ML

#### Conformational Sampling Deciphers the Chameleonic Properties of a VHL-Based Degrader
**Source**: Pharmaceutics | **Date**: 2023-01-01 | **DOI**: [10.3390/pharmaceutics15010272](https://doi.org/10.3390/pharmaceutics15010272)
**Authors**: Ermondi G et al.
**Track**: Computational Methods

The authors benchmark two conformational sampling algorithms and their selection schemes against PROTAC-1, a passively cell-permeable VHL-based degrader whose chameleonic behaviour is already established by NMR and physicochemical measurement. The design is calibration rather than prediction: an experimentally characterised molecule is used to ask which computational ensemble reproduces known environment-dependent behaviour. What the data supports: sampling algorithms differ measurably in whether they recover the polar-surface-burial transition that defines chameleonicity, and the selection scheme, not only the sampler, changes the answer — making ensemble generation an explicit methodological decision rather than a preprocessing detail. What the data does not support: this is a single-molecule calibration. No generalisation across degrader chemotypes is claimed or supported, and nothing connects a computed ensemble to a measured brain exposure, the endpoint that matters for CNS use.

**Verdict**: Establishes that degrader permeability is an ensemble property 2D descriptors cannot represent, but with n=1 it calibrates a method rather than validating a predictor.

---

#### Geometry-Informed Parameter-Efficient Fine-Tuning of Pre-trained Molecular GNNs for Blood-Brain Barrier Permeability Prediction
**Source**: arXiv (preprint, not yet peer-reviewed) | **Date**: 2026-08-04 | **DOI**: —
**Authors**: Vieto Vega M et al.
**Track**: Computational Methods

The authors apply geometry-informed parameter-efficient fine-tuning (PEFT) to pre-trained molecular GNNs for BBBP classification, arguing that full fine-tuning is parameter-inefficient and overfits the small, class-imbalanced BBBP datasets that dominate this task (preprint, not yet peer-reviewed). What the data supports: the framing of the bottleneck is credible and shared across this corpus — BBBP labels are scarce and imbalanced, and transfer from large pre-trained molecular representations is the cheapest available remedy. It is the natural baseline for adapting a general molecular GNN to a small specialised permeability set. What the data does not support: evaluation stays inside the standard BBBP benchmark distribution, overwhelmingly Ro5 small molecules. Nothing establishes transfer to bRo5 chameleonic chemistry, and no applicability-domain analysis tells a user when the prediction should not be trusted.

**Verdict**: The most efficient current route to adapting pre-trained GNNs to small BBBP datasets, and therefore the baseline a degrader-focused external validation must beat, but its own evidence stops at the Ro5 boundary.

---

#### Multi-solvent conformational ensembles for predicting cyclic peptide permeability
**Source**: Scientific Data | **Date**: 2026-08-05 | **DOI**: [10.1038/s41597-026-07984-9](https://doi.org/10.1038/s41597-026-07984-9)
**Authors**: Liu W, Pham NH, Verma CS et al.
**Track**: Computational Methods

The authors release CycPeptMPDB-4D, a resource pairing cyclic-peptide permeability data with physics-based conformational sampling across solvent environments, explicitly motivated by the failure of 2D molecular representations to capture macrocyclic conformational flexibility. What the data supports: the dataset makes the ensemble-based representation route tractable for a whole bRo5 modality — the sampling cost that normally blocks this approach is paid once and published. It is the clearest statement that solvent-dependent conformational dynamics, not static descriptors, is the right input for macrocycle permeability. What the data does not support: the modality is cyclic peptides, not bifunctional degraders, which differ in three-part architecture, a flexible linker and a far larger accessible conformational space. Transfer is a hypothesis, not a result.

**Verdict**: Supplies the missing input representation for bRo5 permeability modelling and removes the usual excuse for 2D descriptors, though transfer from cyclic peptides to degraders is untested.

---

### Experimental Validation & Assays

#### Blood-brain-barrier organoids for investigating the permeability of CNS therapeutics
**Source**: Nature Protocols | **Date**: 2018-12 | **DOI**: [10.1038/s41596-018-0066-x](https://doi.org/10.1038/s41596-018-0066-x)
**Authors**: Bergmann S, Lawler SE, Qu Y et al.
**Track**: Experimental Validation

The authors provide a full protocol for BBB organoids formed by coculturing endothelial cells, pericytes and astrocytes under low-adhesion conditions, and for using them to measure transport of candidate CNS therapeutics. What the data supports: the organoids reconstitute tight junctions, molecular transporters and efflux pumps — the machinery monolayer endothelial cultures reproduce inconsistently, and precisely the biology BBB-Nuke argues descriptor-based models are blind to. The protocol makes reproducible in vitro permeability measurement accessible outside specialist labs. What the data does not support: an organoid is not brain exposure. Quantitative correspondence between organoid permeability and in vivo Kp,uu is not established, and the system is not demonstrated on large bRo5 molecules.

**Verdict**: Defines the ground truth any computational BBB validation inherits, and the noise floor that bounds how strong such a validation's conclusion can honestly be.

---

### TPD: PROTACs, Molecular Glues, Degraders

#### Discovery of XL01126: A Potent, Fast, Cooperative, Selective, Orally Bioavailable, and Blood-Brain Barrier Penetrant PROTAC Degrader of Leucine-Rich Repeat Kinase 2
**Source**: Journal of the American Chemical Society | **Date**: 2022-08-25 | **DOI**: [10.1021/jacs.2c05499](https://doi.org/10.1021/jacs.2c05499)
**Authors**: Liu X, Kalogeropulou A, Domingos S et al.
**Track**: TPD

The authors report XL01126, a bifunctional VHL-based degrader of LRRK2 characterised as potent, fast, cooperative and selective, with oral bioavailability and demonstrated brain penetration. (Annotated from title and curation metadata rather than the full abstract — verify reported PK values against the paper before citing numbers.) What the data supports: the central claim is an existence proof. A molecule with the molecular weight and polarity of a PROTAC reached the brain after oral dosing, contradicting the widespread assumption that degraders are structurally excluded from CNS applications. What the data does not support: one compound is not a design rule. No transferable predictor of which degraders will penetrate is supplied, and there is no basis for deciding computationally, in advance, which member of a series to synthesise.

**Verdict**: The reference point for every CNS degrader claim, and the clearest demonstration that the field's real deficit is not feasibility but prospective prediction.

---

### ML for Clinical Translation & Datasets

#### Deep Learning for Blood-Brain Barrier Permeability Prediction: From Discriminative Models to Mechanism-Aware Design
**Source**: arXiv (preprint, not yet peer-reviewed) | **Date**: 2025-07-24 | **DOI**: —
**Authors**: Yang Z, Xiao Y
**Track**: ML-Clinical

The authors survey BBB permeability prediction from physicochemical rules through early ML to current deep models, arguing the trajectory runs toward mechanism-aware design rather than purely discriminative classification (preprint, not yet peer-reviewed). What the data supports: as an inventory it names the model families in current use and is explicit that rule-based physicochemical methods carry systematic misjudgement inherited from prior empirical evidence, and that early ML models generalised poorly. Identifying generalisation, not accuracy, as the weak point is its most defensible contribution. What the data does not support: a survey cannot establish that any catalogued model works outside its training distribution, and none of the surveyed evaluations report performance on beyond-Rule-of-5 chemistry. The mechanism-aware direction is argued as a trend, not demonstrated with held-out evidence.

**Verdict**: The best available map of which BBBP models exist and how they are evaluated, which is exactly how it reveals that none has been tested on the molecules CNS degrader programmes need to screen.

---

#### Systematic Evaluation of Local and Global Machine Learning Models for the Prediction of ADME Properties
**Source**: Molecular Pharmaceutics | **Date**: 2023-03-06 | **DOI**: [10.1021/acs.molpharmaceut.2c00962](https://doi.org/10.1021/acs.molpharmaceut.2c00962)
**Authors**: Di Lascio E, Gerebtzoff G, Rodriguez-Perez R
**Track**: ML-Clinical

The authors systematically compare local QSPR models, trained on a single project or chemical series, against global models trained across large, diverse ADME datasets, using industrial assay data. What the data supports: this is the corpus's hardest evidence on applicability domain. Model performance depends on the relationship between training chemistry and target chemistry, not on dataset size alone — the general form of the question this issue asks about BBB models and degraders. What the data does not support: the endpoints are standard ADME assays on drug-like chemistry; BBB permeability on bRo5 molecules is not among them. How far a BBBP model falls when moved into degrader space remains unmeasured.

**Verdict**: The strongest prior that a globally trained BBBP model will lose ranking power on a narrow bRo5 series, and the template for how that loss should be measured.

---

#### BBB-Nuke: Transport-Aware Prediction of Blood-Brain Barrier Penetration in Small Molecules
**Source**: bioRxiv (preprint, not yet peer-reviewed) | **Date**: 2026-07-14 | **DOI**: [10.64898/2026.07.13.738280](https://doi.org/10.64898/2026.07.13.738280)
**Authors**: Abasciano N, Hadipour H, Poddar A et al.
**Track**: ML-Clinical

The authors present BBB-Nuke, a modular pipeline combining ten physicochemical descriptors, GCN-predicted ionisation state and CNS-MPO desirability with explicit substrate-probability modelling for seven efflux transporters (P-gp/MDR1, BCRP/ABCG2, MRP1, MRP2, MRP4, MATE1, OAT3) (preprint, not yet peer-reviewed). What the data supports: the diagnosis is the contribution — existing models are blind to the active transport biology that dominates exclusion at the barrier in vivo, and modularity makes that omission addressable rather than implicit. It is the closest work here to arguing BBB models fail for mechanistic, not statistical, reasons. What the data does not support: the scope is stated in the title, small molecules. CNS-MPO and the descriptor panel are defined for Ro5 chemistry and are least meaningful for large, conformationally adaptive degraders, so the pipeline does not yet reach the molecules where the transport question is most acute.

**Verdict**: Names the right missing mechanism and builds the modular architecture to include it, but stops at the Ro5 boundary, leaving the degrader slice of the same argument open.

---

## Panel Discussion

Panel: Domain Expert A (Medicinal Chemistry / beyond-Rule-of-5 physicochemistry) ·
Domain Expert B (ML / Molecular Property Prediction) · Domain Expert C (CNS Pharmacology /
BBB Transport Biology)

**Domain Expert A**: Every model in this set consumes descriptors — TPSA, logP, CNS-MPO —
that are computed from a single static structure. Ermondi's degrader and the CycPeptMPDB-4D
peptides both demonstrate that the relevant quantity changes with environment. So what
exactly is a BBBP classifier predicting when it is handed a chameleonic molecule: a
property, or the average of two states it never occupies?

**Domain Expert B**: The BBBP benchmark is small, imbalanced, and reused by everyone here.
If the geometry-informed PEFT work and BBB-Nuke both report gains on the same distribution,
how much of the apparent progress is method and how much is a decade of collective
overfitting to one dataset — and would either result survive Di Lascio's local-versus-global
protocol applied across chemical series?

**Domain Expert C**: XL01126 crossed the barrier and the organoid protocol shows the
barrier is a cellular system with transporters, not a lipid sheet. Yet no paper here pairs a
computed permeability score with a measured Kp,uu for a degrader. If that pairing does not
exist in the literature, on what basis does anyone claim a model is useful for CNS degrader
triage?

**Convergence**

The corpus is unified by a single admission that no paper states outright: the standard
representation of a molecule is inadequate for the molecules the field now cares about.
Three papers say it in different vocabularies. Ermondi says it conformationally — PROTAC-1's
permeability is a property of an ensemble whose polar surface is buried or exposed depending
on environment. CycPeptMPDB-4D says it as infrastructure, paying the sampling cost across
solvents so that ensembles become an available input. BBB-Nuke says it mechanistically —
what is missing is not resolution but efflux transporter biology. Di Lascio supplies the
statistical form of the same claim: a global model's advantage evaporates when the target
chemistry diverges from its training chemistry.

Against this, the modelling papers proceed on an assumption that is genuinely incompatible
with the physical-chemistry papers. Yang's survey and the geometry-informed PEFT work both
frame the problem as data scarcity and class imbalance, to be solved by transfer from large
pre-trained representations. That framing presumes the label is well-defined and the
representation adequate — precisely what Ermondi's calibration denies for bRo5 space. Expert
A's question exposes the consequence: fine-tuning more efficiently on 2D-derived features
optimises a proxy that may not exist for a chameleonic degrader. Two research programmes
here look complementary and are in fact competing for the same diagnosis.

The emergent insight is about what the field has stopped measuring. XL01126 established
feasibility in 2022; BBB-Nuke and the organoid protocol establish, from opposite ends, that
the barrier is an active biological system; and yet the entire corpus contains no paired
dataset of computed permeability and measured brain exposure for degraders. The field
possesses an existence proof, a measurement protocol, several candidate representations, and
a well-developed statistical method for detecting applicability-domain failure — and has
never assembled them into the one experiment that would reveal whether current screening
tools work. That absence is not a gap in knowledge so much as a gap in accounting: nobody
has been made responsible for checking the ruler.

Expert B's suspicion is the sharpest open question. Because every model here is evaluated on
the same BBBP-derived benchmarks, reported improvements are not independent evidence of
generalisation; they are evidence of fit to a shared, Ro5-dominated distribution. Applying
Di Lascio's local-versus-global protocol with chemical-series splits — rather than random or
scaffold splits — would separate the two, and no paper in this set has done it.

Three things remain unaddressed. First, none of these papers reports an applicability-domain
flag, so a user receives a confident score for a molecule 400 Da outside the training range
with no warning; the uncertainty-quantification machinery to fix this exists but is not
deployed. Second, the correspondence between Bergmann's organoid permeability and in vivo
Kp,uu is unquantified, which caps how strong any external validation's conclusion can be.
Third — and this is a prerequisite for the generative work the field is already pursuing —
no one has established that a computed permeability score ranks degraders correctly at all.
Building a generative model that optimises such a score, before that ranking is verified,
repeats the error the structural-prediction community made in treating an interface score as
a reward.

---

## Quick Scan

| # | Title | Track | Verdict | Link |
|---|-------|-------|---------|------|
| 1 | Conformational Sampling Deciphers the Chameleonic Prope… | Computational Methods | Establishes that degrader permeability is an ensemble property 2D descripto… | [↗](https://doi.org/10.3390/pharmaceutics15010272) |
| 2 | Geometry-Informed Parameter-Efficient Fine-Tuning of Pr… | Computational Methods | The most efficient current route to adapting pre-trained GNNs to small BBBP… | — |
| 3 | Multi-solvent conformational ensembles for predicting c… | Computational Methods | Supplies the missing input representation for bRo5 permeability modelling a… | [↗](https://doi.org/10.1038/s41597-026-07984-9) |
| 4 | Blood-brain-barrier organoids for investigating the per… | Experimental Validation | Defines the ground truth any computational BBB validation inherits, and the… | [↗](https://doi.org/10.1038/s41596-018-0066-x) |
| 5 | Discovery of XL01126: A Potent, Fast, Cooperative, Sele… | TPD | The reference point for every CNS degrader claim, and the clearest demonstr… | [↗](https://doi.org/10.1021/jacs.2c05499) |
| 6 | Deep Learning for Blood-Brain Barrier Permeability Pred… | ML-Clinical | The best available map of which BBBP models exist and how they are evaluate… | — |
| 7 | Systematic Evaluation of Local and Global Machine Learn… | ML-Clinical | The strongest prior that a globally trained BBBP model will lose ranking po… | [↗](https://doi.org/10.1021/acs.molpharmaceut.2c00962) |
| 8 | BBB-Nuke: Transport-Aware Prediction of Blood-Brain Bar… | ML-Clinical | Names the right missing mechanism and builds the modular architecture to in… | [↗](https://doi.org/10.64898/2026.07.13.738280) |