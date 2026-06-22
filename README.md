# bothros-research

Exploratory findings from the [BOTHROS](https://github.com/jmacdonald263/bothros)
Aegean sign-reading pipeline — *what a vision model recovers from Linear A / Linear B
sign shapes that isn't obvious by eye.* Companion to the tool repo
([github.com/jmacdonald263/bothros](https://github.com/jmacdonald263/bothros)); weights on
[Hugging Face](https://huggingface.co/JMacD263/linear-a-linear-b-bothros), live
[demo Space](https://huggingface.co/spaces/JMacD263/bothros-demo), archived on
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

Model-dependent findings are run on **both published BOTHROS weight sets on Hugging Face**
and report the delta: the **full-data release** (`*_release.pth`, featured — the model a
user actually downloads) and the **held-out-safe benchmark** (`*_classifier.pth`). Sign
embeddings are the penultimate-layer activations of those classifiers. The two columns
answer different questions — the release number is the deployed model's performance
(inflated to an *in-domain ceiling* on the oracle, since the test crops are in its training
set), and the benchmark number is the honest held-out generalisation estimate. A recurring
result: **"most data" wins some tasks and loses others** (it beats benchmark on the Pylos
scribal Mantel and the in-domain oracle, but is marginally *worse* on cross-script cognates
and Knossos hand-ID). The **model-free** findings (entropy / Heaps / sign-network / topics /
restoration) read only the sign sequences — no classifier — and are leakage-immune by
construction. See [`METHODOLOGY.md`](METHODOLOGY.md).

## Licence

- **Code / notebooks:** MIT.
- **Derived data** (GT correspondence tables, hand labels, embedding-derived tables):
  CC BY-NC-SA 4.0 — derived from DĀMOS, SigLA, lineara.xyz, LinearBExplorer, GORILA
  (research-only). **No copyrighted corpus images** are included; reproductions fetch
  tablet images from those sources.
