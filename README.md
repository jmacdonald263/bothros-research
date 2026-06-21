# bothros-research

Exploratory findings from the [BOTHROS](https://github.com/jmacdonald263/bothros)
Aegean sign-reading pipeline — *what a vision model recovers from Linear A / Linear B
sign shapes that isn't obvious by eye.* Companion to the tool repo; weights on
[Hugging Face](https://huggingface.co/JMacD263/linear-a-linear-b-bothros), archived on
[Zenodo](https://doi.org/10.5281/zenodo.20746759).

> **Research preview.** These are honest experiments, not decipherment. Every claim
> here clears the [rigor gate](METHODOLOGY.md) — anchored to published ground truth
> with significance tests — or it doesn't ship. The retracted Phaistos result is kept
> as a worked example of how this can go wrong.

## Three registers (read all three — the third is what makes the rest trustworthy)

1. **AI matches experts** *(validation).* Unsupervised structure from sign shapes that
   agrees with established scholarship — e.g. scribal-hand clusters vs Skelton's
   palaeography; the secure Linear A ↔ Linear B sign correspondences recovered by
   visual similarity alone.
2. **AI extends** *(discovery — candidate-only).* Structure the model proposes *beyond*
   the secure set (e.g. candidate LA↔LB correspondences), offered for human review and
   pre-declared as candidate, never as confirmation.
3. **AI fails / confounds** *(honesty).* The Phaistos-placement result, retracted because
   cross-script proximity was confounded by per-script rendering style. Featured, with
   the diagnostic — see [`findings/NEGATIVE_phaistos.md`](findings/NEGATIVE_phaistos.md).

## Findings

See [`findings/`](findings/) (index: [`findings/_INDEX.md`](findings/_INDEX.md)). Each
finding ships its reproduction script/notebook and a stated null + significance test.

## Reproducibility

Embedding-based findings use the **shipped, held-out-safe (benchmark) BOTHROS weights**
— one unified detector + the two classifiers — so the figures come from one pipeline and
no anchored claim rests on the leakage-prone full-data *release* variant (see
[`METHODOLOGY.md`](METHODOLOGY.md) for why). Sign embeddings are the penultimate-layer
activations of those classifiers. The **model-free** findings (entropy / Heaps /
sign-network) read only the sign sequences — no classifier — and are leakage-immune by
construction.

## Licence

- **Code / notebooks:** MIT.
- **Derived data** (GT correspondence tables, hand labels, embedding-derived tables):
  CC BY-NC-SA 4.0 — derived from DĀMOS, SigLA, lineara.xyz, LinearBExplorer, GORILA
  (research-only). **No copyrighted corpus images** are included; reproductions fetch
  tablet images from those sources.
