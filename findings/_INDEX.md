# Findings index

Each page names the script and inputs that produce its numbers (the scripts live in the
development repo — see the [README](../README.md#reproducing)) and states its null /
significance test.

## Primary — require the BOTHROS classifier

Results from the classifier's sign-embedding space; they do not reproduce with a generic
vision model. Run on both published weight sets (`*_release.pth` / `*_classifier.pth`) with
the difference reported.

| Theme | Result | Page |
|---|---|---|
| **Cross-script LA↔LB correspondences** | secure P@5 = 100% on both weight sets; secure P@1 90.9% (benchmark) / 81.8% (release); ImageNet baseline 18.5%, **DINOv3 0/58** | [cross_script_cognates.md](cross_script_cognates.md) |
| **Scribal hands** | Pylos Mantel r = 0.684 / 0.649 (p < 0.05); LA Knossos hand-ID top-1 44.7% / 42.6% vs 17.7% baseline; LB Knossos 22.4% (weak) | [scribal_hands.md](scribal_hands.md) |
| **Classifier gold-crop accuracy** | LA 89.7% / 79.3%, LB 86.7% / 64.5% (release in-domain / benchmark held-out); ≥20-instance 83.8%, `<5` = 0%; training-free prototype head 71–86% | [classifier_oracle.md](classifier_oracle.md) |

## Supporting — corpus statistics (transcription-only)

Read only the transcribed sign sequences (DĀMOS / SigLA / lineara.xyz), not the classifier
or detector. Included for context; a few are firsts for these scripts.

| Theme | Result | Page |
|---|---|---|
| Sign function (logogram vs. syllabogram) | distributional-feature classifier 88.2% LOO (ROC-AUC 0.95) | [sign_function.md](sign_function.md) |
| Sign structure | fractions attach to commodity logograms (154 vs 76); bigrams recover *ku-ro* "total"; allograph detection frequency-confounded → inconclusive | [sign_structure.md](sign_structure.md) |
| Corpus structure | LB > LA sequential structure, held at matched corpus size; entropy ranks formulaicity, not linguisticality | [distributional_structure.md](distributional_structure.md) |
| Restoration | LB masked-sign top-1 25.5% / top-5 45.2% (vs 0.08% chance); LA near-floor 4.4%; LM rescoring +0.3pp (near-null) | [restoration.md](restoration.md) |
| LA corpus structure | 3/8 LDA topics logogram-anchored; site JS 0.33–0.72 (confounded → descriptive); word-segmentation +4pp over chance | [la_corpus_structure.md](la_corpus_structure.md) |

## Notes

Negatives and limits are reported in place: the allograph dead-end, the LA restoration floor,
the LM-rescoring near-null, the in-domain/held-out oracle gap, the prototype head's tail
failure, and the site-variation confound. Cross-script proximity without correspondence
anchoring is excluded for the rendering-style confound (see [`../METHODOLOGY.md`](../METHODOLOGY.md)).
Findings focus on Linear A and Linear B; Cretan Hieroglyphic appears only as a control in the
corpus-structure comparison.
