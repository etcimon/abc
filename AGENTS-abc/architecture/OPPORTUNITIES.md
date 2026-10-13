# Opportunities

No ABC function showed up in the TinyRocket `repair_timing` callgrind. Do
not tune ABC to speed that benchmark.

If a later profile is a resynthesis or clock-gating run:

- Stay on `Gia_Man_t` for the sweep. Converting to `Abc_Ntk_t` allocates an
  object per node and builds fanout lists. That conversion is the usual
  cliff.
- Truth-table operations are word-wide. A per-bit rewrite is the Verilator
  mistake of shift-and-mask on a value that already fits in a register.
- Structural hash lookups are already O(1) expected. A second hash of the
  same fanin pair duplicates `opt_merge`'s old extra table. Do not add one.
- SAT proofs (`src/sat/kissat`, `src/sat/cadical`, `src/proof/pdr`) are
  their own solvers. Wrapping them in another C++ container does not make
  the SAT search shorter.
- Equivalence and fraiging change the network. They are correct only if the
  miter is unsat. A faster sweep that skips the miter is a different circuit.

OpenROAD should pass networks through the MiniAIG or MiniLUT frame APIs
when it only needs a GIA. Building an `Abc_Ntk_t` to call one `&` command
is the expensive path.
