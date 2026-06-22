# Planned: a sign-level phylogeny of the Aegean scripts (CH · LA · LB · Cypro-Minoan)

Status: **design only — not yet run.** This is the principled replacement for cross-script
*proximity* placement (the retracted Phaistos attempt failed because raw embedding distance
between scripts is confounded by drawing style; see [`METHODOLOGY.md`](METHODOLOGY.md)). A
phylogeny from explicit, falsifiable characters can be controlled for that confound. The
design below reflects the prior-art and method review.

## Novelty — framed precisely (a reviewer will know the adjacent work)

The novelty is in the **unit of analysis**. Sorting prior work by what the tree's tips are:

- **Whole scripts as tips** (Aegean family trees from cross-script sign similarity) —
  **already exists** (Revesz 2016/17; Daggumati & Revesz 2018–23), and is regarded as fringe
  by mainstream Aegeanists (UPGMA clock, DNA-encoding of glyphs). **Do not claim "first tree
  of the Aegean script family"** — and do not inherit their conclusions; cite only as the
  pipeline to avoid.
- **Scribal hands as tips** (within Linear B), characters = sign-form variants —
  **exists, and is the legitimacy anchor**: Skelton 2008 (*Archaeometry*), maximum parsimony,
  passed a known-answer test, reused as ground truth by Srivatsan et al. 2021. We extend its
  design from scribes-as-taxa to **signs-as-taxa**.
- **Individual signs as tips**, characters = shape features — **no instance found, any
  script** (only a framework proposed: Pardede/Hosszú/Kovács 2026).

**Defensible novel claims:** (a) first phylogeny whose tips are individual Aegean *signs*;
(b) first sign-form tree to include **Cypro-Minoan** as a taxon; (c) first grounded in a
learned classifier embedding rather than only hand-coded features. **Indefensible:** "first
Aegean script tree."

## Method (network-first, anchored, validated)

1. **Character coding — anchor to catalogue sign IDENTITY, not raw shape** (Skelton's design;
   our defence against allograph / scribe / editor noise). Characters = structural features
   (stroke count, junctions, closed loops, symmetry, orientation, sub-elements). Mark
   inapplicable/contingent characters explicitly (Brazeau 2019; Tarasov 2023 — naïve "?"
   coding makes artefacts); avoid correlated duplicate characters; unordered (Fitch) by
   default, ordered only for genuine counts (e.g. number of strokes). A learned-embedding
   feature set can be a *second* character source, cross-checked against the hand-coded one.
2. **Run a network FIRST.** Given documented inter-script borrowing + a small matrix + the
   medium confound, the honest primary object is a **NeighborNet (SplitsTree6)**; present a
   tree only if the data are genuinely tree-like (δ-score / Q-residuals near 0). Borrowing
   perturbs topology little (Greenhill–Currie–Gray 2009) but wrecks dates — so claim
   topology, never dates.
3. **Tree inference (if tree-like): Bayesian Mkv + Γ** (Lewis 2001, with the Mkv
   ascertainment-bias correction since only variable characters are coded) in **MrBayes** —
   the one tool giving true posterior support. Cross-check with **IQ-TREE** (ML, `MK+ASC`,
   ultrafast bootstrap) and **parsimony with implied weighting** (TNT or phangorn), sweeping
   the concavity constant. Report **consistency / retention indices** for homoplasy.

## Validation and controls (non-negotiable, in order)

1. **Known-answer first.** Recover the published LA↔LB correspondences (the **11 secure**
   pairs in `corpus.db` — the broader "54" set is the less-secure tier; treat the secure 11
   as the anchor) as sisters / ancestor-descendant *before* any novel claim. If they don't
   recover, the coding measures style, not descent — stop.
2. **Style/medium negative control.** This confound is *documented on these exact scripts*:
   Corazza 2022 found Cypro-Minoan clusters driven by writing *medium*, not sign identity;
   the facsimile analogue is editor drawing convention (GORILA vs CHIC vs SigLA house style).
   Build a parallel "nuisance" matrix coding only editor-source / substrate, and Mantel-test
   the sign tree against it. **If the tree correlates more with style than with known descent,
   that is the finding** — reported, not hidden.
3. **Tree-likeness** (δ-score, Q-residuals) before committing to a tree.
4. **Support ≠ accuracy.** Watch long-branch attraction (acute at few taxa × few characters);
   don't interpret weak nodes; run a **permutation null** (real tree must beat permuted-state
   noise).

## Tools (all local, no GPU)

MrBayes (primary, posterior support) · SplitsTree6 (NeighborNet, run first) · IQ-TREE (ML
cross-check) · phangorn/R (open parsimony). **Not** BEAST2 (its clock/dating stack is
overkill) and **not** DendroPy/Bio.Phylo for inference (they only read/manipulate trees).

## Inputs

Facsimiles GORILA (LA) / CHIC (CH) / SigLA / LB sign tables; Cypro-Minoan sign list; anchor =
11 secure LA↔LB correspondences (`corpus.db`). The character coding is the new work.

## Honest verdict

**Worth doing, but only as a known-answer-validated, network-first exercise with the
style/medium negative control.** A raw-shape tree across mixed facsimile sources is likely
confounded — Skelton shows the validated version is not hopeless; Revesz shows the
unvalidated version to avoid. Large effort, real risk; this is the next substantial piece,
not a quick result.

