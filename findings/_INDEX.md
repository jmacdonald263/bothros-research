# Findings index

Each page carries its reproduction command and a stated null / significance test.
Model-dependent findings are run on both published weight sets — `*_release.pth` (full-data)
and `*_classifier.pth` (held-out benchmark) — and report the difference. Model-free findings
read only sign sequences. More training data helps some tasks (scribal Pylos, in-domain
oracle) and not others (cross-script, Knossos hand-ID); each is reported as measured.

| # | Theme | Result | Page |
|---|---|---|---|
| 1 | Cross-script LA↔LB correspondences | `la_shared` secure P@5 = 100% on both weight sets; secure P@1 90.9% (benchmark) / 81.8% (release); ImageNet baseline 18.5% | [cross_script_cognates.md](cross_script_cognates.md) |
| 2 | Scribal hands (Pylos Mantel + Knossos hand-ID) | Pylos Mantel r = 0.684 / 0.649 (p < 0.05); LA Knossos hand-ID top-1 44.7% / 42.6% vs 17.7% baseline; LB Knossos 22.4% (weak) | [scribal_hands.md](scribal_hands.md) |
| 3 | Sign function — logogram vs. syllabogram | distributional-feature classifier 88.2% LOO (ROC-AUC 0.95), CI [82.4, 92.2] | [sign_function.md](sign_function.md) |
| 4 | Sign structure (fractions, bigrams, allographs) | fractions attach to commodity logograms (154 vs 76); bigrams recover *ku-ro* "total"; syllabary β = 0.236; allograph detection frequency-confounded → inconclusive | [sign_structure.md](sign_structure.md) |
| 5 | Corpus structure (entropy, Heaps, networks) | LB > LA sequential structure, held at matched corpus size; entropy ranks formulaicity, not linguisticality | [distributional_structure.md](distributional_structure.md) |
| 6 | Restoration (n-gram / Transformer) | LB masked-sign top-1 25.5% / top-5 45.2% (vs 0.08% chance); LA near-floor 4.4% | [restoration.md](restoration.md) |
| 7 | Classifier gold-crop accuracy | LA 89.7% / 79.3%, LB 86.7% / 64.5% (release in-domain / benchmark held-out) | [classifier_oracle.md](classifier_oracle.md) |
| 8 | LA corpus structure (topics, site variation, segmentation) | 3/8 LDA topics logogram-anchored; site JS 0.33–0.72 (confounded → descriptive); word-segmentation +4pp over chance | [la_corpus_structure.md](la_corpus_structure.md) |

Negatives and limits are reported in place: the allograph dead-end (#4), the LA restoration
floor (#6), the in-domain/held-out oracle gap (#7), and the site-variation confound (#8).
Cross-script proximity without correspondence anchoring is excluded for the rendering-style
confound described in [`../METHODOLOGY.md`](../METHODOLOGY.md).

## Scope

Findings focus on Linear A and Linear B. Cretan Hieroglyphic appears only as a control where
a three-script comparison is informative (#5). Model-dependent results use the published
release and benchmark weights; the corpus-only analyses are independent of the weights.
