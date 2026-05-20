---
title: "Issue 2: May 2026 — Targeted Protein Degradation: Glue Discovery Goes Generative"
date: 2026-05-19
period: "2026-05-05 to 2026-05-19"
issue_number: 2
topic: "protac"
tags: [protein-degradation, tpd, computational, ml-clinical]
---

## Field Intelligence Synthesis

**The Moment**
PROTAC computation is in its evaluation-and-curation phase. This week's corpus contains two benchmarks (Jin et al., Ribes et al.), one geometric-deep-learning model (Kothakapu et al.), and one all-AlphaFold/MD exploration of a novel E3 ligase (Kumari & Sobhia) — but zero new wet-lab degradation data. The field is auditing itself before scaling further.

**Convergences**
Three of the four PROTAC-relevant papers acknowledge the same upstream problem: PROTAC datasets are small, noisy, and contaminated by selection bias toward CRBN and VHL. TACK formalises this with a statistical evaluation framework over a new knowledge dataset; the Jin et al. benchmark quantifies how poorly deep generative models perform when held to honest metrics; SE(3)-PROTACs and the Parkin paper both lean on SE(3)-equivariant or AlphaFold-derived 3D representations because the binary "active/inactive" framing has run out of headroom.

**Tensions**
The two benchmark papers (Jin et al. and TACK) implicitly disagree on what "good" means: Jin et al. judge generative models on PROTAC-likeness and novelty; TACK judges them on predicted degradation activity. A model can win on one and lose on the other. Meanwhile Kumari & Sobhia proposes a Parkin-driven workflow with not a single ubiquitination assay — a structural plausibility argument standing in for biology. SE(3)-PROTACs trains on degradation labels that, per TACK, are exactly the labels we shouldn't yet trust.

**Signal for Practitioners**
Before training another PROTAC degradation predictor, audit the label set against the TACK framework — many literature "active" labels are concentration-dependent or cell-line-specific and shouldn't be treated as ground truth. If you're proposing a non-CRBN/VHL E3 ligase, budget for at least one in-cell ubiquitination experiment before publication; AlphaFold-plus-MD is no longer a sufficient bar.

---

## Annotated Bibliography

### Computational Methods & ML

