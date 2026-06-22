# Sign function — logogram vs. syllabogram from distribution alone

Linear A signs fall into functional classes — syllabograms (phonetic) and logograms
(word/commodity signs). This asks whether that distinction is recoverable from **how a sign
is used** in the corpus, without any glyph-shape or phonetic information.

## Setup

- **Labels:** 169 Linear A signs with a catalogued function (98 logograms/ideograms, 71
  syllabograms).
- **Features (distributional, no image, no classifier):** corpus frequency, fraction of
  documents a sign appears in, isolation rate (how often it stands alone), positional
  statistics, and adjacency to numerals.
- **Model:** logistic regression, leave-one-out cross-validation.

## Result

| metric | value |
|---|---|
| majority-class baseline | 58.0% (98 logograms / 169) |
| LOO accuracy | **88.2%** (95% CI [82.4, 92.2], n = 169) |
| ROC-AUC | 0.95 |
| macro F1 | 0.88 |
| syllabogram precision / recall | 0.83 / 0.90 |
| logogram precision / recall | 0.92 / 0.87 |

Functional class is strongly predictable from usage alone — logograms and syllabograms
occupy distinct distributional niches (logograms are rarer per token, more often isolated,
and take numerals/fractions; syllabograms recur and combine). This is consistent with the
[fraction-attachment and bigram structure](sign_structure.md) seen separately: the same
administrative grammar that binds fractions to commodity logograms also separates the two
classes distributionally.

The classifier additionally assigns a predicted class to 45 currently unlabelled signs;
these are offered as candidates, not labels.

## Limits

- Distributional features partly encode frequency, and the two classes differ in frequency,
  so some of the signal is frequency itself — expected, not a confound to hide, but it means
  this measures "logograms and syllabograms are used differently," not a shape-based reading.
- 169 labelled signs; the CI is the honest spread. The null is the 58.0% majority-class
  rate (always predict "logogram"); 88.2% with a CI lower bound of 82.4% sits well clear of
  it, and ROC-AUC 0.95 is threshold-independent — so the result is not a base-rate artefact.
- Model-free: uses corpus statistics only, so the published-weights distinction does not
  apply here.

## Reproduce

```bash
python3 src/exp2_logo_vs_syllab.py   # -> data/exp2_logo_vs_syllab.json
```
