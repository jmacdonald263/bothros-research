# Findings index & re-run tracker

Each theme is written up with its reproduction script + rigor-gate evidence (anchored
P@K / permutation / baseline-null). Model-dependent findings are run on **both published
Hugging Face weight sets** — `*_release.pth` (full-data, featured: the model a user
downloads) and `*_classifier.pth` (held-out-safe benchmark) — and we report the delta.
Model-free findings (corpus-only) carry no weights dependence.

"More data" is **not** a universal win: release beats benchmark on scribal hands (#2) and
inflates the oracle by memorisation (#6), but is *marginally worse* on cross-script (#1).
We report each as measured.

| # | Theme | Register | Result (release / benchmark where both apply) | Status |
|---|---|---|---|---|
| 1 | **Cross-script LA↔LB cognates** | matches + extends | `la_shared` **secure P@5 = 100%** on BOTH; P@1 secure 81.8% (rel) / 90.9% (bench); two nulls fail (ImageNet 18.5%, **DINOv3 0%**) | ✅ [written](cross_script_cognates.md) — both weights + DINOv3 null |
| 2 | **Scribal hands** (Pylos Mantel + Knossos hand-ID, LB+LA) | matches | Pylos Mantel **r=0.684/0.649**; **LA Knossos hand-ID top-1 44.7%/42.6%** (2.5× baseline, NMI ≈0.49); LB Knossos weak (22.4%) | ✅ [written](scribal_hands.md) — both weights, 2 sites, LA+LB |
| 3 | **Sign structure** (allograph / logogram-vs-syllabogram / numerals) | extends + matches + dead-end | fractions→commodity logograms (154 vs 76); bigrams recover *ku-ro* "total"; syllabary β=0.236; allograph **freq-confounded (−0.76) → inconclusive** | ✅ [written](sign_structure.md) — model-free |
| 4 | **Distributional / sequential structure** (entropy, Heaps, network) | extends + self-negative | CH>LB>LA on all 3 methods, but ranks **formulaicity not linguisticality** (LB control) | ✅ [written](distributional_structure.md) — model-free |
| 5 | **Restoration** (n-gram / transformer) | extends + LA negative | LB masked-sign **top-1 25.5% / top-5 45.2%** (vs 0.08% chance); **LA near-floor 4.4%** | ✅ [written](restoration.md) — model-free LM |
| 6 | **Classifier-as-oracle** (DeepScribe context) | matches | LA **89.7% / 79.3%**, LB **86.7% / 64.5%** (release in-domain / benchmark held-out); gap = memorisation, measured | ✅ [written](classifier_oracle.md) — both weights |
| 7 | **NEGATIVE: Phaistos placement** | honesty | retracted 2026-05-27 (rendering-style confound) | ✅ [written](NEGATIVE_phaistos.md) — the centrepiece |
| 8 | **LA corpus structure** (LDA topics / site variation / word-seg) | extends, descriptive | 3/8 LDA topics logogram-anchored; site JS 0.33–0.72 (confounded → descriptive); branching-entropy seg +4pp over chance (near-neg) | ✅ [written](la_corpus_structure.md) — model-free |

**8 findings written.** Registers: matches (#1/#2/#3/#6), extends (#1/#3/#4/#5/#8),
negatives shipped (#7 Phaistos + DINOv3 in #1 + self-negatives in #3/#4/#5/#8). Every
anchored claim has GT + a baseline/null.

**All model-dependent findings now run on the published weights.** `exp4` Knossos hand-ID +
`exp3` tablet embeddings/contextual-parallels re-run on the published release (and LA also on
benchmark); folded into #2. The `exp3` contextual-parallel retrieval demos (qualitative, no
GT) ran on release too — referenced as a tool in #5, not scored.

## Re-run protocol (per `../METHODOLOGY.md`)

1. Re-extract sign embeddings from the released classifiers (penultimate layer), both
   scripts → fresh embedding store. (LA crops: `data/la_classifier_release_v1`; LB:
   `data/lb_labeller_release` — local, match the released classifiers.)
2. Re-run each theme; report the delta vs the prior internal number.
3. Detector fixed to the released unified weights.
4. Gate every claim (anchored P@K, null + significance, secure-vs-candidate, negatives in).

## Suggested order

Start with the **validated** headlines (registers 1 + 2 — they're the credibility
anchors), then the **negative** (7, the centrepiece), then discovery (3–5). Scribal
hands (2) is the cleanest first re-run (no class-name anchoring friction).
