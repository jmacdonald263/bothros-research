# Cross-script LA↔LB sign cognates — the embedding recovers published correspondences

**Register: matches + extends.** This is the credibility anchor: a vision model, given
only sign *shapes*, recovers the Linear A ↔ Linear B sign correspondences that scholars
established from decipherment — and ranks them with the right confidence order. It clears
the [rigor gate](../METHODOLOGY.md) on every count, and it is reported against the
**shipped benchmark (held-out-safe) weights**, *not* the full-data release weights — the
anchored metric must not be contaminated by the test-set memorisation that inflates the
release classifiers.

## Setup

- **Anchor (published GT):** the 54 known LA↔LB correspondences resolvable in the shipped
  vocabulary, from `corpus.db` `sign_correspondences`, pre-stratified by confidence into
  **secure (11)**, **probable (32)**, **possible (11)**. Secure = the high-confidence
  published equivalences; probable/possible are progressively weaker scholarly proposals.
- **Method:** embed every LA and LB sign crop through a script's ConvNeXt-Tiny backbone
  (penultimate layer), take per-class centroids, and for each LA sign rank all LB signs by
  cosine. **P@K = is the published LB correspondent within the LA sign's top-K LB
  neighbours.**
- **Three embedding spaces, one of them the null:**
  `pretrained` (ImageNet, no Aegean training) is the **baseline** — if shape alone
  explained the matches, it would score; `la_shared` / `lb_shared` route both scripts
  through the LA / LB fine-tuned backbone.

## Result

**`la_shared` (LA-backbone embedding) — the winner, tier-stratified:**

| tier (n) | P@1 | P@3 | P@5 | P@10 |
|---|---|---|---|---|
| **secure (11)** | **90.9%** (10/11) | 100% | **100%** (11/11) | 100% |
| probable (32) | 75.0% | 93.8% | 96.9% | 96.9% |
| possible (11) | 63.6% | 81.8% | 81.8% | 81.8% |
| **all (54)** | **75.9%** (41/54) | 92.6% | **94.4%** (51/54) | 94.4% |

**Baseline vs signal (P@5, all 54):**

| embedding | P@1 | P@5 | P@10 |
|---|---|---|---|
| `pretrained` (ImageNet null) | 1.9% | **18.5%** | 40.7% |
| `lb_shared` (LB backbone) | 59.3% | 79.6% | 90.7% |
| **`la_shared` (LA backbone)** | **75.9%** | **94.4%** | 94.4% |

Two things matter here:

1. **The signal is real, not shape-coincidence.** The ImageNet baseline scores 18.5% P@5
   (secure-tier P@1 = **0/11**) — its nearest neighbours collapse onto a few hub signs
   (LB numerals, `me`). The Aegean-fine-tuned `la_shared` space lifts that to **94.4% P@5
   / 90.9% secure P@1**. The lift over the null *is* the finding.
2. **The model ranks the tiers in the scholars' own order.** secure (90.9% P@1) >
   probable (75.0%) > possible (63.6%). The embedding "agrees more" with the
   correspondences epigraphers are more sure of — exactly what a faithful signal should do,
   and not something you can get by tuning.

## Honest deltas & limits

- **Reproduces the prior internal headline exactly:** "100% P@5 on the 11 secure
  correspondences" → confirmed, 11/11, now with shipped weights and a full tier breakdown.
- **Small secure-n.** 11 secure pairs is few; P@1 90.9% rests on 10/11. The ImageNet
  baseline at 0/11 is what makes even this small n decisive — the gap, not the point
  estimate, is the claim.
- **Directionality.** `la_shared` (both scripts through the LA backbone) beats `lb_shared`.
  We report `la_shared` as the headline and disclose both; the asymmetry is itself worth
  noting (the LA backbone's feature space separates these signs better).
- **Candidate vs confirmed.** Only the *secure* tier is treated as confirmation of the
  method; probable/possible results are reported as the model's agreement with weaker
  proposals, not as new claims about those pairs.

## Reproduce

```bash
# shipped benchmark weights (release/weights/la_classifier.pth + lb_classifier.pth);
# MPS — run ONE at a time, cap memory (see ../../bothros/docs note):
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 /opt/homebrew/bin/python3.12 \
    src/cross_script_embeddings.py
# -> data/cross_script_similarity_la_classifier__lb_classifier.json
#    (precision_at_k by tier, per embedding method)
```

Contrast with [the Phaistos negative](NEGATIVE_phaistos.md): *that* claim had no GT anchor
and died on the rendering-style confound. This one is anchored to 54 published
correspondences with a baseline null — which is the whole difference.
