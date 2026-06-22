# Linear A internal structure — topics, site variation, word boundaries

Three model-free reads of the Linear A corpus (sign sequences and tablet bags from
`corpus.db`, no classifier). These are exploratory descriptions of administrative structure;
two of the three carry explicit confounds and are reported as descriptive rather than as
claims, since LA has no decipherment to validate against.

## 1. Topic model — unsupervised functional groupings

LDA over 412 LA tablets (313-sign vocabulary, 8 topics): 3 of 8 topics are logogram-anchored
— the decomposition separates tablets by what commodity they record, clustering commodity
logograms and fractions away from the syllabographic material (global sign-type mix: 198
logogram / 97 syllabogram / 16 fraction). The same LDA over the larger Linear B corpus
(5,167 tablets, 10 topics) likewise produces commodity-anchored topics (e.g. a sheep-
accounting topic led by `ovis:m` / `ovis:f`).

This agrees with the [accounting structure](sign_structure.md) seen in the fraction analysis
(commodity-logogram + quantity), from an unsupervised angle. Reported as a description, not a
typology: there is no external topic ground truth, and on short tablets the syllabogram
topics are largely frequency-driven.

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

Only +4pp over chance. LA words are short (mean ≈3 signs), so there is little intra-word
predictability for branching entropy to exploit, and the per-sign estimate is noisy on a
small corpus. The marginal gain is reported rather than the recall-inflated threshold-sweep
F1 (0.49 at recall 0.96, not a real operating point). Consistent with LA's lower sequential
structure ([corpus structure](distributional_structure.md)).

## Reproduce

```bash
/opt/homebrew/bin/python3.12 src/exp_lda_topics.py              # LA topics
/opt/homebrew/bin/python3.12 src/expc3_topic_model.py           # LB + LA topics
/opt/homebrew/bin/python3.12 src/expb3_b5_dialectology.py       # site variation
/opt/homebrew/bin/python3.12 src/exp_branching_entropy_seg.py   # word segmentation
```
