# Cross-script LA↔LB correspondences from visual similarity

Given only sign shapes, the classifier embedding recovers Linear A ↔ Linear B sign
correspondences established from decipherment, and ranks them in the published confidence
order. Reported on both weight sets (release + benchmark) against an ImageNet baseline null.

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

Both weight sets reach secure P@5 = 100% — every high-confidence correspondence is in the
top 5 — and both rank the tiers in the published order (benchmark P@1: secure 90.9% >
probable 75.0% > possible 63.6%).

The full-data release model is slightly worse here than the held-out benchmark (secure P@1
81.8% vs 90.9%; all-54 P@5 92.6% vs 94.4%). Unlike the [oracle](classifier_oracle.md), where
the release model's extra (memorised) data inflates the figure, this is a relational task the
extra tablets do not improve — the larger, noisier release vocabulary blurs the centroids
slightly. More training data is not uniformly better.

## Baselines

P@K on all 54, with two no-Aegean-training nulls:

| embedding | P@1 (all) | P@5 (all) |
|---|---|---|
| DINOv3 ViT-B (generic self-supervised) | 0% (0/58) | ~0% |
| ImageNet ConvNeXt | 1.9% | 18.5% |
| `lb_shared` (LB backbone, fine-tuned) | 59.3% | 79.6%–87.0% |
| `la_shared` (LA backbone, fine-tuned) | 72–76% | 92.6–94.4% |

Both nulls fail: ImageNet scores 18.5% P@5 (its neighbours collapse onto hub signs — LB
numerals, `me`), and DINOv3 scores 0/58 P@1. The signal requires a backbone fine-tuned on
Aegean signs; generic visual features, including a modern self-supervised ViT, do not carry
it.
## Limits

- 11 secure pairs; benchmark P@1 90.9% rests on 10/11. The baselines at ~0–18% are what make
  the small sample informative — the gap, not the point estimate.
- Only the secure tier is treated as confirmation; probable/possible report the model's
  agreement with progressively weaker scholarly proposals.
- Reproduces the prior internal estimate ("100% P@5 on 11 secure") on both weight sets.

## Reproduce

```bash
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  python3 src/cross_script_embeddings.py            # benchmark weights
  python3 src/cross_script_embeddings.py --release  # full-data weights
# -> data/cross_script_similarity_{la_classifier,la_classifier_release}__*.json
python3 src/expa4_dinov3_cross_script.py             # DINOv3 baseline null
```

This is scored against the 54 published correspondence pairs rather than raw cross-script
proximity, for the rendering-style confound described in [`../METHODOLOGY.md`](../METHODOLOGY.md).
