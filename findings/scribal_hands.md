# Scribal hands — the embedding agrees with expert palaeography (Pylos)

**Register: matches.** Given only sign-crop *shapes*, the LB classifier's embedding groups
Pylos tablets by **who wrote them** in a way that correlates with Emmett Bennett /
Skelton's hand attributions — established by eye, decades before any vision model. This is
the cleanest "AI matches the experts" result in the repo: it is anchored to an **external**
palaeographic coding (not the model's own labels), so leakage is not even in play.

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

## Honest limits

- **Small n.** 8 hands / 28 pairs is the binding constraint; r ≈ 0.65–0.68 at p ≈ 0.02–0.03
  is a real but moderate effect on a small sample. We report it as corroborating expert
  palaeography on Pylos, not as a general scribe-ID method.
- **Pylos-only.** Other find-spots aren't coded densely enough for the same test yet (a
  Knossos hand-ID extension on published weights is the obvious next step).
- **Reproduces the prior internal estimate** (an earlier unpublished checkpoint gave
  r = 0.674) — the published-weights runs land in the same band, so the result held up.

## Why it clears the [rigor gate](../METHODOLOGY.md)

Anchored to a *published, external* coding (not the model's labels); significance from a
permutation null, not a bare correlation; n and effect size stated openly; scope limited to
what the data supports (Pylos, 8 hands). The agreement with Skelton is the kind of
independent corroboration that makes the [cross-script result](cross_script_cognates.md)
credible too — same discipline, different anchor.

## Reproduce

```bash
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/expk3_pylos_mantel.py            # benchmark (labeller_v3), r=0.649
  /opt/homebrew/bin/python3.12 src/expk3_pylos_mantel.py --release  # full-data, r=0.684
```
