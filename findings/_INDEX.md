# Findings index & re-run tracker

Each theme is re-run against the **released-model** embedding store, then written up
with its reproduction script + the rigor-gate evidence (anchored P@K, null, significance).
"Prior internal" = the pre-release estimate to re-verify; report deltas honestly.

All re-runs below used the **shipped benchmark (held-out-safe) weights** — NOT the
full-data release weights — so every anchored metric is leakage-free. (The release weights
memorise the test tablets; using them here would inflate the numbers, as #6 documents.)

| # | Theme | Register | Result | Status |
|---|---|---|---|---|
| 1 | **Cross-script LA↔LB cognates** | matches + extends | `la_shared` **secure P@5 = 100%** (11/11), P@1 = 90.9%; all-54 P@5 = 94.4%; ImageNet baseline 18.5% (null) | ✅ [written](cross_script_cognates.md) — reproduces prior, + full tiers & baseline |
| 2 | **Scribal hands** (Pylos, Mantel vs Skelton) | matches | Mantel **r = 0.674, p = 0.036** (n=8 hands) | ✅ [written](scribal_hands.md) — reproduces exactly |
| 3 | **Sign structure** (allograph / logogram-vs-syllabogram / numerals) | extends | partly in #4 (LA syllabary β=0.236 vs whole-corpus 0.45; community→type NMI 0.11) | ⏳ remaining (own page TBD) |
| 4 | **Distributional / sequential structure** (entropy, Heaps, network) | extends + self-negative | CH>LB>LA on all 3 methods, but ranks **formulaicity not linguisticality** (LB control); LA β in NL band | ✅ [written](distributional_structure.md) — model-free, leakage-immune |
| 5 | **Restoration** (n-gram / transformer / contextual parallels) | extends | built; some never fully evaluated | ⏳ remaining |
| 6 | **Classifier-as-oracle** (DeepScribe context) | matches | **LA 79.3%** top-1 (CI [70,89]) · **LB 64.5%** (CI [53.5,73.8]); avoided the 22.3% trap; release inflates to 90/87 = memorisation | ✅ [written](classifier_oracle.md) |
| 7 | **NEGATIVE: Phaistos placement** | honesty | retracted 2026-05-27 (rendering-style confound) | ✅ [written](NEGATIVE_phaistos.md) — the centrepiece |

**5 of 7 themes written**, all three honest registers covered (matches #1/#2/#6, extends
#1/#4, negative #7 + the self-negative inside #4). #3 and #5 remain.

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
