# Sign structure — accounting syntax, attested words, and an allograph dead-end

**Register: extends + matches, with an honest dead-end.** Three model-free reads of *what
kind of sign goes where* in Linear A. Two recover structure that agrees with what
epigraphers already know (so they validate the method); one is reported as **inconclusive**
because a frequency confound eats the signal. All read sign sequences from `corpus.db` — no
classifier, so leakage cannot apply.

## 1. The accounting syntax: fractions bind to commodity logograms

Linear A fraction signs (250 tokens, 101 documents, 15 distinct signs; `A707` dominant at
104) attach overwhelmingly to **logograms, not syllabograms**:

| left neighbour of a fraction | count |
|---|---|
| logogram | **154** |
| syllabogram | 76 |
| document start | 17 |
| none | 3 |

And the logograms that take fractions are exactly the **commodities**: `GRA` (grain),
`OLIV` (olives), `VIN`/`FIC` (wine/figs), `A303`, `A308`. This is the administrative grammar
of the tablets recovered distributionally — *commodity-logogram + quantity-fraction* — and
it matches how the LA accounting system is understood. A clean "the data shows the known
structure" result.

## 2. Bigrams recover attested LA words

The top LA sign bigrams aren't noise — they are **known administrative terms**:

| bigram | count | what it is |
|---|---|---|
| **ku-ro** | 37 | the attested LA word *kuro* = **"total"** |
| ki-ro | 15 | *kiro* = "deficit / owed" |
| A707-A704 | 24 | a fraction sequence (the accounting tail) |
| sa-ra2 | 18 | *sara₂*, a recurring LA term |

Recovering *kuro* ("total") as the single most frequent bigram, purely from co-occurrence,
is the distributional method agreeing with decipherment scholarship — the same kind of
external check that anchors the [cross-script](cross_script_cognates.md) and
[scribal-hand](scribal_hands.md) findings.

## 3. The syllabary is near-closed; the logographic tail is open

From the [inventory analysis](distributional_structure.md): restricting LA to its ~100
**syllabographic** core gives Heaps β = 0.236 (22% hapax) — a near-saturated, well-sampled
inventory — whereas whole-corpus LA is β = 0.45 (46% hapax). The openness of LA's type
inventory lives almost entirely in the **logograms** (commodity signs, ligatures,
one-offs), not the syllabary. A useful decomposition: the phonetic core is essentially
known; the long tail is administrative logograms.

## 4. Distributional allograph detection — INCONCLUSIVE (the dead-end)

We tried to find **allographs** (variant shapes of the same sign) by flagging sign pairs
with near-identical contextual distributions (low Jensen–Shannon divergence): 89 signs,
1,792 same-type pairs, candidates like `AB06`/`AB77` (JSD 0.234).

**It does not survive its own control.** The correlation between a pair's JSD and the
signs' minimum frequency is **−0.76**: low-JSD ("looks like an allograph") tracks high
frequency, not genuine distributional identity. Frequent signs share generic contexts;
rare signs have noisy distributions. The "allograph" candidates are a **frequency artefact**,
so we report the method as inconclusive rather than publish the candidate list. (The fix
would be a frequency-matched null per pair — future work.) Reported here because a method
that fails its control is part of the honest record, same as
[Phaistos](NEGATIVE_phaistos.md).

## Reproduce

```bash
/opt/homebrew/bin/python3.12 src/expg_numeral_fraction.py        # -> data/expg_numeral_fraction.json
/opt/homebrew/bin/python3.12 src/exp5a_la_bigrams.py             # -> data/exp5a_la_bigrams.json
/opt/homebrew/bin/python3.12 src/exp_distributional_allograph.py # -> data/exp_distributional_allograph.json (inconclusive)
```
