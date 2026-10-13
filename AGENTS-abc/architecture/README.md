# ABC architecture notes

ABC is the logic network library. OpenROAD links it for cut enumeration,
clock gating, and restructuring (`src/cut`, `src/cgt`, `src/rmp`). A timing
report does not enter ABC. A callgrind of `repair_timing` that shows no ABC
frames is expected.

| Note | What it answers |
|---|---|
| [EXECUTION.md](EXECUTION.md) | Which network a command mutates |
| [DATA.md](DATA.md) | `Abc_Ntk_t` versus `Gia_Man_t` |
| [OPPORTUNITIES.md](OPPORTUNITIES.md) | What is O(1) per node, and what is not |

Fork: `https://github.com/etcimon/abc`, branch `g6lc`. No algorithm edit
on that branch yet.
