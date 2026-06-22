# Scribal hands (Pylos + Knossos, LB + LA)

Given only sign-crop shapes, the classifier embedding groups tablets by who wrote them, in
agreement with catalogued hand attributions (Bennett / Skelton). The anchor is an external
palaeographic coding, not the model's own labels, so the result does not depend on the
classifier's training labels.

## Setup

- **Anchor (external GT):** Skelton's palaeographic hand coding for Pylos scribes — a
  human, shape-based grouping of tablets by scribe, independent of our pipeline.
- **Embedding:** each Pylos tablet → mean of its sign-crop embeddings (LB ConvNeXt-Tiny);
  tablets grouped by hand (PyNNN→hand via a Jaccard ID-bridge). 5,884 crops across 137 PyNNN
  tablets; 128 mapped; 12 hands with ≥2 tablets, **8 usable** (both Skelton-coded and
  embedded).
- **Test:** **Mantel test** — correlate the inter-hand distance matrix in embedding space
  against Skelton's, with a **permutation null** (label shuffles) for significance.

## Result — both published weight sets

| LB weights | Mantel **Pearson r** | permutation **p** |
|---|---|---|
| **release** (`lb_classifier_release.pth`, full-data) | **0.684** | **0.020** |
| **benchmark** (`lb_classifier.pth`, held-out-safe) | 0.649 | 0.030 |

n = 8 hands (28 pairs) in both. **Both significant at p < 0.05:** the embedding's
hand-to-hand distances track the palaeographer's. The model recovers scribal structure —
individual handwriting — from sign shape alone, on either published model.

**Here the full-data release edges the benchmark** (r 0.684 vs 0.649) — more training data
helped. Note this is the *opposite* of the [cross-script result](cross_script_cognates.md),
where release was marginally worse: "more data" helps some tasks and not others, and we
report each as measured rather than assuming a rule.

## Result B — Knossos direct hand-ID (a second site, and Linear A)

A different test on a different site: instead of a Mantel correlation, **classify each
Knossos tablet's scribal hand by nearest-centroid in embedding space** (leave-one-out),
against the catalogued hand attributions. Extends the result from LB-Pylos to **LA-Knossos**
and a second method.

| script / weights | hands (tablets) | **top-1** | top-5 | baseline (majority hand) | NMI |
|---|---|---|---|---|---|
| **LA — benchmark** | 23 (141) | **44.7%** | 77.3% | 17.7% | 0.494 |
| **LA — release** | 23 (141) | 42.6% | 75.2% | 17.7% | 0.446 |
| **LB — release** | 52 (510) | 22.4% | 53.7% | 18.6% | 0.335 |

**Linear A hand-ID is a clear signal** — top-1 ≈ 43–45%, **~2.5× the majority-hand
baseline** (17.7%), NMI ≈ 0.45–0.49, on either published model. So the "the embedding sees
the scribe" effect is not a Pylos/LB artefact: it holds for **Linear A at Knossos** too.
(Benchmark again edges release slightly, as on cross-script.)

**Linear B at Knossos is weak** — top-1 22.4% is barely above its 18.6% baseline (52 hands
spread over 510 tablets is a hard 52-way problem with few tablets each). Reported honestly
as marginal, not as a second LB win.

## Honest limits

- **Small n / hard multiway.** Pylos Mantel rests on 8 hands; LA Knossos is a 23-way
  problem on 141 tablets; LB Knossos a 52-way problem barely above baseline. These are
  corroborations of expert palaeography on the better-sampled cases, not a general scribe-ID
  tool.
- **"More data" again non-universal.** Benchmark ≥ release on both the LA Mantel-adjacent
  and Knossos hand-ID — consistent with [cross-script](cross_script_cognates.md); the
  full-data model's edge shows up on the oracle (memorisation) and Pylos Mantel, not here.
- **Reproduces the prior internal estimate** (an earlier unpublished checkpoint gave Pylos
  r = 0.674 and LA Knossos top-1 ≈ 45%) — the published-weights runs land in the same band.

## Reproduce

```bash
# Pylos Mantel (vs Skelton):
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/expk3_pylos_mantel.py            # benchmark (labeller_v3), r=0.649
  /opt/homebrew/bin/python3.12 src/expk3_pylos_mantel.py --release  # full-data, r=0.684
# Knossos direct hand-ID (LA/LB): build tablet embeddings, then leave-one-out classify:
  /opt/homebrew/bin/python3.12 src/exp3_contextual_parallels.py [--release] --out <npz>
  /opt/homebrew/bin/python3.12 src/exp4_hand_clustering.py --script LA --npz <npz>  # LA 44.7%/42.6%
```
