# Sign restoration — predicting a missing sign from context

Mask one sign in a sequence and predict it from its neighbours — the task relevant to
damaged tablets, and a measure of how much sign context constrains what comes next. Context
constrains the missing sign substantially in Linear B and very little in Linear A.

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

Context predicts the missing LB sign at ~25% top-1 / ~45% top-5, against a 0.08% random
floor (mean rank ~7 of 1,264).

The Transformer barely beats the 4-gram (25.5% vs 24.6% top-1) and is worse in the tail: its
mean rank (46.5) is far higher than the trigram's (6.9) — it wins at the very top but, when
wrong, is wrong by a large margin, while the n-gram backs off gracefully. At this corpus size
(≈59k training tokens) the deep model has no real advantage; classical n-grams are
competitive.

## Result — Linear A (836 test positions, vocab 337)

| model | top-1 | top-5 | top-10 |
|---|---|---|---|
| Transformer (3-layer) | **4.4%** | 16.6% | 27.6% |

LA restoration is near the floor: 4.4% top-1 is above chance (0.3%) but far below LB's 25%.
The LA corpus is an order of magnitude smaller and its sequences are looser (consistent with
the [corpus-structure result](distributional_structure.md)). Contextual restoration is not a
useful tool for LA at the current corpus size.

## Does the LM help end-to-end? (near-null)

A trigram sign LM (built from the corpus, test perplexity 83) was used to rescore the LB
pipeline's top-5 predictions per box — sweeping the LM weight, position weight, and a
confidence gate. Best result: per-line accuracy 0.698 → **0.701 (+0.3pp)**; higher LM
weights make it worse. So the LM that restores masked signs at 25% in isolation does **not**
meaningfully improve the deployed pipeline: where the classifier is already right the LM
agrees, and where it is wrong the LM (perplexity 83) is not confident enough to override.
Reported as a near-null, closing a built-but-never-evaluated component.

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
