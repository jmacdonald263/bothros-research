# Classifier-as-oracle — the pure sign-ID ceiling, and a first for Aegean syllabaries

**Register: matches.** Strip away detection error — feed the classifier *perfect*
gold-box / human-traced sign crops — and ask: how often does it name the sign correctly?
This is the classification **ceiling**, the number a researcher needs to separate "the
classifier is wrong" from "the detector missed the box." It is also, as far as we know, the
first published sign-classification accuracy for Linear A and Linear B against gold crops.

## Result (shipped benchmark weights, held-out)

| script | crops (tablets) | **Top-1** | Top-5 | 95% CI (Top-1) |
|---|---|---|---|---|
| **Linear A** | 87 pairs (52) | **79.3%** | 83.9% | Wilson [69.6, 86.5] · cluster [70.0, 89.2] |
| **Linear B** | 904 (64) | **64.5%** | 73.8% | cluster [53.5, 73.8] |

- **LA**: `la_classifier.pth` on the held-out oracle GT (gold boxes).
- **LB**: `lb_classifier.pth` on **904 tight human-traced imagemap crops** (the honest
  crop set — see the trap below).

For loose context, DeepScribe (Williams et al.) reports ≈74% top-1 for Elamite cuneiform
sign classification. Our LA gold-crop top-1 (79.3%) is in that range; LB (64.5%) below it.
This is **not** a head-to-head — different script, corpus, and class count — only a sanity
check that the Aegean classifiers are competitive with the state of the art for an
analogous ancient-sign-ID task. The reportable contribution is that *no such number existed
for LA/LB before*; the comparison is decoration, not the claim.

## The trap we did not fall into (pre-declared)

There are two "LB oracle" numbers in the project, and only one is honest:

- **64.5%** — `eval_clean_oracle_crops.py` on **tight, human-traced** crops (904). Reported.
- **22.3%** — `eval_classifier_oracle.py` on loose detector-aligned crops
  (`lb_clean_eval_v1`). **Not** an oracle of the classifier — it folds in crop-box slop, so
  it understates classifier quality badly.

We report 64.5% and name the 22.3% explicitly so no future reader cites the wrong one.
([Why this matters](../METHODOLOGY.md): a number is only a finding if you can say exactly
how it was computed.)

## The leakage line (why these are the *benchmark*, not the release, weights)

The full-data **release** classifiers score this same oracle far higher — LA ≈90%, LB
≈87% — but they were trained on the held-out tablets, so that is **memorisation, not
skill**. The honest ceiling is the held-out benchmark figure above. This is the same
discipline applied to the [demo-toggle decision](../../bothros/docs/DEMO_TOGGLE_DECISION.md):
release numbers look better and mean less.

## Reproduce

```bash
# LB (regenerates 64.5%):
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/eval_clean_oracle_crops.py \
    --classifier release/weights/lb_classifier.pth
# LA (79.3% already persisted): data/oracle_la_clean_v1_heldout.json (meta.top1)
```
