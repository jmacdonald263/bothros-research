# Cross-script LA↔LB sign cognates — the embedding recovers published correspondences

**Register: matches + extends.** The credibility anchor: a vision model, given only sign
*shapes*, recovers the Linear A ↔ Linear B sign correspondences scholars established from
decipherment — and ranks them in the scholars' own confidence order. Reported on **both
published weight sets** (full-data release + held-out benchmark) with **two independent
null baselines**. It clears the [rigor gate](../METHODOLOGY.md) on every count.

## Setup

- **Anchor (published GT):** the 54 known LA↔LB correspondences resolvable in the shipped
  vocabulary (`corpus.db` `sign_correspondences`), pre-stratified into **secure (11)**,
  **probable (32)**, **possible (11)**.
- **Method:** embed each LA/LB sign crop through a ConvNeXt-Tiny backbone (penultimate
  layer), take per-class centroids, rank LB signs by cosine for each LA sign. **P@K = is
  the published correspondent within the LA sign's top-K LB neighbours.**
- **`la_shared`** (both scripts through the LA backbone) is the headline space; `lb_shared`
  disclosed alongside.

## Result — both published weight sets (`la_shared`)

| weights | secure P@1 | secure P@5 | all-54 P@1 | all-54 P@5 |
|---|---|---|---|---|
| **benchmark** (`la_classifier.pth`, held-out-safe) | **90.9%** (10/11) | **100%** (11/11) | 75.9% | **94.4%** |
| **release** (`la_classifier_release.pth`, full-data) | 81.8% (9/11) | **100%** (11/11) | 72.2% | 92.6% |

**Both published models hit secure P@5 = 100%** — every high-confidence correspondence is
recovered in the top 5. They rank the tiers in the scholars' order too (benchmark P@1:
secure 90.9% > probable 75.0% > possible 63.6%).

**Honest delta — more data did *not* help here.** The full-data release model is **slightly
worse** than the held-out benchmark on this task (secure P@1 81.8% vs 90.9%; all-54 P@5
92.6% vs 94.4%). Unlike the [oracle](classifier_oracle.md) — where the release model's extra
(memorised) data inflates the number — cross-script cognate recovery is a *relational* task
the extra tablets don't improve; if anything the larger, noisier release vocabulary blurs
the centroids a little. Worth stating plainly: "best model = most data" is **not** a
universal rule, and this finding is a clean counter-example.

## Two nulls — the signal is real, and backbone choice is decisive

P@5 on all 54, ranked:

| embedding | P@1 (all) | P@5 (all) |
|---|---|---|
| **DINOv3 ViT-B** (generic self-supervised, no Aegean training) | **0%** (0/58) | ~0% |
| ImageNet ConvNeXt (no Aegean training) | 1.9% | 18.5% |
| `lb_shared` (LB-backbone, fine-tuned) | 59.3% | 87.0%–79.6% |
| **`la_shared`** (LA-backbone, fine-tuned) | **72–76%** | **92.6–94.4%** |

Two distinct nulls fail: a generic ImageNet backbone scores 18.5% P@5 (its neighbours
collapse onto hub signs — LB numerals, `me`), and **DINOv3 — a strong modern
self-supervised ViT — scores 0/58 P@1**, worse than ImageNet. The cross-script signal is
**not** generic visual similarity: it requires a backbone *fine-tuned on Aegean signs*.
That the fancier self-supervised model does worse is itself the point — task-specific
training, not model size or recency, is what carries sign identity across scripts.

## Honest limits

- **Small secure-n.** 11 secure pairs; benchmark P@1 90.9% rests on 10/11. The two nulls at
  ~0–18% are what make the small n decisive — the gap, not the point estimate, is the claim.
- **Candidate vs confirmed.** Only the *secure* tier confirms the method; probable/possible
  are the model's agreement with progressively weaker scholarly proposals.
- **Reproduces the prior internal headline** ("100% P@5 on 11 secure") on both weight sets.

## Reproduce

```bash
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/cross_script_embeddings.py            # benchmark weights
  /opt/homebrew/bin/python3.12 src/cross_script_embeddings.py --release  # full-data weights
# -> data/cross_script_similarity_{la_classifier,la_classifier_release}__*.json
# DINOv3 null: src/expa4_dinov3_cross_script.py -> data/expa4_dinov3_cross_script.json
```

Contrast with [the Phaistos negative](NEGATIVE_phaistos.md): that claim had no GT anchor and
died on the rendering-style confound. This one is anchored to 54 published correspondences
with **two** baseline nulls — the whole difference.
