# Planned: a character-based sign phylogeny (CH → LA → LB → Cypriot)

Status: **design only — not yet run.** This is the principled replacement for cross-script
*proximity* placement. The retracted Phaistos attempt (see [`METHODOLOGY.md`](METHODOLOGY.md))
failed because raw embedding distance between scripts is confounded by drawing style. A
phylogeny built from **explicit structural characters** is falsifiable and can be controlled
for that confound, so it is the defensible way to ask how the Aegean scripts relate by sign
form.

## Question

Do the Bronze Age Aegean scripts — Cretan Hieroglyphic, Linear A, Linear B, Cypro-Minoan /
Cypriot — relate by sign *form* in a way that recovers known descent, and can the model
propose novel sign correspondences with explicit support values?

## Method

1. **Character matrix.** Code each sign as a binary vector of structural features
   (stroke counts, junctions, closed loops, symmetry axes, dominant orientation, presence of
   specific sub-elements), drawn from GORILA / CHIC / SigLA facsimiles. Characters are
   defined explicitly and published, so anyone can re-code or dispute them — unlike an opaque
   embedding distance.
2. **Tree inference.** Maximum parsimony plus a Bayesian Mk model (Lewis 2001) over the
   matrix → tree with branch support (bootstrap / posterior).
3. **Anchoring.** The 11 secure LA↔LB correspondences are the validation set: they must
   emerge as supported sisters before any novel claim is read off the tree.

## Validation and controls

- **Pass condition:** the secure LA↔LB pairs recover as well-supported sisters. If they do
  not, the character coding is inadequate and nothing downstream is reported.
- **Homoplasy control:** down-weight common, easy strokes (a single vertical line recurs
  everywhere; consistency index ≈ 0.1). Report per-character CI and exclude or down-weight
  low-CI characters.
- **"Medium tree" control:** verify the tree groups by descent, not by writing medium or by
  the modern editor who drew the facsimile (a known confound — GORILA/CHIC drawings are
  editorial). Code from multiple facsimile sources and check the topology is stable.
- **Candidate vs confirmed:** novel correspondences are reported only with support values
  and marked candidate, never as established.

## Inputs (mostly in hand)

- Facsimiles: GORILA (LA), CHIC (CH), SigLA catalogue, LB sign tables — already used
  elsewhere in the project.
- Anchor set: the 54 published LA↔LB correspondences (11 secure), in `corpus.db`.
- Tooling: standard phylogenetics (parsimony / Mk); the character coding is the new work.

## Effort and risk

Large; high-risk. The character coding is laborious and subjective, and the homoplasy and
medium confounds are real. It is listed here as the next substantial research piece, not a
quick result — the smaller findings in `findings/` should be read as the validated work, and
this as a proposal.
