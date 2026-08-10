---
title: "Issue 5: August 2026 — Targeted Protein Degradation: ML-Clinical"
date: 2026-08-10
period: "2026-07-27 to 2026-08-10"
issue_number: 5
topic: "protac"
tags: [protein-degradation, tpd, computational, ml-clinical, drug-likeness]
---

## Field Intelligence Synthesis

**The Moment**
The CNS degrader field is arguing about brain penetration using an endpoint that its own pharmacokinetics literature retired eighteen years ago. Read together, these eight papers show a machine-learning community optimising binary blood-brain-barrier classifiers while the clinical pharmacology community has long since moved to Kp,uu — and neither has yet turned to look at degraders.

**Convergences**
Three independent lines are converging on the same architecture: let a learned model predict one physically meaningful term, then let a mechanistic model assemble the rest. Gülave et al. plug a random-forest Kp,uu predictor into LeiCNS-PK3.0; Liu and Kosugi scale animal ML models to human and benchmark them against mechanistic NeuroPK; Hsu et al. make the efflux term itself interpretable via integrated gradients over molecular graphs. The direction is decomposition, not bigger end-to-end models.

**Tensions**
The decomposition premise is under attack from inside the same community. Van Valkengoed, Rottschäfer and de Lange simulate the P-glycoprotein expression–activity relationship and find it neither linear nor drug-independent — which is precisely the assumption a fixed-weight efflux term requires. Meanwhile Uchida et al. show the human barrier expresses more BCRP and *less* P-gp than mouse, so every rodent-trained model inherits a species-specific bias that no amount of held-out validation will reveal.

**Signal for Practitioners**
Stop reporting BBB+/− accuracy and start reporting Kp,uu rank correlation with a stated applicability domain — the dynamic ranges differ by an order of magnitude, and your classifier is being graded on the noisier axis. If you work on degraders, the paired data now exists on the efflux side (3,271 compounds in Brenner et al.) and nowhere on the brain side; that asymmetry, not model capacity, is the binding constraint.

---

## Annotated Bibliography

### Computational Methods & ML

