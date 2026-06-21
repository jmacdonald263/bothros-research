# Methodology

## Embedding pipeline

Sign embeddings are the **penultimate-layer activations** of the shipped BOTHROS
classifiers (ConvNeXt-Tiny, one per script), extracted over gold-box sign crops. The
detector is fixed to the shipped unified YOLO11 weights, so every figure comes from one
pipeline (same discipline as the tool repo's scorecard).

**Which weights — and why it's the held-out-safe ones.** BOTHROS ships two classifier
variants: the **benchmark** weights (trained with the held-out split *excluded* — these
carry the published numbers) and a **release** variant (trained on the *full* corpus
including held-out → max-capability but **not benchmarkable**: its oracle top-1 is
inflated by memorisation — LA 90% vs 79% honest, LB 87% vs 64%). For every **anchored**
finding here (P@K on GT correspondences; scribal-hand Mantel) we use the **benchmark /
held-out-safe** weights as primary — precisely because the release weights memorised the
test tablets, which would confound a similarity or agreement statistic. The release
weights are a max-capability extra, never the basis of an anchored claim; where a
finding's robustness to the classifier is in doubt we report a benchmark-vs-release
*delta* rather than swapping the headline. (The purely **model-free** findings — entropy,
Heaps, sign-network — read only the sign sequences and so are leakage-immune by
construction; no classifier touches them at all.)

## The rigor gate — the "Phaistos rule" (non-negotiable)

Every "the model sees X" claim must clear **all** of these before it enters `findings/`:

1. **Anchored, not naked proximity.** Validate against *published* ground truth
   (sign correspondences, scribal hands) with **P@K** / agreement statistics. Raw
   embedding nearest-neighbours are never a finding on their own — that is precisely
   the trap that sank the Phaistos result.
2. **Baseline + significance + a stated null.** Permutation test / Mantel / bootstrap
   CI (tooling exists in the tool repo). State the null and the effect size, not just
   a point estimate.
3. **Pre-declare secure vs candidate.** Mark which pairs/labels are *secure GT* and
   which are *model-proposed candidates* up front, so discovery is never laundered as
   confirmation.
4. **Negatives included.** Results that weaken or vanish under the gate are reported,
   not retuned until they return. The retracted Phaistos result ships as a worked
   example of the confound.

## The Phaistos confound (why it's featured)

An early result appeared to "place" the Phaistos Disc signs near Linear A in embedding
space. It was **retracted** (2026-05-27): cross-script proximity was confounded by
**per-script rendering style** (stroke weight, medium, drawing convention) rather than
sign identity — two scripts drawn in the same style cluster together regardless of what
the signs mean. The fix is the rigor gate above (anchor to GT correspondences with P@K;
control for rendering style). See [`findings/NEGATIVE_phaistos.md`](findings/NEGATIVE_phaistos.md).

## Honest-delta protocol

Each experiment is re-run against the released-model embedding store, and its number is
reported **alongside the prior internal estimate** (see `findings/_INDEX.md`). A finding
that *weakens* with better tools is itself reportable — it is not hidden or re-tuned.
