# Linear A internal structure — topics, site variation, word boundaries

**Register: extends, descriptive — with two honest near-negatives.** Three model-free
reads of the Linear A corpus itself (sign sequences + tablet bags from `corpus.db`, no
classifier). They are exploratory: useful descriptions of administrative structure, but two
of the three are reported with explicit confounds rather than as claims, because LA has no
decipherment to validate against.

## 1. Topic model — unsupervised functional groupings

LDA over 412 LA tablets (313-sign vocabulary, 8 topics): **3 of 8 topics are
logogram-anchored** — i.e. the unsupervised decomposition spontaneously separates tablets
by *what commodity they record*, clustering commodity logograms + fractions away from the
syllabographic material. Global sign-type mix: 198 logogram / 97 syllabogram / 16 fraction.

This agrees with the [accounting structure](sign_structure.md) seen in the fraction
analysis (commodity-logogram + quantity), now from a fully unsupervised angle. Reported as a
**candidate** description, not a claim: there is no external topic GT for LA, so this is "the
corpus has commodity-functional structure a topic model can find," not a typology.

## 2. Site variation — distributional, NOT (yet) dialectal

Pairwise Jensen–Shannon divergence of LA sign distributions across 6 find-spots:

| | most similar | most divergent |
|---|---|---|
| pair | Haghia Triada ↔ Zakros (**0.33**) | Khania ↔ Palaikastro (**0.72**) |

(tablet counts: Haghia Triada 259, Khania 142, Phaistos 50, Zakros 49, Knossos 42,
Palaikastro 23.)

There *is* measurable site-to-site variation — but we explicitly **do not call it dialect**.
Two confounds are uncontrolled: (a) **sample size** — Palaikastro (n=23) has noisy
distributions that inflate its divergence to every other site; and (b) **administrative
function** — different scriptoria recorded different commodities, so distributional
distance conflates *what was written* with *how*. The honest statement is: LA sign usage
differs by site, by an amount that tracks sample size and likely function; isolating a
dialectal signal would need frequency-matched, function-controlled comparison. Reported as
**descriptive / inconclusive** for any linguistic claim.

## 3. Word segmentation — a marginal result, reported as such

Unsupervised word-boundary detection via Harris **branching entropy** over LA sign
sequences (435 docs, 1,247 gold word boundaries from `word_parity`, boundary rate 0.326):

| | F1 |
|---|---|
| branching-entropy (matched operating point) | 0.367 |
| random baseline @ the boundary rate | 0.326 |
| **honest gain over random** | **+0.042** |

A **near-negative**: only +4pp over chance. LA words are short (mean ≈3 signs), so there is
little intra-word predictability for branching entropy to exploit, and the per-sign estimate
is noisy on a small corpus. We report the marginal gain honestly rather than the
recall-inflated threshold-sweep F1 (0.49, recall 0.96 — not a real operating point).
Consistent with LA carrying the least sequential structure of the three scripts
([distributional](distributional_structure.md)).

## Why these are here

The [rigor gate](../METHODOLOGY.md) is not only for the headline wins — it is why the two
weak results above ship with their confounds named (site variation) and their honest gain
quoted (segmentation) instead of being dropped or dressed up. All three are model-free, so
the choice of classifier weights does not enter.

## Reproduce

```bash
/opt/homebrew/bin/python3.12 src/exp_lda_topics.py              # -> data/exp_lda_topics.json
/opt/homebrew/bin/python3.12 src/expb3_b5_dialectology.py       # -> data/expb3_b5_dialectology.json
/opt/homebrew/bin/python3.12 src/exp_branching_entropy_seg.py   # -> data/exp_branching_entropy_seg.json
```
