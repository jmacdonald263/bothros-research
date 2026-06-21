# Sequential structure & sign inventory — three methods, one honest ceiling

**Register: extends — with a self-imposed negative.** This is the one family of results
that **never touches the classifier**: it reads only the sign *sequences* in the corpus
(`corpus.db`). So it is leakage-immune by construction — the release-vs-benchmark
distinction (which haunts the embedding findings) cannot apply here. What it can still get
wrong is *over-interpretation*, and the headline below is the guard against exactly that.

## Result: CH > LB > LA on every sequential-structure metric — and that ordering is the finding's own warning

Three independent, model-free measures of "how much does sign *order* carry structure
beyond a bag of signs," each against its own shuffled/rewired null:

| Metric (↑ = more sequential structure) | Cretan Hieroglyphic | Linear B | Linear A |
|---|---|---|---|
| **Order signal** (H₂ shuffle-null − H₂, bits) | **1.48** (25.2% of H₁) | 0.60 (8.2%) | 0.42 (6.2%) |
| **Directional asymmetry gain** (vs degree-preserving rewire) | **+0.356** | +0.235 | +0.094 |
| **Community modularity gain** (vs rewire null) | **+0.137** | +0.075 | +0.068 |

All three agree: **CH > LB > LA**, all above their nulls. Tidy — and a trap.

**The ordering refutes the tempting reading.** Linear B *is* Mycenaean Greek — a known
natural language. Cretan Hieroglyphic is mostly short, formulaic seal-legends. If these
metrics measured *linguisticality*, LB would top CH. It doesn't — CH tops LB on all three.
So what they actually rank is **formulaicity / template-rigidity**, not linguistic depth.
This is [Sproat's (2010) critique](https://aclanthology.org/J10-4006/) of conditional-
entropy "is-it-language" tests — now demonstrated **with our own known-language control**
(LB) rather than asserted. **We therefore make no "LA is / isn't a language" claim from
this.** That restraint is the result.

What we *do* report: the relative regime (CH most templated; LB and LA looser, LA loosest)
and, separately, LA's inventory behaviour below.

## Inventory: LA sits in the natural-language band, but undersampled

Heaps' law (type growth β) + hapax fraction, per script:

| | tokens | types | hapax % | Heaps β | regime |
|---|---|---|---|---|---|
| **CH** | 3,781 | 135 | **5.9%** | **0.256** | inventory near-**saturated** (small fixed sign-set, well-sampled) |
| **LB** | 35,181 | 992 | 39.9% | 0.404 | **undersampled**, natural-language band |
| **LA** | 4,324 | 346 | **45.7%** | **0.451** | **undersampled**, natural-language band |

LA's β = 0.45 sits squarely in the natural-language band (0.4–0.6) — a *weak positive*
signal, but honestly flagged as **undersampling-confounded**: 45.7% of LA sign-types occur
once, so the projection (≈373 unseen types at 5× corpus) and the absolute entropies are
**lower bounds**, not estimates. Restricting LA to its ~100 syllabographic core drops β to
0.236 (hapax 22%) — i.e. the syllabary looks near-closed; it's the **logographic tail**
that inflates whole-corpus LA. That decomposition is itself the useful structural fact.

## A weak-but-real bonus: sign-adjacency communities partly recover sign *type*

The LA co-occurrence network's 12 communities align with the syllabogram / logogram /
fraction partition at **NMI 0.113 vs a 0.029 null** (purity 0.656) — ~4× the null. Weak,
but real: who-stands-next-to-whom carries a shadow of *what kind of sign it is*, with no
glyph-shape input at all. Pre-declared as a **candidate** structural signal, not a typology.

## How this clears the [rigor gate](../METHODOLOGY.md)

1. **Anchored / controlled** — every metric is reported as a *gain over a stated null*
   (within-document shuffle for entropy; degree-preserving rewire for the network), and the
   whole family carries a **known-language control (LB)** that is what exposes the
   formulaicity ceiling.
2. **Negative shipped, not buried** — the headline *is* the negative: the metric cannot
   adjudicate linguisticality, and we say so instead of cropping to "LA shows language-like
   structure."
3. **Secure vs candidate** — the relative regime and LA's β-band placement are reported as
   weak signals; the community→type recovery is flagged candidate.

## Reproduce

```bash
# model-free — reads corpus.db sign sequences only; needs networkx (python3.12)
/opt/homebrew/bin/python3.12 src/exp_entropy_linguisticality.py   # -> data/exp_entropy_linguisticality.json
/opt/homebrew/bin/python3.12 src/exp_heaps_inventory.py           # -> data/exp_heaps_inventory.json
/opt/homebrew/bin/python3.12 src/exp_sign_network.py              # -> data/exp_sign_network.json
```

Prior internal estimate (2026-06-18): qualitative "CH > LB > LA, don't overclaim." This
re-run **confirms it quantitatively and unchanged** — as expected for a model-free analysis.
