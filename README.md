# bothros-research

What the [BOTHROS](https://github.com/jmacdonald263/bothros) sign classifier recovers from
Linear A and Linear B sign *shapes* — and a set of supporting corpus analyses. Companion to
the tool repo; weights on [Hugging Face](https://huggingface.co/JMacD263/linear-a-linear-b-bothros),
live [demo](https://huggingface.co/spaces/JMacD263/bothros-demo), archived on
[Zenodo](https://doi.org/10.5281/zenodo.20746759).

These are experiments, not decipherment. Each finding states its ground truth, baseline, and
significance test, and reports results that weaken or fail as readily as those that hold.

---

## Primary findings — what the classifier embeddings reveal

These require the BOTHROS classifier: the results come from its sign-embedding space and do
not reproduce with a generic vision model.

### 1. Cross-script LA↔LB correspondences from sign shape — [`findings/cross_script_cognates.md`](findings/cross_script_cognates.md)

Given only sign shapes, the classifier embedding recovers the Linear A ↔ Linear B sign
correspondences established from decipherment: **secure-tier precision@5 = 100%** (P@1 90.9%),
ranked in the published confidence order. The signal is specific to the Aegean-fine-tuned
classifier — an ImageNet backbone scores 18.5% P@5 and **DINOv3 (a strong self-supervised
ViT) scores 0/58**. This is the result that most clearly shows the model has learned
something real about the scripts, and it is the natural basis for outreach.

### 2. Scribal hands — [`findings/scribal_hands.md`](findings/scribal_hands.md)

The embedding groups tablets by who wrote them, agreeing with catalogued hand attributions:
a Pylos Mantel test against Skelton's palaeography (r ≈ 0.68, p < 0.05), and Linear A
hand identification at Knossos (top-1 44.7%, ~2.5× the majority-hand baseline).

### 3. Classifier gold-crop accuracy — [`findings/classifier_oracle.md`](findings/classifier_oracle.md)

First published gold-crop sign-classification accuracy for LA and LB (LA 79.3%, LB 64.5%
held-out; 83.8% on well-sampled signs, vs an Old-Babylonian ResNet-50's 87.1%). Includes a
frequency stratification (the `<5`-instance long-tail floor) and a training-free
nearest-centroid head.

---

## Supporting analyses — corpus statistics

Secondary. These read only the **transcribed sign sequences** (DĀMOS / SigLA / lineara.xyz),
not the classifier or detector — anyone with the transcriptions could compute them. Included
for context and because a few are firsts for these scripts.

- **Sign function** (logogram vs. syllabogram from distribution, 88% LOO) — [`findings/sign_function.md`](findings/sign_function.md)
- **Sign structure** (fraction syntax, attested-word bigrams, allograph dead-end) — [`findings/sign_structure.md`](findings/sign_structure.md)
- **Corpus structure** (entropy, Heaps, networks, matched-size control) — [`findings/distributional_structure.md`](findings/distributional_structure.md)
- **Restoration** (n-gram / Transformer masked-sign prediction; LM rescoring near-null) — [`findings/restoration.md`](findings/restoration.md)
- **LA corpus structure** (topics, site variation, word segmentation) — [`findings/la_corpus_structure.md`](findings/la_corpus_structure.md)

Index with all numbers: [`findings/_INDEX.md`](findings/_INDEX.md).

---

## Method and weights

Model-dependent findings are run on both published weight sets and report the difference:
the full-data **release** classifiers (`*_release.pth`) and the held-out-safe **benchmark**
classifiers (`*_classifier.pth`). The release figure is the deployed model's accuracy (an
in-domain ceiling on the gold-crop oracle, whose test crops are in its training set); the
benchmark figure is the held-out estimate. More training data helps some tasks and not
others; results are reported as measured.

Cross-script claims are scored against published correspondence pairs (precision@K), not raw
embedding proximity — nearest-neighbour distance across scripts is confounded by per-script
drawing style. See [`METHODOLOGY.md`](METHODOLOGY.md). A larger planned piece, a
character-based sign phylogeny, is scoped in [`PHYLOGENY_PLAN.md`](PHYLOGENY_PLAN.md) (design
only, not yet run).

## Reproducing

Each finding page names the script and inputs that produce its numbers. The analysis
scripts (`src/*.py`) live in the BOTHROS development repository and are available on
request — they are not yet packaged here, so treat each page's reproduction command as a
precise specification of what was run rather than a turnkey script in this repo. They run
against three obtainable inputs:

- **Weights** — the published classifiers + detector on
  [Hugging Face](https://huggingface.co/JMacD263/linear-a-linear-b-bothros)
  (`download_weights.py` in the [tool repo](https://github.com/jmacdonald263/bothros)).
- **`corpus.db`** — a local SQLite corpus built from the public DĀMOS, SigLA and
  lineara.xyz / LinearBExplorer sources via the tool repo's ingestion scripts; no
  copyrighted images are redistributed.
- **Ground-truth tables** — the published LA↔LB correspondence pairs and the catalogued
  scribal-hand attributions the anchored metrics score against.

`requirements.txt` lists the Python dependencies.

## Scope

Linear A and Linear B. Cretan Hieroglyphic appears only as a corpus-statistics control
(it is not a released script — its pipeline evaluation is held back by train/test data
leakage and too little available data to resolve it). Cypro-Minoan, and the planned
Corazza 2022 replication, is parked — data-blocked: the comparable corpus (Corazza's
Sign2Vec_d) is copyright-restricted and non-redistributable.

## Acknowledgements

- **Linear A** — sign boxes and the AB-code catalogue from
  [SigLA](https://sigla.phis.me) (**Ester Salgarella & Simon Castellan**); inscription
  images from [lineara.xyz](https://lineara.xyz) (mwenge; ultimately GORILA, Godart &
  Olivier).
- **Linear B** — transcriptions from [DĀMOS](https://damos.hf.uio.no) (Federico Aurora)
  and the sign imagemap from [LinearBExplorer](https://github.com/mwenge/LinearBExplorer)
  (mwenge).

## Licence

- **Code:** MIT (see [`LICENSE`](LICENSE)).
- **Derived data** (correspondence tables, hand labels, derived statistics): CC BY-NC-SA 4.0
  (see [`DATA_LICENSE`](DATA_LICENSE)), derived from DĀMOS, SigLA, lineara.xyz,
  LinearBExplorer, GORILA (research use). No copyrighted corpus images are included;
  reproductions fetch images from those sources.
