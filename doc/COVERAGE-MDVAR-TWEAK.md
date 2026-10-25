# Coverage tweak: ITWOM variability mode 12 to 2

Status: documented, code change NOT yet committed. Do not merge. Needs the exact line (see below).

## What it is

On Box B (aatuti-apps-01) a local copy of Signal-Server, `/root/a5-mdvar/src-patched`, carried one
Box B-only edit in `src/models/itwom3.0.cc`:

    propv.mdvar = 12;   ->   propv.mdvar = 2;

`mdvar` is the Longley-Rice / ITWOM variability mode passed to `qlrpfl` / `qlrpfl2`. Mode 12 is the
value hard-coded in several entry points. Mode 2 selects the broadcast-style single-message
variability setting instead of 12. Which one is right depends on the coverage study, so this is
a model-assumption change, not a bug fix.

## Why

It was used for the Panguna coverage runs on 1 to 2 Aug 2026 (a5-mdvar test of how the variability
mode shifts predicted coverage). The finals from those runs are in
`/opt/te/backups/coverage-finals-2026-10-25.tar.gz` on Box B (sha256 in the file beside it).
Reason for the test: compare coverage under mdvar 2 against the stock mode 12.

## Where `propv.mdvar = 12;` exists in this base (1193a22)

1. `point_to_point_ITM` (around line 2492)
2. `point_to_pointMDH_two` (around line 2744)
3. `point_to_pointDH` (around line 2839)

`point_to_point` (the main path in `los.cc`) takes `mode_var` from the caller and is not hard-coded.

## Open

The record kept when the Box B copy was deleted (25 Oct 2026) says "one line" but not which of the
three. Apply the change to the right one before using this branch. The Dell session that diffed
`a5-mdvar/src-patched` against the fix branch has the answer. A Binary Lane backup image of
2026-10-24 19:08Z on Box B still holds the original tree if the diff is needed again.

## Base

Branched from `fix/threading-race-use-after-free` (1193a22), which is what the matching
`fix-build/fix-plain` tree was identical to. Not merged, no PR.
