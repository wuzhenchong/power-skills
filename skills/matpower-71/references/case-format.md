# Case Format

MATPOWER 7.1 case files are MATLAB `.m` files or `.mat` files that define or return an `mpc` struct.

## Required Fields

- `mpc.version`: normally `'2'` for version 2 case format.
- `mpc.baseMVA`: system MVA base.
- `mpc.bus`: one row per bus.
- `mpc.gen`: one row per generator.
- `mpc.branch`: one row per branch.

## Common Optional Fields

- `mpc.gencost`: generator cost data for OPF.
- `mpc.bus_name`: bus names.
- `mpc.gentype`: generator type strings.
- `mpc.genfuel`: generator fuel strings.
- OPF extension fields: `A`, `l`, `u`, `H`, `Cw`, `N`, `fparm`, `z0`, `zl`, `zu`.
- User-defined fields may be loaded by `loadcase`; saving them may require a `savecase` callback.

## Minimal Case Pattern

```matlab
function mpc = case_custom
mpc.version = '2';
mpc.baseMVA = 100;

mpc.bus = [
    1  3  0   0   0 0 1 1.00 0 230 1 1.05 0.95;
    2  1  50  30  0 0 1 1.00 0 230 1 1.05 0.95;
];

mpc.gen = [
    1  80  0  100 -100 1.00 100 1 200 0;
];

mpc.branch = [
    1 2 0.01 0.05 0 100 100 100 0 0 1 -360 360;
];

mpc.gencost = [
    2 0 0 3 0.01 20 0;
];
```

## Practical Guidance

- Use `loadcase` instead of directly calling case files when accepting user input.
- Use `savecase` when writing MATPOWER-format outputs.
- Use named constants for column access.
- Be explicit about units: loads and generations are MW/MVAr; voltages are p.u.; angles are degrees.
- After running PF/OPF, MATPOWER appends result columns to `bus`, `gen`, and `branch`; do not assume solved matrices have only input columns.