## Suggested first step — the pilot (data path now scoped)

Gate the whole project on a ~22-sign pilot (the 11 secure LA↔LB pairs) before scaling:

1. **Glyphs from a common Unicode font, not facsimiles.** Render the LA and LB members of
   each secure pair from a single Aegean font (Noto Sans Linear A + Linear B, or Douros
   Aegean) at identical size/weight. This removes the editor/medium confound *by
   construction* — the cleanest possible input, and the honest baseline before touching
   style-varied facsimiles. (Map: secure AB-code → Linear A Unicode; reading → Linear B
   syllabogram Unicode. SigLA `attestations.json` provides AB-code→crop for a facsimile
   cross-check; Bennett images are unlabelled page fragments, not per-sign usable.)
2. **Skeleton-free structural characters** (skimage is PEP-668-blocked; cv2+scipy suffice):
   Hu moments, hole count (scipy.ndimage Euler number), aspect/extent/solidity, vertical &
   horizontal symmetry, projection profiles. (Add a Zhang–Suen thinning in numpy later if
   stroke/junction counts are wanted.)
3. **Gate, in order:** (a) **known-answer** — are the secure LA↔LB pairs closer in character
   space than random LA–LB pairs? (b) **style control** — do the signs separate by *script*
   (LA vs LB) rather than pair up by correspondence? If they separate by script, the
   characters encode rendering, not identity → confounded → report and stop. Only if the
   pilot passes both is the full-inventory build (NeighborNet + Mkv) worth it.

A common-font pilot that *still* separates by script would be a clean negative (shape alone
doesn't carry the correspondence); one where secure pairs are closest is the green light.

## Pilot result (run 2026-06-22, `src/exp_phylo_pilot.py`)

Ran the gate on the 11 secure pairs using **our real crops** (LA = SigLA facsimiles by
AB-code; LB = `lb_labeller_train_v3` by reading) + skeleton-free explicit characters (holes,
aspect, extent, solidity, symmetry, Hu moments).

- **Gate 1 (known-answer): weak.** Secure-partner P@1 = 18% (2/11) vs 9% chance; mean partner
  rank 4.7/11. The secure LB partner is not reliably the nearest LB sign in character space.
- **Gate 2 (style control): fires.** Intra-script distance (4.93) < inter-script (5.46) — the
  signs cluster by *script*; within-pair (5.12) barely below cross-pair (5.50).

**Verdict: the naive version does not clear the gate** — consistent with the predicted
medium/source confound (Corazza 2022) and the Phaistos lesson. **Two honest caveats on the
pilot itself**, so this is not a clean death sentence for the project: (a) the sources are
processed *differently* — LA is a clean alpha-channel facsimile silhouette, LB a noisy
photo-Otsu mask — and that asymmetry alone could drive the script split; (b) skeleton-free
characters are coarse and miss stroke topology (the research's recommended characters).

**Three variants run — the result is robust:**

| variant | P@1 (secure partner nearest) | mean partner rank | style control |
|---|---|---|---|
| LA SigLA-facsimile + LB linearb-**photo**, skeleton-free chars | 18% | 4.73/11 | script-separated |
| LA SigLA + LB linearb-**facsimile** (source-matched), skeleton-free | 18% | 3.91/11 | script-separated |
| source-matched + **skeleton** chars (endpoints/junctions/length) | 18% | 4.55/11 | script-separated |

(`lineara.xyz` + `linearb.xyz` both have photo *and* facsimile sets, all per-sign labelled —
the earlier "no labelled LB facsimile" note was wrong; the real issue was a source-*type*
mismatch, since fixed. LA syllabograms come from SigLA because the lineara.xyz facsimile
imagemap is GORILA-`*NNN`-coded and lacks the secure low syllabograms.)

**Verdict (robust): auto-extracted structural characters do not work.** P@1 is stuck at 18%
(2× chance, weak) and signs separate by script across *every* combination of source-type and
character-set. There is a faint real signal (within-pair < cross-pair distance — homomorphic
pairs are slightly closer) but it is dominated by script/editor style and never reaches
reliable matching. This is exactly what the method research predicted: the discriminative
work is in **expert, catalogue-anchored hand-coding of characters (Skelton's design)**, which
is a specialist, weeks-long task — *not* an automatable feature pipeline.

**Recommendation: do NOT scale to the full NeighborNet/Mkv tree.** The automatable routes
have been tried and gated out; a credible build now requires either expert palaeographic
character coding (major commitment, uncertain payoff) or a fundamentally better
style-invariant shape representation. The pilot did its job — it saved that effort and tells
us precisely why. (Scripts: `src/exp_phylo_pilot.py`.)

## Key references

Lewis 2001 (Mk/Mkv); Wright & Hillis 2014 (Bayesian Mk beats parsimony on noisy morphology);
Greenhill–Currie–Gray 2009 (borrowing preserves topology, not dates); Bryant & Moulton 2004
(NeighborNet); **Skelton 2008** (LB scribal-hand parsimony — the anchor); Revesz / Daggumati
& Revesz (script-as-taxon — the cautionary precedent); Corazza 2022 (CM medium confound);
Braović et al. 2024 (Aegean review; allograph/scribe challenges); Salgarella 2020 + SigLA
(LA↔LB homomorphy); Ferrara–Montecchi–Valério 2022 (CH↔LA: shared template, not direct
descent).
