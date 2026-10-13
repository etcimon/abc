# Data and cost

## `Abc_Ntk_t`

Each object is a heap node. Fanin is a small array. Fanout is a linked list.
Walking fanout is O(fanout). Looking up a node by name is a hash
(`src/misc/st`) when the network built it, and a scan when it did not.
Structural hashing of AND nodes is a hash of the two fanins, O(1) expected.

Truth tables for 6-input functions are `unsigned` words (`src/opt/dau`,
`src/aig/aig/aigTruth.c`), not bit loops. A 6-input cone is one machine
word. Do not rewrite those as per-bit loops.

## `Gia_Man_t`

`Gia_Obj_t` is a struct in one array. The id is the index. Fanin0 and
fanin1 are integer ids. Complement is the low bit of the id, the same idea
as OpenROAD's pin tag. Dereferencing a fanin is O(1). There is no pointer
chase through a fanout list unless fanout has been built.

Memory for the manager is a custom allocator (`src/misc/mem`), not one
`malloc` per node. Replacing it with `std::vector` of pointers would be
slower and would change the id arithmetic.

## Cuts

`src/opt/cut` and `src/aig/gia` cuts are sets of leaves packed in integers.
Enumerating cuts is exponential in the cut size. The cut size parameter is
the cost knob. Raising it changes the mapping, not just the runtime.

## Timing inside ABC

`src/aig/aig/aigTiming.c` and `src/map/scl` carry unit or library delays on
ABC nodes. Those delays are not OpenSTA arrivals. Do not mix the two clocks.
