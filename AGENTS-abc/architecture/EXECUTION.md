# Execution path

The standalone binary is `src/base/main`. `Abc_Start` creates the frame
(`Abc_Frame_t`). `Cmd_CommandExecute` runs one command string. OpenROAD's
tests call that pair (`src/cut/test/cpp/TestAbc.cc`).

Two network lifetimes matter:

1. `Abc_Ntk_t` (`src/base/abc`, objects in `src/misc` and `src/opt/nwk`).
   Nodes are `Abc_Obj_t` with fanin and fanout lists. Commands in
   `src/base/abci` (`Abc_Command*`) operate on this network. Structural
   hashing and rewriting live under `src/aig` and `src/opt/rwr`.
2. `Gia_Man_t` (`src/aig/gia`). Nodes are integer ids in a compact array.
   `&` commands in the GIA package are the ones that stay fast on large
   netlists. MiniAIG and MiniLUT bridges (`Abc_FrameGiaInputMiniAig`,
   `Abc_FrameGiaOutputMiniLut`) are how an outside tool passes a network
   without building a full `Abc_Ntk_t`.

Mapping to gates is `src/map` (`mapper`, `if`, `mio` for genlib, `scl` for
standard-cell timing inside ABC). SAT and equivalence are `src/sat` and
`src/proof`. They are not on the OpenROAD STA path.

Clock gating in OpenROAD (`src/cgt`) exports an `Abc_Ntk_t`, runs a
combinational check, and imports the result. That is a logic pass. It does
not update STA arrivals.
