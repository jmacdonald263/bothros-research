# Classifier-as-oracle — the pure sign-ID ceiling, and the memorisation gap laid bare

**Register: matches.** Feed the classifier *perfect* gold-box / human-traced sign crops and
ask how often it names the sign correctly — the classification **ceiling**, isolating
classifier quality from detector error. As far as we know this is the first published
gold-crop sign-classification accuracy for Linear A and Linear B. It is also the one
finding where the **released full-data** weights and the **held-out benchmark** weights
must both be shown, because the gap between them *is* the memorisation effect — measured.

## Result — both published weight sets

| script | crops (tablets) | RELEASE (full-data) Top-1 | BENCHMARK (held-out) Top-1 | gap |
|---|---|---|---|---|
| **Linear A** | 87 (52) | **89.7%** (78/87) · top-5 94.3% | **79.3%** (CI [69.6, 86.5]) · top-5 83.9% | +10.4 |
| **Linear B** | 904 (64) | **86.7%** (784/904, CI [81.1, 90.8]) · top-5 88.7% | **64.5%** (CI [53.5, 73.8]) · top-5 73.8% | +22.2 |

Both are the **weights published on Hugging Face**: `*_classifier_release.pth` (full-data,
the model a user actually downloads and runs) and `*_classifier.pth` (held-out-safe).

**Read the two columns correctly — this finding is the one place the distinction bites.**
The oracle task is literally "predict the label of *this* crop," and the release model was
trained on the full corpus *including these exact held-out crops*. So its 89.7% / 86.7% is
partly **reciting memorised answers** — it is the right number for "how good is the deployed
model on tablets it has seen," and an **over-estimate** for a researcher's genuinely novel
photograph. For novel input the honest estimate is the **held-out** column (79.3% / 64.5%),
where the eval crops were withheld from training. A real upload sits between, leaning toward
held-out the more novel it is.

(This is *why* the other findings here — [cross-script](cross_script_cognates.md),
[scribal hands](scribal_hands.md) — can feature the release weights without the same
caveat: their tasks are not "predict the trained-on label," so the contamination is
indirect. The oracle is the strict case, so it carries both numbers explicitly.)

## DeepScribe context (loose)

DeepScribe (Williams et al.) reports ≈74% top-1 for Elamite cuneiform sign classification.
LA gold-crop top-1 is in that range on either weight set; the reportable point is that **no
such number existed for LA/LB before**, not a head-to-head (different script/corpus/classes).

## The crop trap we did not fall into (pre-declared)

Two "LB oracle" numbers exist; only the tight-crop one is a real oracle:

- **`eval_clean_oracle_crops.py` on 904 tight human-traced crops** — reported above
  (release 86.7% / benchmark 64.5%).
- **`eval_classifier_oracle.py` on loose detector-aligned crops** (`lb_clean_eval_v1`) gives
  ~22% for the benchmark weights — that folds in crop-box slop and is **not** a classifier
  oracle. Named here so no future reader cites it by mistake.

## Reproduce

```bash
# LB, both weight sets (904 tight crops):
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/eval_clean_oracle_crops.py \
    --classifier release/weights/lb_classifier_release.pth   # 86.7%  (release)
#   --classifier release/weights/lb_classifier.pth           # 64.5%  (benchmark)
# LA, both weight sets (held-out oracle GT):
  /opt/homebrew/bin/python3.12 src/eval_classifier_oracle.py \
    --classifier release/weights/la_classifier_release.pth \
    --gt data/la_clean_heldout_oracle.json                   # 89.7%  (release)
#   --classifier release/weights/la_classifier.pth ...       # 79.3%  (benchmark)
```
