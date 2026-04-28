# Optimal Power Flow

Use `runopf` for AC OPF and `rundcopf` for DC OPF.

## AC OPF

```matlab
define_constants;
mpc = loadcase('case30');
mpopt = mpoption('verbose', 0, 'out.all', 0);
[results, success] = runopf(mpc, mpopt);
assert(success == 1);
fprintf('Objective = %.6f\n', results.f);
```

## DC OPF

```matlab
results = rundcopf('case118', mpoption('verbose', 0, 'out.all', 0));
```

Use DC OPF for linearized active-power dispatch and congestion studies. Use AC OPF when reactive power, voltages, losses, or AC feasibility matter.

## Generator Cost

`mpc.gencost` defines OPF cost curves. Common models:

- Polynomial costs with `MODEL = 2`.
- Piecewise-linear costs with `MODEL = 1`.

Use `idx_cost` or `define_constants` for cost columns.

## Common Result Fields

- `results.f`: final objective value.
- `results.success`: success flag.
- `results.bus`, `results.gen`, `results.branch`: solved system data and result columns.
- `results.om`: OPF model object in workflows that expose it.
- `results.raw`: lower-level solver output in some configurations.

## Practical Defaults

```matlab
mpopt = mpoption( ...
    'verbose', 0, ...
    'out.all', 0, ...
    'opf.ac.solver', 'MIPS');
```

When solver availability is unknown, avoid assuming optional commercial solvers. MIPS is bundled with MATPOWER.