#### A robust and interpretable graph neural network-based protocol for predicting P-glycoprotein substrates
**Source**: Briefings in Bioinformatics | **Date**: 2025-07-02 | **DOI**: [10.1093/bib/bbaf392](https://doi.org/10.1093/bib/bbaf392)
**Authors**: Hsu KC et al.
**Track**: Computational Methods

The authors build a graph neural network protocol for P-glycoprotein *substrate* classification — deliberately the harder and less-studied task than inhibitor prediction — comparing graph convolutional networks, AttentiveFP and an ensemble on 1,995 drug molecules (1,202 substrates, 793 nonsubstrates). What the data supports: AttentiveFP reaches ROC-AUC 0.848 and accuracy 0.815, beating traditional descriptor methods. Integrated-gradient attribution recovers 20 substructures associated with substrate behaviour, of which the top four confer greater than 70% substrate probability on their own — turning the model from a score into a chemical hypothesis a medicinal chemist can act on. What the data does not support: the training set is small and near-balanced by construction (60/40), which does not reflect the real prior over chemical space; nothing here extends past rule-of-five molecules, so the applicability domain almost certainly excludes degraders; and substrate classification is binary, not a quantitative efflux ratio, so it cannot enter a Kp,uu decomposition as a magnitude.

**Verdict**: Atom-level attribution finally makes efflux liability a localisable molecular feature rather than an opaque score, but on a 1,995-molecule Ro5 dataset it cannot yet tell a degrader chemist whether the linker is the problem.

---

#### Simulation-based assessment of the P-glycoprotein expression-activity relationship shows a drug and system dependency
**Source**: Journal of Pharmacokinetics and Pharmacodynamics | **Date**: 2026-02-02 | **DOI**: [10.1007/s10928-025-10015-6](https://doi.org/10.1007/s10928-025-10015-6)
**Authors**: van Valkengoed DW, Rottschäfer V, de Lange ECM
**Track**: Computational Methods

The authors interrogate the two assumptions underpinning in vitro–in vivo extrapolation of P-glycoprotein activity — that expression scales linearly with activity, and that the relationship is drug-independent — using a membrane kinetic binding model parameterised by association, dissociation and efflux rate constants alongside passive permeability. What the data supports: simulations across seven P-gp substrates show the expression–activity relationship is both drug-dependent and system-dependent, varying with dose and with baseline transporter expression. The conflicting experimental results in the literature are thus reconciled as a predictable consequence of kinetics rather than as measurement error. What the data does not support: this is a theoretical exploration, not an experimental test — no new in vivo data are generated, and the conclusion depends on the chosen binding-model structure and on rate constants that are themselves poorly determined for most compounds. Nothing addresses beyond-rule-of-five chemistry, where the passive permeability term is exactly where the uncertainty lives.

**Verdict**: If expression does not scale linearly to activity and the relationship is drug-specific, then any Kp,uu model carrying a fixed-weight efflux term is mis-specified in principle — a result that constrains every hybrid architecture in this issue.

---

### Experimental Validation & Assays

#### Quantitative targeted absolute proteomics of human blood-brain barrier transporters and receptors
**Source**: Journal of Neurochemistry | **Date**: 2011-04 | **DOI**: [10.1111/j.1471-4159.2011.07208.x](https://doi.org/10.1111/j.1471-4159.2011.07208.x)
**Authors**: Uchida Y et al.
**Track**: Experimental Validation

The authors deliver the first absolute quantification of the human blood-brain barrier transporter complement, isolating brain microvessels from the cortices of seven male donors and measuring 114 membrane proteins by LC-MS/MS with in-silico-selected surrogate peptides. What the data supports: BCRP is the most abundant drug transporter at 8.14 fmol/µg protein and is 1.85-fold higher in human than mouse, while P-glycoprotein at 6.06 fmol/µg is 2.33-fold *lower* in human than mouse mdr1a. Several rodent organic anion transporters fall below the limit of quantification in human. GLUT1 is comparable across species; LAT1 is fivefold lower in human. What the data does not support: seven male donors give no sex, age or disease-state resolution, and abundance is not activity — the paper quantifies how much transporter is present, not how much flux it produces. Regional and interindividual variability are outside its scope.

**Verdict**: This is the absolute scale against which every rodent-to-human brain-exposure extrapolation should be corrected, and it shows the correction is not a constant — P-gp and BCRP move in opposite directions between species.

---

#### Estimating Efflux Transporter-Mediated Disposition of Molecules beyond the Rule of Five (bRo5) Using Transporter Gene Knockout Rats
**Source**: Biological & Pharmaceutical Bulletin | **Date**: 2020-03-01 | **DOI**: [10.1248/bpb.b19-00641](https://doi.org/10.1248/bpb.b19-00641)
**Authors**: Miyake T
**Track**: Experimental Validation

The author uses transporter knockout rats — Mdr1a, Bcrp and the double knockout — as a causal control to isolate the contribution of efflux to the disposition of five marketed beyond-rule-of-five drugs: asunaprevir, cyclosporine, danoprevir, ledipasvir and simeprevir, dosed both intravenously and orally. What the data supports: the study starts from the observation that marketed bRo5 molecules tend to be efflux substrates, and quantifies clearance, oral bioavailability and absorption with the transporters genetically removed rather than pharmacologically inhibited — the cleanest available design, free of the inhibitor-selectivity artefacts that confound MDCK and Caco-2 work. What the data does not support: this is oral absorption and systemic clearance, not brain exposure — no Kp,uu, no central compartment, no barrier. Five compounds is a case series, and all five are antivirals or an immunosuppressant, none of them bifunctional degraders.

**Verdict**: The strongest causal evidence that efflux dominates disposition once molecules leave rule-of-five space, established at the gut and liver — leaving the identical experiment at the blood-brain barrier unperformed.

---

### TPD: PROTACs, Molecular Glues, Degraders

#### Assays for Measuring the Cell Permeability of Proteolysis-Targeting Chimeras (PROTACs): Performance, Correlations, Applicability and Recommendations
**Source**: Molecular Pharmaceutics | **Date**: 2026-07-06 | **DOI**: [10.1021/acs.molpharmaceut.5c01908](https://doi.org/10.1021/acs.molpharmaceut.5c01908)
**Authors**: Brenner C et al.
**Track**: TPD

The authors assemble what is, by a wide margin, the largest public degrader permeability resource — 3,271 PROTACs across diverse chemical and target space — and cross-compare up to three assays per compound: NanoBRET E3 ligase target engagement, Caco-2 apparent permeability, and a P-glycoprotein efflux liability assay. What the data supports: NanoBRET availability shift correlates strongly with Caco-2 apparent permeability, giving the field an empirical basis for choosing between assays and, crucially, a stated chemical space in which each readout is reliable. Most degraders in the set are beyond-rule-of-five, so this is applicability-domain evidence generated on the actual chemistry rather than extrapolated to it. What the data does not support: every measurement is cellular. There is no in vivo brain exposure, no Kp,uu, and no CNS compartment anywhere in the dataset — the efflux axis is populated at n = 3,271 while the brain axis remains empty.

**Verdict**: The claim that degrader efflux data does not exist is now false; what does not exist is efflux paired with brain exposure, which narrows the open question from a data desert to a single missing join.

---

### ML for Clinical Translation & Datasets

#### On the rate and extent of drug delivery to the brain
**Source**: Pharmaceutical Research | **Date**: 2008-08 | **DOI**: [10.1007/s11095-007-9502-2](https://doi.org/10.1007/s11095-007-9502-2)
**Authors**: Hammarlund-Udenaes M et al.
**Track**: ML-Clinical

The authors define the parameter set that brain drug delivery should actually be described with — Kp,uu (unbound brain-to-blood concentration ratio), CLin (permeability clearance into brain) and Vu,brain (intra-brain distribution) — and argue that permeability, the quantity the computational field optimises, is the least relevant of the three for drugs given repeatedly. What the data supports: the decisive numbers are ranges. Across CNS-active drugs, Kp,uu spans roughly 150-fold, log BB spans up to 2,000-fold, and blood-brain-barrier permeability spans over 20,000-fold. The extent of delivery, not the rate, determines steady-state exposure under continuous dosing, and the three parameters are separately measurable early in discovery. What the data does not support: it is a framework paper, not a predictive method — it supplies no model, no dataset, and nothing specific to beyond-rule-of-five chemistry, which barely existed as a design category in 2008.

**Verdict**: The field's binary blood-brain-barrier label sits on the widest-ranging and noisiest of the three axes, which was quantified here in 2008 and has been largely ignored by the machine-learning literature since.

---

#### Human Brain Penetration Prediction Using Scaling Approach from Animal Machine Learning Models
**Source**: The AAPS Journal | **Date**: 2023-09-05 | **DOI**: [10.1208/s12248-023-00850-1](https://doi.org/10.1208/s12248-023-00850-1)
**Authors**: Liu S, Kosugi Y
**Track**: ML-Clinical

The authors attempt human Kp,uu,brain prediction indirectly: rebuild machine-learning models for rat Kp,uu, develop new models for monkey Kp,uu, then scale the animal predictions to human, using mechanistic NeuroPK models on the identical monkey and human datasets as the comparator. What the data supports: rat Kp,uu predictivity from earlier work is successfully reproduced with current open-source tooling, monkey models are established for the first time, and the animal-model-plus-scaling route is benchmarked head-to-head against a mechanistic alternative rather than against another QSAR. What the data does not support: the datasets are small enough to require leave-one-out cross-validation, which is a statement about data scarcity as much as about method choice. No applicability domain is reported for chemistry outside the training space, and species scaling is applied as an empirical factor rather than derived from transporter abundance — the very quantity Uchida et al. show differs directionally between species.

**Verdict**: The most honest existing answer to "can machine learning predict Kp,uu in humans" — yes, indirectly, on datasets so small that leave-one-out is mandatory and any beyond-rule-of-five extrapolation is unguarded.

---

#### Prediction of the Extent of Blood-Brain Barrier Transport Using Machine Learning and Integration into the LeiCNS-PK3.0 Model
**Source**: Pharmaceutical Research | **Date**: 2025-02 | **DOI**: [10.1007/s11095-025-03828-0](https://doi.org/10.1007/s11095-025-03828-0)
**Authors**: Gülave B et al.
**Track**: ML-Clinical

The authors build a QSPR model for rat Kp,uu from 98 compounds using 2D and 3D descriptors computed in MOE, compare random forest, support vector machines, K-nearest neighbours and sparse partial least squares, then feed the predictions into the LeiCNS-PK3.0 physiologically-based CNS pharmacokinetic model. What the data supports: random forest performs best, with R² of 0.61 on test data and 61% of predictions within twofold error, and the predicted Kp,uu values integrate successfully into the PBPK workflow — a working demonstration that a learned parameter can substitute for a measured one inside a mechanistic CNS model. What the data does not support: 98 rat compounds is a small training set for a 2D/3D descriptor space, twofold error is a wide tolerance for a parameter that spans 150-fold, and the single learned term is the aggregate transport extent — efflux is never separated from passive permeability, so the model cannot attribute a low prediction to a mechanism. No degrader or bRo5 evaluation is attempted.

**Verdict**: The clearest existing template for the hybrid architecture this issue circles — learn one term, let mechanism assemble the rest — but the term it learns is the composite, which is exactly the aggregation the field now needs to break open.

---

## Panel Discussion

**Panel**: Domain Expert A (Clinical Pharmacology / CNS PK) · Domain Expert B (ML & Cheminformatics) · Domain Expert C (Medicinal Chemistry, bRo5 & Degraders)

**Domain Expert A**: Every model in this set is trained on rodent Kp,uu and validated on rodent Kp,uu, yet the human barrier expresses less P-gp and more BCRP than the mouse. If the species correction is directionally opposite for the two dominant efflux transporters, what exactly is a held-out rat test set validating?

**Domain Expert B**: Three of these papers decompose brain exposure into a learned term plus a mechanistic scaffold, and one of them argues the decomposition's central coefficient is drug- and system-dependent. Are we building interpretable models, or are we building models whose interpretability is an artefact of a linearity assumption nobody has tested on the chemistry we care about?

**Domain Expert C**: Brenner has 3,271 degraders with paired Caco-2 and P-gp efflux measurements, and not one of them has a brain number attached. Is the field's bottleneck a modelling problem at all, or is it that nobody has run the in vivo study that would cost a fraction of the compute already spent on classifiers?

**Convergence**

The corpus is unified by a single problem stated in two incompatible vocabularies. The machine-learning papers — Hsu, and to a degree the QSPR half of Gülave — treat brain penetration as a property of the molecule, learnable from structure. The pharmacokinetics papers — Hammarlund-Udenaes, Uchida, van Valkengoed — treat it as a property of a system in which the molecule is only one input, alongside transporter abundance, expression-activity kinetics and species. Both communities are asking whether a compound reaches the brain; only one of them believes the answer is fully contained in the compound.

That split explains the endpoint dispute. Hammarlund-Udenaes quantified the stakes in 2008: Kp,uu varies ~150-fold across CNS drugs, log BB up to 2,000-fold, permeability over 20,000-fold. A binary classifier trained on the permeability-flavoured axis is being scored on the noisiest available signal, and its apparent accuracy is partly a measure of how coarse the labels are. The field has largely optimised the wrong target for a decade, and the paper saying so is neither new nor obscure.

The consensus response is decomposition, and here the corpus turns on itself. Gülave's architecture — a random forest supplying Kp,uu into LeiCNS-PK3.0, R² 0.61, 61% within twofold — is the template everyone is converging on, and Hsu supplies exactly the interpretability that would let a chemist act on the efflux term via integrated-gradient substructures. But van Valkengoed, Rottschäfer and de Lange demonstrate by simulation that the P-gp expression–activity relationship is neither linear nor drug-independent. A decomposition with a fixed-weight efflux term is therefore not merely imprecise; it is mis-specified, and its interpretability is the most dangerous thing about it, because an attribution map over a wrong functional form still looks actionable.

The genuinely emergent insight lives in the gap between Miyake and Brenner, and neither paper states it. Miyake shows, with knockout rats, that marketed bRo5 molecules are efflux substrates and that efflux governs their disposition — at the gut and the liver. Brenner shows that the degrader field has now measured P-gp efflux liability on 3,271 compounds — in cells. Put them side by side and the field's actual position becomes visible: the efflux hypothesis for degraders is causally supported in the wrong tissue and empirically populated in the wrong system. Nobody has run knockout-rat brain exposure on a degrader series, which is neither a modelling problem nor an expensive one. The missing artefact is a join between two datasets that both already exist.

What this corpus collectively exposes is a specific, bounded ignorance. None of these papers reports paired efflux and Kp,uu measurements for a single beyond-rule-of-five degrader — a prerequisite for testing whether the efflux term carries more weight in bRo5 space than in Ro5 space, which is the claim the entire "go toward active transport" position rests on. None establishes an applicability domain in molecular weight for the efflux predictors, so we cannot say whether Hsu's 1,995-molecule model has anything to say about a 900-dalton bifunctional. And none reconciles transporter abundance with transporter activity — meaning the ambition of deriving Kp,uu from a cell-scale simulation currently lacks its central equation, not just its data.

---

## Quick Scan

| # | Title | Track | Verdict | Link |
|---|-------|-------|---------|------|
| 1 | A robust and interpretable graph neural network-based p… | Computational Methods | Atom-level attribution finally makes efflux liability a localisable molecul… | [↗](https://doi.org/10.1093/bib/bbaf392) |
| 2 | Simulation-based assessment of the P-glycoprotein expre… | Computational Methods | If expression does not scale linearly to activity and the relationship is d… | [↗](https://doi.org/10.1007/s10928-025-10015-6) |
| 3 | Quantitative targeted absolute proteomics of human bloo… | Experimental Validation | This is the absolute scale against which every rodent-to-human brain-exposu… | [↗](https://doi.org/10.1111/j.1471-4159.2011.07208.x) |
| 4 | Estimating Efflux Transporter-Mediated Disposition of M… | Experimental Validation | The strongest causal evidence that efflux dominates disposition once molecu… | [↗](https://doi.org/10.1248/bpb.b19-00641) |
| 5 | Assays for Measuring the Cell Permeability of Proteolys… | TPD | The claim that degrader efflux data does not exist is now false; what does … | [↗](https://doi.org/10.1021/acs.molpharmaceut.5c01908) |
| 6 | On the rate and extent of drug delivery to the brain | ML-Clinical | The field's binary blood-brain-barrier label sits on the widest-ranging and… | [↗](https://doi.org/10.1007/s11095-007-9502-2) |
| 7 | Human Brain Penetration Prediction Using Scaling Approa… | ML-Clinical | The most honest existing answer to "can machine learning predict Kp,uu in h… | [↗](https://doi.org/10.1208/s12248-023-00850-1) |
| 8 | Prediction of the Extent of Blood-Brain Barrier Transpo… | ML-Clinical | The clearest existing template for the hybrid architecture this issue circl… | [↗](https://doi.org/10.1007/s11095-025-03828-0) |