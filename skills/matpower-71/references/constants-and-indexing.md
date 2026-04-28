# Constants and Indexing

MATPOWER matrices are column-oriented. Avoid hard-coded column numbers in generated code.

## Preferred Pattern

```matlab
define_constants;
mpc = loadcase('case30');
mpc.bus(2, PD) = 30;
mpc.branch(:, RATE_A) = 100;
```

`define_constants` defines names from helper functions including `idx_bus`, `idx_gen`, `idx_brch`, and `idx_cost`.

## Direct Helper Pattern

Use this inside functions when avoiding workspace-wide constant injection:

```matlab
[PQ, PV, REF, NONE, BUS_I, BUS_TYPE, PD, QD, GS, BS, BUS_AREA, VM, VA] = idx_bus;
[GEN_BUS, PG, QG, QMAX, QMIN, VG, MBASE, GEN_STATUS, PMAX, PMIN] = idx_gen;
```

## Internal vs External Numbering

MATPOWER converts cases to internal indexing for simulation:

- Isolated buses, out-of-service generators, and out-of-service branches may be removed.
- Bus numbering becomes consecutive internally.
- Original data and ordering metadata are stored in `mpc.order`.
- Final results are converted back to external numbering before being returned by top-level functions such as `runpf` and `runopf`.

When modifying internals or writing callbacks, confirm whether data is in external or internal ordering.

## Common Mistakes

- Treating bus numbers as row indices after `ext2int` or before `int2ext`.
- Hard-coding `bus(:, 3)` instead of `bus(:, PD)`.
- Reading result columns before a PF/OPF has run.
- Forgetting that inactive equipment may not exist in internal-order data.
