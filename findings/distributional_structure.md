# Corpus structure — sequence order, inventory, and adjacency

Model-free analyses of the sign sequences in `corpus.db` (no classifier, so independent of
the weights). Linear A and Linear B are the focus; Cretan Hieroglyphic is included only as a
control where a three-way comparison is informative.

## Sequence order

How much does sign *order* carry structure beyond a bag of signs, measured against a
within-document shuffle null:

| order signal (H₂ shuffle-null − H₂, bits) | LB | LA | CH (control) |
|---|---|---|---|
| | 0.60 | 0.42 | 1.48 |

**Is LA's lower value real, or just its smaller corpus?** A matched-corpus-size bootstrap
(LB and CH subsampled to LA's ~4,200 tokens, 20 resamples) keeps the ordering:

| matched-size order signal (bits) | value [95% CI] |
|---|---|
| LA (full) | 0.476 [0.471, 0.481] |
| LB (subsampled to LA size) | 0.594 [0.574, 0.615] |
| CH (subsampled to LA size) | 1.560 [1.555, 1.565] |

LB stays above LA at equal size, so **LA genuinely carries less adjacent-sign structure
than LB** — not a sampling artifact.

What this does *not* show: that the metric measures language. CH (short, formulaic
seal-legends) scores highest, above LB (known Mycenaean Greek). If the metric tracked
linguisticality, LB would lead. It does not, so it ranks **formulaicity / template-rigidity**
([Sproat 2010](https://aclanthology.org/J10-4006/) makes this argument; the LB control shows
it directly). No "LA is / isn't a language" claim follows from these numbers.

## Inventory (Heaps / Zipf)

| | tokens | types | hapax % | Heaps β | regime |
|---|---|---|---|---|---|
| LB | 35,181 | 992 | 39.9% | 0.404 | undersampled (natural-language band) |
| LA | 4,324 | 346 | 45.7% | 0.451 | undersampled (natural-language band) |
| CH (control) | 3,781 | 135 | 5.9% | 0.256 | near-saturated inventory |

LA's β sits in the natural-language band (0.4–0.6), but with 45.7% of types occurring once
the absolute entropies and the ≈373-unseen-type projection at 5× corpus are lower bounds, not
estimates. Restricting LA to its ~100 syllabographic core drops β to 0.236 (22% hapax): the
syllabary is near-closed, and the open tail is logographic — a useful decomposition in its
own right.

## Adjacency

Sign-adjacency networks, gain over a degree-preserving rewire null:

| | directional-asymmetry gain | modularity gain |
|---|---|---|
| LB | +0.235 | +0.075 |
| LA | +0.094 | +0.068 |
| CH (control) | +0.356 | +0.137 |

The Linear A co-occurrence graph (164 nodes, 668 edges) is hub-structured: the busiest signs
are syllabograms (*ku* deg 213, *a* 173, *da* 156, *ta* 152) plus the fraction logogram A707
(deg 153), which also has the highest betweenness (0.229) — it bridges otherwise separate
groups, consistent with its accounting role. The network's communities weakly track the
syllabogram/logogram/fraction split (NMI 0.113 vs 0.029 null, ~4× chance) — a candidate
structural signal, not a typology.

## Reproduce

```bash
/opt/homebrew/bin/python3.12 src/exp_entropy_linguisticality.py   # entropy / order signal
/opt/homebrew/bin/python3.12 src/expb1b_corpus_size_control.py    # matched-size bootstrap
/opt/homebrew/bin/python3.12 src/exp_heaps_inventory.py           # Heaps / Zipf
/opt/homebrew/bin/python3.12 src/exp_sign_network.py              # directional asymmetry / modularity
/opt/homebrew/bin/python3.12 src/expi_cooccurrence_network.py     # LA co-occurrence hubs
```
