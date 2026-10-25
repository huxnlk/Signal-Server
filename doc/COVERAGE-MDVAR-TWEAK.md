# Coverage tweak: ITWOM variability mode 12 to 2

Status: documented, code change NOT yet committed. Do not merge. Needs the exact line (see below).

## What it is

On Box B (aatuti-apps-01) a local copy of Signal-Server, `/root/a5-mdvar/src-patched`, carried one
Box B-only edit in `src/models/itwom3.0.cc`:

    propv.mdvar = 12;   ->   propv.mdvar = 2;

`mdvar` is the Longley-Rice / ITWOM variability mode passed to `qlrpfl` / `qlrpfl2`. In the ITM
convention the tens digit adds options (10 and up drop location variability) and the units digit
picks the reliability mode (2 is mobile). So 12 and 2 differ in whether location variability is
included. This is a model-assumption change, not a bug fix. Check the ITWOM source comments before
relying on this summary.

## Why

The Box B copy was named `a5-mdvar` and last touched 1 to 3 Aug 2026, alongside the Panguna coverage
runs. Finals from those runs are in `/opt/te/backups/coverage-finals-2026-10-25.tar.gz` on Box B
(sha256 in the file beside it). The stated purpose is not recorded; it looks like a test of how
the variability mode shifts predicted coverage. Confirm with whoever ran it.

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
