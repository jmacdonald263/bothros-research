# Methodology

## Embeddings

Sign embeddings are the penultimate-layer activations of the BOTHROS classifiers
(ConvNeXt-Tiny, one per script) over gold-box sign crops. The detector is fixed to the
shipped unified YOLO11 weights, so figures come from one pipeline.

## Weights: release vs. benchmark

Two published weight sets are used and reported side by side:

- **release** (`*_release.pth`) — trained on the full corpus, including the held-out split.
  This is the downloadable model. Its gold-crop oracle accuracy is an *in-domain ceiling*:
  the test crops are in its training set, so the figure overstates performance on unseen
  tablets.
- **benchmark** (`*_classifier.pth`) — trained with the held-out split withheld. Its figures
  are held-out estimates.

Where a result could be driven by memorisation, both are reported and the difference stated.
Embedding *structure* is less sensitive to this than top-1 accuracy.

## Anchoring cross-script claims

Cross-script relatedness is scored as **precision@K against published correspondence pairs**,
with a baseline (an ImageNet backbone with no Aegean training) as the null. Raw
nearest-neighbour distance between scripts is not used as evidence: it is confounded by
per-script drawing/rendering style, so two scripts drawn in a similar style cluster together
regardless of sign identity. An early attempt to "place" the Phaistos Disc relative to Linear
A on bare proximity was discarded for this reason — the ordering it produced (a genuine
sister script sitting *farther* from LA than an isolate) reflected rendering style, not
relatedness. The anchored metric avoids this.

## Reporting

Each finding states its ground truth, baseline/null, and significance test (permutation,
Wilson or cluster-bootstrap CI), and marks secure ground truth separately from
model-proposed candidates. Results that weaken under these checks are reported as such.
