# bothros-research

Exploratory analyses built on the [BOTHROS](https://github.com/jmacdonald263/bothros)
Aegean sign-reading pipeline — structure in Linear A and Linear B sign data recovered by a
vision model and by corpus statistics. Companion to the tool repo; weights on
[Hugging Face](https://huggingface.co/JMacD263/linear-a-linear-b-bothros), live
[demo](https://huggingface.co/spaces/JMacD263/bothros-demo), archived on
[Zenodo](https://doi.org/10.5281/zenodo.20746759).

These are experiments, not decipherment. Each finding states its ground truth, baseline,
and significance test, and reports results that weaken or fail as readily as those that
hold.

## Findings

See [`findings/`](findings/) — index and per-theme results in
[`findings/_INDEX.md`](findings/_INDEX.md). Each page carries its reproduction command and
a stated null / significance test. Themes:

- Linear A ↔ Linear B sign correspondences from visual similarity, scored against published
  correspondence tiers.
- Scribal-hand structure vs. catalogued hand attributions (Pylos Mantel; Knossos hand-ID).
- Sign-function classification (logogram vs. syllabogram) from distributional features.
- Corpus structure: conditional entropy, Heaps/Zipf inventory growth, sign-adjacency
  networks, topic models, with a matched-corpus-size control.
- Sign restoration (n-gram and Transformer) and classifier gold-crop accuracy.

A larger planned piece — a character-based sign phylogeny across the Aegean scripts, as the
principled alternative to cross-script proximity — is scoped in [`PHYLOGENY_PLAN.md`](PHYLOGENY_PLAN.md)
(design only, not yet run).

## Method and weights

Model-dependent findings are run on both published weight sets and report the difference:
the full-data **release** classifiers (`*_release.pth`, the downloadable model) and the
held-out-safe **benchmark** classifiers (`*_classifier.pth`). They answer different
questions — the release figure is the deployed model's accuracy (an in-domain ceiling on
the gold-crop oracle, whose test crops are in its training set), the benchmark figure is the
held-out estimate. More training data helps some tasks (scribal Pylos, in-domain oracle) and
not others (cross-script, Knossos hand-ID); results are reported as measured. Model-free
findings (entropy, Heaps, networks, topics, restoration) read only sign sequences.

Cross-script claims are scored against published correspondence pairs (precision@K), not raw
embedding proximity: nearest-neighbour distance across scripts is confounded by per-script
drawing style, so an anchored metric is used throughout. See [`METHODOLOGY.md`](METHODOLOGY.md).

## Licence

- **Code / notebooks:** MIT.
- **Derived data** (correspondence tables, hand labels, derived statistics): CC BY-NC-SA 4.0,
  derived from DĀMOS, SigLA, lineara.xyz, LinearBExplorer, GORILA (research use). No
  copyrighted corpus images are included; reproductions fetch images from those sources.
