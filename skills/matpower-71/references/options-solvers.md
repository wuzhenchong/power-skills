# Options and Solvers

Use `mpoption` for MATPOWER options. Do not mutate undocumented option internals unless the task requires source-level debugging.

## Common Options

```matlab
mpopt = mpoption('verbose', 0, 'out.all', 0);
mpopt = mpoption(mpopt, 'pf.enforce_q_lims', 1);
mpopt = mpoption(mpopt, 'opf.ac.solver', 'MIPS');
```

## Output Control

- `verbose`: controls progress text.
- `out.all`: controls pretty-printed output.

Use quiet options in tests and scripts that should produce deterministic logs.

## Solver Guidance

- MIPS is bundled and is the safest default for many OPF examples.
- Optional solvers may be faster or more robust but may not be installed.
- When generating reusable code, allow the caller to pass `mpopt`.
- When debugging, print or inspect `mpopt` rather than assuming defaults.

## Reproducibility

Record non-default solver choices, tolerances, and output settings in scripts. For research workflows, save both input case and solved results.
