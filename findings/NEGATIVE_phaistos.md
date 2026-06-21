# NEGATIVE — the Phaistos placement that didn't hold

**This is the most important page in the repo.** It is a result we *retracted*, kept
here in full, because it shows the exact failure mode the [rigor gate](../METHODOLOGY.md)
exists to catch. A method you can only trust is one whose failures you can see.

## The claim (retracted)

The Phaistos Disc (45 signs, 241 tokens, a script isolate) was embedded with the sign
classifier and its signs measured for proximity to Linear A in embedding space. An early
run (128-d embeddings) reported a **mean nearest-neighbour cosine of 0.19** to Linear A
and concluded the Phaistos Disc sits **disjoint from Linear A** — a tidy, publishable-
sounding "the model sees that Phaistos is a true isolate."

## Why we retracted it (2026-05-27)

Two independent checks killed it:

1. **It wasn't robust to the embedding.** Re-running on a later classifier's embeddings
   put the same Phaistos–LA proximity at **0.497** (mid-range), not 0.19. Two embeddings
   of the *same* signs disagreed wildly, so the number measured the *model*, not the
   script.
2. **The ordering was palaeographically impossible.** In those embeddings, **Cretan
   Hieroglyphic — a genuine sister Aegean script, ancestral/cousin to Linear A — sat
   *farther* from LA (0.274) than the Phaistos isolate did (0.497).** If the metric
   measured script relatedness, that ordering cannot happen. It exposed what the metric
   was *actually* tracking: **per-script rendering / drawing style** (stroke weight,
   medium, the hand that drew the reference glyphs), not linguistic or genealogical
   relationship. Two scripts drawn in the same style look close regardless of what their
   signs mean.

The metric reliably separates only the **unrelated floor** (cuneiform, 0.106) from
everything Aegean — it has no resolution for the finer "which Aegean script is closer to
which" question that the retracted claim depended on.

## What a valid test would need

The same discipline that makes the [cross-script LA↔LB result](cross_script_cognates.md)
trustworthy: **published ground-truth correspondence pairs, scored by P@K** — not a bare
nearest-neighbour proximity scan. LA↔LB has 64 secure GT correspondences, so P@1/P@5 is
immune to the rendering-style confound. The Phaistos Disc has **no such correspondence
GT** (it's undeciphered and isolate), so there is nothing to anchor against. The honest
status of "where does Phaistos sit relative to LA" is therefore **INCONCLUSIVE** — and
must stay inconclusive until anchoring GT exists, which for a script isolate it may never.

## The rule this produced

> **Never report a cross-script relatedness claim from raw embedding proximity. Anchor to
> GT correspondences with P@K, or don't claim it.**

This is now [the first line of the rigor gate](../METHODOLOGY.md). Every "the model sees
X" page in this repo clears it; this page is the one that didn't, kept as the reason the
others can be believed.
