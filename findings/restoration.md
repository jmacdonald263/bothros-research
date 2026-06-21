# Sign restoration — predicting a missing sign from context

**Register: extends — with an honest LA floor.** Mask one sign in a sequence and predict
it from its neighbours. This is the task that matters for damaged tablets, and it asks a
sharper question than classification: *how much does sign context constrain what comes
next?* The answer is "a lot for Linear B, very little for Linear A" — and that gap is
itself a finding.

## Setup

- **Task:** held-out masked-token prediction over sign sequences (`corpus.db`), train/test
  split. Metric: top-K accuracy + mean rank of the true sign.
- **Three models, classical → neural:** back-off **n-grams** (order 2/3/4) and a small
  **3-layer Transformer** (masked-LM, d=128). The n-gram order-2 doubles as the simple
  baseline the fancier models must beat.
- **Null:** LB vocabulary is 1,264 signs, so random top-1 ≈ 0.08% and random mean-rank ≈ 632.

## Result — Linear B (14,554 test positions, vocab 1,264)

| model | top-1 | top-5 | top-10 | mean rank | perplexity |
|---|---|---|---|---|---|
| n-gram (2) — baseline | 14.8% | 34.5% | 45.2% | 7.6 | 75.1 |
| n-gram (3) | 21.3% | 41.9% | 52.0% | **6.9** | 82.2 |
| n-gram (4) | 24.6% | 44.0% | 53.3% | — | 101.2 |
| **Transformer (3-layer)** | **25.5%** | **45.2%** | **55.7%** | 46.5 | — |

Context predicts the missing LB sign at **~25% top-1 / ~45% top-5** — against a 0.08%
random floor, mean rank ~7 out of 1,264. The structure is strongly there.

**The neural model barely beats the 4-gram (25.5% vs 24.6% top-1), and is worse in the
tail.** Its mean rank (46.5) is far worse than the trigram's (6.9): the Transformer wins at
the very top but, when wrong, is *very* wrong, while the n-gram backs off gracefully. At
this corpus size (≈59k training tokens) the deep model has no real edge — the honest
reading is that **classical n-grams are competitive**, and we say so rather than headline
the Transformer.

## Result — Linear A (836 test positions, vocab 337)

| model | top-1 | top-5 | top-10 |
|---|---|---|---|
| Transformer (3-layer) | **4.4%** | 16.6% | 27.6% |

**LA restoration is near the floor.** 4.4% top-1 is above chance (0.3%) but far below LB's
25% — the LA corpus is an order of magnitude smaller and its sequences are looser
(consistent with the [distributional result](distributional_structure.md): LA carries the
least sequential structure of the three scripts). This is reported as an **honest negative
for LA**: contextual restoration is not yet a useful tool there, and more LA data is the
only fix.

## Companion: contextual parallels (qualitative)

`exp3_contextual_parallels.py` retrieves the most *similar whole tablets* by mean sign
embedding (e.g. Kn1516 → Kn607 at cosine 0.86) — a "find tablets like this one" aid for
spotting formulaic parallels. Useful for an epigrapher, but qualitative (no GT to score
against), so it is reported as a tool, not a measured claim.

## Reproduce

```bash
/opt/homebrew/bin/python3.12 src/exp1_ngram_restoration.py      # -> data/exp1_ngram_restoration.json
PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.6 PYTORCH_MPS_LOW_WATERMARK_RATIO=0.4 \
  /opt/homebrew/bin/python3.12 src/expa2_transformer_restoration.py  # -> data/expa2_transformer_restoration.json
/opt/homebrew/bin/python3.12 src/exp3_contextual_parallels.py   # -> data/exp3_contextual_parallels.json
```
