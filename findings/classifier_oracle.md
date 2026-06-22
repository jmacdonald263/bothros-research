# Classifier gold-crop accuracy

Feed the classifier perfect gold-box / human-traced sign crops and measure how often it
names the sign correctly. This isolates classifier quality from detector error and gives a
classification ceiling. Both published weight sets are reported, because the difference
between them is the memorisation effect.

## Result — both published weight sets

| script | crops (tablets) | RELEASE (full-data) Top-1 | BENCHMARK (held-out) Top-1 | gap |
|---|---|---|---|---|
| **Linear A** | 87 (52) | **89.7%** (78/87) · top-5 94.3% | **79.3%** (CI [69.6, 86.5]) · top-5 83.9% | +10.4 |
| **Linear B** | 904 (64) | **86.7%** (784/904, CI [81.1, 90.8]) · top-5 88.7% | **64.5%** (CI [53.5, 73.8]) · top-5 73.8% | +22.2 |

Both are the **weights published on Hugging Face**: `*_classifier_release.pth` (full-data,
the model a user actually downloads and runs) and `*_classifier.pth` (held-out-safe).

The two columns answer different questions. The task is "predict the label of this crop,"
and the release model trained on the full corpus including these held-out crops, so its
89.7% / 86.7% is in part recall of training data — the right figure for tablets the deployed
model has seen, an over-estimate for a novel photograph. For novel input the held-out column
(79.3% / 64.5%) is the honest estimate. A real upload sits between, leaning toward
held-out the more novel it is.

The cross-script and scribal-hand findings do not carry the same caveat: their tasks are not
"predict the trained-on label," so contamination there is indirect. The oracle is the direct
case, so both numbers are reported.

## By training frequency

The overall figure is dragged down by rare signs. Stratifying the Linear A oracle by how
often each sign appears in training:

| training-frequency stratum | eval signs | benchmark top-1 | release top-1 |
|---|---|---|---|
| ≥20 instances | 80 | **83.8%** | 92.5% |
| 5–19 | 3 | 66.7% | 66.7% |
| <5 | 2 | 0% | 66.7% |
| unseen in training | 1–2 | 0% | 0% |

The ≥20-instance stratum is the comparable figure: an Old-Babylonian cuneiform ResNet-50
(arXiv:2507.13959) reports 87.1% top-1 on its ≥20-instance signs; the benchmark LA classifier
is 83.8% on the same footing, just below it. The `<5` stratum (0%) is the long-tail floor —
the classifier's main weakness, and where rare-sign methods would have to improve. The
held-out eval is small (87 signs), so the strata are directional.

## Training-free prototype head

Can a sign be classified without the trained softmax head at all — by nearest class
*centroid* in embedding space (the premise behind a "gallery" / template-matching classifier,
where adding a sign needs one example and no retraining)? Centroids built from training crops
(penultimate-layer embeddings, capped at 25/class), held-out crops classified by cosine
nearest centroid, same crops and label mapping as above:

| LA weights | prototype top-1 | prototype top-5 | softmax top-1 | softmax top-5 |
|---|---|---|---|---|
| benchmark | 71.3% (CI [61.0, 79.7]) | 87.4% | 79.3% | 83.9% |
| release | 86.2% (CI [77.4, 91.9]) | 91.9% | 89.7% | 94.3% |

The training-free head is competitive but below the trained softmax on top-1 (−8pp
benchmark, −3.5pp release), while its top-5 is comparable or slightly higher. It does **not**
rescue the long tail — the `<5`-instance and unseen strata are 0% as with softmax. So it is
useful as a flexibility feature (add a class from one exemplar, no retraining) rather than an
accuracy gain.

A per-exemplar **k-NN(1)** variant (nearest individual training crop, not the centroid) was
also tested and is **worse**, not better: 57.5% top-1 vs the centroid's 71.3% (benchmark LA).
On clean gold crops a single nearest neighbour is noisier than the class mean, so averaging
helps here. Neither head rescues the `<5`-instance tail (0% under softmax, centroid, and
k-NN alike) — that floor is data scarcity, not the head. (k-NN did edge the centroid on the
5–19 stratum, but n=3.)

## DeepScribe context

DeepScribe (Williams et al.) reports ≈74% top-1 for Elamite cuneiform sign classification.
LA gold-crop top-1 is in that range on either weight set. This is not a head-to-head
(different script, corpus, and class count); we note it only because no comparable LA/LB
figure was published before.

## Crop definition (avoiding a misleading number)

Two "LB oracle" numbers exist; only the tight-crop one is a classifier oracle:

- `eval_clean_oracle_crops.py` on 904 tight human-traced crops — reported above
  (release 86.7% / benchmark 64.5%).
- `eval_classifier_oracle.py` on loose detector-aligned crops (`lb_clean_eval_v1`) gives
  ~22% for the benchmark weights — this folds in crop-box error and is not a classifier
  oracle. Noted so it is not cited as one.

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