#### Comprehensive Assessment and Benchmark of Deep Generative Models for Proteolysis TArgeting Chimera (PROTAC) Design
**Source**: Journal of Chemical Information and Modeling | **Date**: 2026-05-11 | **DOI**: [10.1021/acs.jcim.5c03212](https://doi.org/10.1021/acs.jcim.5c03212)
**Authors**: Jin J, Hou T, Liu H et al.
**Track**: Computational Methods

The authors construct a head-to-head benchmark of deep generative models for de novo PROTAC design — the first study to apply a uniform evaluation protocol to a class of models that have so far been published with non-comparable in-house metrics. The benchmark spans linker-design models, full-PROTAC graph generators, and 3D-conditioned approaches, scored on validity, uniqueness, novelty, drug-likeness, and PROTAC-specific structural features (warhead-linker-ligand topology, linker length, ring count). What the data supports: a quantitative leaderboard that finally lets readers compare models that previously each reported their own favourable metric. The paper makes clear that high validity rates in prior reports often coexisted with poor structural fidelity to known degraders, and that 3D-aware models do not yet dominate 2D graph generators on the metrics that matter for PROTACs specifically. What the data does NOT support: any claim about which generated molecules would actually degrade their target. The benchmark evaluates molecular properties, not in-cell DC50 or Dmax. No wet-lab synthesis or assay is reported. Models tuned on this benchmark may overfit to chemoinformatic surrogates of PROTAC-likeness that are themselves imperfect proxies for degradation activity.

**Verdict**: A useful and overdue audit of the PROTAC generative model literature, but the benchmark scores molecular structures, not degraders — pairing it with TACK-style activity evaluation is the natural next step before any of these models can be trusted to nominate experimental candidates.

---

#### SE(3)-PROTACs: Geometric deep learning for PROTAC degradation prediction
**Source**: Briefings in Bioinformatics | **Date**: 2026-05-04 | **DOI**: [10.1093/bib/bbag228](https://doi.org/10.1093/bib/bbag228)
**Authors**: Kothakapu AR, Madugula S, Gandeed SBS et al.
**Track**: Computational Methods

The authors propose SE(3)-PROTACs, an SE(3)-equivariant geometric deep learning model that predicts PROTAC-induced degradation by jointly modelling the 3D arrangement of the POI, the E3 ligase, and the PROTAC, alongside sequence-level features for the two proteins. The motivating claim is that prior degradation predictors (DeepPROTACs, ET-PROTACs and similar) either flatten the ternary complex to 2D representations or treat the three components in independent encoders before late fusion, missing the geometry that determines whether a target lysine reaches the E2 enzyme productively. What the data supports: the paper reports improvements over published baselines on standard PROTAC degradation classification metrics (accuracy, AUC, AUPR), with the geometric architecture credited for handling rotation-invariant 3D arrangements of the ternary complex. Sequence features for POI and E3 are framed as complementary background information that pure-structure models discard. What the data does NOT support: external validation. The benchmark set is the same labelled PROTAC corpus the field has been recycling, which TACK (this same issue) shows is statistically problematic. There is no prospective experiment, no degradation assay on a novel PROTAC predicted by the model, and no analysis of how performance degrades on E3 ligases beyond CRBN/VHL — which is precisely the regime where new methods would matter.

**Verdict**: SE(3)-PROTACs is a sensible architectural step toward geometry-aware degradation prediction, but its evaluation rests on the same noisy labels TACK is now challenging — a prospective wet-lab test on a single non-CRBN target would change the conversation more than another AUC point.

---

### TPD: PROTACs, Molecular Glues, Degraders

#### Beyond CRBN and VHL: Parkin driven lysine-based ternary complexes for selective β3-Tubulin degradation
**Source**: Computers in Biology and Medicine | **Date**: 2026-05-15 | **DOI**: [10.1016/j.compbiomed.2026.111660](https://doi.org/10.1016/j.compbiomed.2026.111660)
**Authors**: Kumari S, Sobhia ME
**Track**: TPD

The authors propose Parkin — an RBR-type E3 ligase implicated in mitophagy and cytoskeletal regulation — as a recruiter for PROTAC-mediated degradation of β3-tubulin. The pipeline is entirely computational: an active, ubiquitin-charged conformation of Parkin is reconstructed with AlphaFold, ternary complexes with β3-tubulin are built using PROTACs of varying linker length and chemistry, and stability is assessed via molecular dynamics, with the catalytic Lys389–Cys431 distance used as a proxy for ubiquitin-transfer competence. What the data supports: a structurally coherent argument that Parkin can in principle hold β3-tubulin's lysine residues in a geometry compatible with ubiquitin transfer, and that linker length and composition modulate ternary stability and cooperativity in the MD trajectories. The authors release distance-calculation scripts on GitHub. What the data does NOT support: any biology. There is no in vitro ubiquitination assay, no cell-based degradation experiment, no SPR or ITC binding measurement, and no synthesised PROTAC. The AlphaFold-modelled active Parkin conformation is itself an inference, since Parkin's RBR mechanism involves dynamic auto-inhibition that is poorly captured by single-state structure prediction. The 'selectivity' claim for β3-tubulin over other tubulin isoforms is structural, not experimental.

**Verdict**: A computational hypothesis paper that motivates Parkin as a candidate E3 ligase worth pursuing, but the gap between AlphaFold + MD geometry and an actual degrading PROTAC is the entire interesting problem — the paper opens the door without walking through it.

---

### ML for Clinical Translation & Datasets

#### Astragalin alleviates ulcerative colitis via FPR1 inhibition and restores Microbiota-Metabolite Homeostasis: A mechanism revealed by deep learning
**Source**: Biochemical Pharmacology | **Date**: 2026-04-30 | **DOI**: [10.1016/j.bcp.2026.118022](https://doi.org/10.1016/j.bcp.2026.118022)
**Authors**: Zhang F, Wang C, Zuo Y et al.
**Track**: ML-Clinical

The authors study Astragalin, a natural flavonoid, in DSS-induced ulcerative colitis and use an integrated deep-learning pipeline to nominate Formyl Peptide Receptor 1 (FPR1) as the molecular target, with downstream effects on gut microbiota composition and metabolite homeostasis. The 'deep learning' component is in service of mechanism-of-action inference for a natural product, not for protein degradation or PROTAC design. What the data supports: the integrated pipeline — likely combining network pharmacology, target prediction, and pathway enrichment — produces a coherent FPR1-centred narrative consistent with the broader literature on flavonoids and IBD, and the abstract indicates microbiota and metabolite readouts complement the in silico predictions. What the data does NOT support: this paper does not belong in a PROTAC issue. It surfaced in the candidates list because the keyword filter matched 'deep learning' plus a topic tag of 'tpd' that is almost certainly a gating error — there is no degrader, no E3 ligase, no ternary complex. Its actual contribution is a deep-learning-aided mechanism-of-action study for a Chinese-medicine-derived flavonoid in colitis, which is a legitimate but unrelated genre.

**Verdict**: Off-topic for this issue's TPD focus — included transparently as an example of how keyword-based candidate gating fails on mixed-method papers; the underlying study itself is a network-pharmacology natural-product mechanism paper, not a PROTAC contribution.

---

#### TACK: A statistical evaluation of degradation activity on a novel TArgeting Chimeras Knowledge dataset
**Source**: arXiv (preprint, not yet peer-reviewed) | **Date**: 2026-05-19 | **DOI**: —
**Authors**: Ribes S, Dunlop N, Mercado R
**Track**: ML-Clinical

The authors introduce TACK, a curated TArgeting Chimeras Knowledge dataset paired with a statistical evaluation framework for PROTAC degradation activity prediction. The motivation, from a group that previously published graph-based generative models for PROTACs, is that the field's current degradation prediction benchmarks fold heterogeneous experimental conditions — cell line, concentration, time point, assay format — into a single binary 'active/inactive' label that is not statistically meaningful. What the data supports: a dataset that disaggregates degradation outcomes by experimental context, with a statistical evaluation protocol that takes uncertainty in the labels seriously and reports calibration as well as point performance. The framework is built around graph neural network and machine learning baselines, giving readers a like-for-like comparison harness. What the data does NOT support: this is a preprint, not yet peer-reviewed; the dataset's coverage skew toward CRBN/VHL — which the authors themselves are best placed to acknowledge — means the framework's most interesting claims (about generalisation across E3 ligases) need testing on out-of-distribution targets. No new generative model is proposed; TACK is an evaluation paper, and its impact depends on the field adopting it as the standard rather than continuing to publish leaderboards on legacy splits.

**Verdict**: TACK is the right-shaped intervention at the right time — a community evaluation standard, not a new model — and if it lands, it will retroactively recalibrate how this issue's other ML papers should be read; the open question is whether SE(3)-PROTACs and similar predictors hold up when re-scored under TACK.

---

## Panel Discussion

**Panel**: Domain Expert A (Medicinal Chemistry / TPD) · Domain Expert B (ML / Protein Design) · Domain Expert C (Structural Biology & Bioinformatics)

**Domain Expert A**: We have four PROTAC-relevant papers in front of us and not a single new degradation data point — every "advance" here is in how we score, model, or hypothesise about a fixed set of legacy experiments. At what point does the field admit that the bottleneck is data generation, not model architecture, and what would a community-scale wet-lab campaign actually look like if it were designed to feed these benchmarks rather than the other way round?

**Domain Expert B**: Jin et al. and Ribes et al. both call themselves "benchmarks" but measure orthogonal things — molecular-property fidelity versus statistical reliability of activity labels. Should we be reading them as competing standards or as upstream and downstream stages of the same evaluation pipeline, and if it's the latter, why is no one yet running SE(3)-PROTACs end-to-end through both?

**Domain Expert C**: The Kumari & Sobhia Parkin paper uses AlphaFold to construct an "active, ubiquitin-charged" Parkin state — but Parkin's whole pharmacological interest is that its activation is a dynamic, phospho-regulated process that single-state structure prediction is fundamentally not built to capture. How much of this paper's structural argument survives if we replace the AlphaFold model with an ensemble representation of the conformational landscape?

**Convergence**

Reading the four PROTAC papers together, a striking pattern emerges: the field is performing housekeeping on a corpus of experiments that nobody is currently expanding. Expert A's framing is the most uncomfortable but the most accurate — Jin et al.'s benchmark, Ribes et al.'s TACK dataset, and Kothakapu et al.'s SE(3)-PROTACs model all draw from substantially the same underlying pool of literature-reported PROTAC degradation outcomes, and that pool has been the same pool for several years. The methodological sophistication is genuinely advancing; the empirical substrate is static. This is what a field looks like when it has outrun its training data.

Expert B's question exposes a real disagreement that the papers themselves do not acknowledge. Jin et al. score generative models on whether their outputs *look like* PROTACs; TACK proposes scoring on whether predicted activity values *match noisy experimental labels under proper statistical treatment*; SE(3)-PROTACs reports degradation classification accuracy on the legacy benchmark TACK is implicitly challenging. These are not three views of the same problem — they are three different problems wearing the word "benchmark." The healthier framing would be a pipeline: Jin et al. as a generative-output sanity check, TACK as the label-reliability layer, and a model like SE(3)-PROTACs as the predictor that has to clear both bars. Nobody is publishing this pipeline yet.

Expert C's challenge to the Parkin paper generalises beyond Parkin. The community has slid into treating AlphaFold-derived structures as load-bearing for mechanistic claims about proteins whose function depends on conformational dynamics — RBR ligases, intrinsically disordered E3 substrates, and the entire class of "induced proximity" interfaces are all in this category. The Parkin paper does state that linker length and chemistry modulate ternary stability in MD, which is the right kind of follow-up — but the founding AlphaFold structure is treated as ground truth rather than as one sample from an ensemble, and that's a methodological move worth flagging across the broader TPD modelling literature.

The genuine emergent insight from reading these papers together is this: **the PROTAC ML field has reached the point where evaluation standards are now more valuable than new models**. SE(3)-PROTACs is a competent geometric-deep-learning contribution, but its marginal value is bounded above by the reliability of the labels it's scored against — and if TACK is right, that ceiling is lower than the field has been pretending. Conversely, the Parkin paper illustrates the opposite failure mode: a domain (new E3 ligase nomination) where computational hypotheses are cheap and the rate-limiting step has flipped to wet-lab follow-through. Both failure modes point to the same diagnosis — the community's modelling capacity has decoupled from its empirical capacity.

The open questions this corpus exposes are specific. None of these papers asks how a degradation predictor trained on CRBN/VHL data should be expected to behave on a novel Parkin-recruiting PROTAC of the kind Kumari & Sobhia hypothesises; the obvious experiment of cross-evaluation is absent. None benchmarks against an honest negative control — a PROTAC chemically valid by Jin et al.'s criteria, predicted active by SE(3)-PROTACs, but experimentally inert. And none address whether the buried-surface-area-correlates-with-potency finding from prior ternary-complex predictors holds up under TACK's stricter label treatment. These are not "more research is needed" gaps — they are specific experiments that any one of these groups could run next quarter and that would meaningfully discriminate between the methodological positions on the table.

---

## Quick Scan

| # | Title | Track | Verdict | Link |
|---|-------|-------|---------|------|
| 1 | Comprehensive Assessment and Benchmark of Deep Generati… | Computational Methods | A useful and overdue audit of the PROTAC generative model literature, but t… | [↗](https://doi.org/10.1021/acs.jcim.5c03212) |
| 2 | SE(3)-PROTACs: Geometric deep learning for PROTAC degra… | Computational Methods | SE(3)-PROTACs is a sensible architectural step toward geometry-aware degrad… | [↗](https://doi.org/10.1093/bib/bbag228) |
| 3 | Beyond CRBN and VHL: Parkin driven lysine-based ternary… | TPD | A computational hypothesis paper that motivates Parkin as a candidate E3 li… | [↗](https://doi.org/10.1016/j.compbiomed.2026.111660) |
| 4 | Astragalin alleviates ulcerative colitis via FPR1 inhib… | ML-Clinical | Off-topic for this issue's TPD focus — included transparently as an example… | [↗](https://doi.org/10.1016/j.bcp.2026.118022) |
| 5 | TACK: A statistical evaluation of degradation activity … | ML-Clinical | TACK is the right-shaped intervention at the right time — a community evalu… | — |