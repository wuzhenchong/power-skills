---
name: matpower-71
description: Guide MATLAB/Octave coding and source-level work with MATPOWER 7.1. Use when writing, reviewing, debugging, or modifying project code involving MATPOWER case files, mpc structs, bus/gen/branch/gencost matrices, define_constants, idx_* constants, loadcase/savecase, runpf/rundcpf, runcpf, runopf/rundcopf, mpoption, OPF extensions, userfcn callbacks, solver options, result interpretation, convergence debugging, or project-local copies of MATPOWER 7.1 source files.
---

# MATPOWER 7.1

Use this skill for MATLAB/Octave work that depends on MATPOWER 7.1. Prefer the MATPOWER 7.1 legacy APIs and behavior unless the user explicitly asks for a newer MATPOWER 8.x MP-Core workflow.

## Core Rules

1. Prefer named constants from `define_constants` or `idx_bus`, `idx_gen`, `idx_brch`, `idx_cost` over hard-coded matrix column numbers.
2. Load cases with `loadcase`; save cases with `savecase`; preserve MATPOWER case struct semantics.
3. Use `runpf` or `rundcpf` for power flow, `runcpf` for continuation power flow, and `runopf` or `rundcopf` for optimal power flow.
4. Use `mpoption` for solver, algorithm, tolerance, verbosity, and output configuration.
5. For custom OPF behavior, first check standard extension fields (`A`, `l`, `u`, `H`, `Cw`, `N`, `fparm`, `z0`, `zl`, `zu`) and `gencost`; use `userfcn` callbacks when staged customization is needed.
6. Do not edit the user's installed MATPOWER package in place. If MATPOWER source behavior must change, edit project-local copies of the relevant `.m` files and verify MATLAB/Octave resolves those copies first.

## Reference Routing

Load only the references needed for the task.

- Start with `references/quickstart.md` for ordinary MATPOWER programming patterns.
- Use `references/case-format.md` for `mpc` structs, `baseMVA`, `bus`, `gen`, `branch`, `gencost`, optional fields, and case construction.
- Use `references/constants-and-indexing.md` for `define_constants`, `idx_*` helpers, and internal vs external numbering.
- Use `references/power-flow.md` for AC/DC PF with `runpf`, `rundcpf`, Q-limit handling, and PF result fields.
- Use `references/continuation-power-flow.md` for CPF with `runcpf`.
- Use `references/opf.md` for AC/DC OPF, `gencost`, `runopf`, `rundcopf`, objectives, and common OPF results.
- Use `references/opf-extensions.md` for custom constraints, costs, variables, and OPF formulation extensions.
- Use `references/callbacks-userfcn.md` for `add_userfcn`, callback stages, and save/print/custom formulation hooks.
- Use `references/options-solvers.md` for `mpoption`, algorithms, solvers, output control, and reproducibility.
- Use `references/results-and-postprocessing.md` for interpreting solved cases and extracting flows, voltages, losses, costs, and success flags.
- Use `references/testing-debugging.md` for installation checks, regression cases, convergence debugging, and path issues.
- Use `references/source-architecture.md` before reading or changing MATPOWER internals.
- Use `references/source-modification-guide.md` before editing any project-local copy of a MATPOWER source file.
- Use `references/source-index.md` for official MATPOWER 7.1 documentation and source lookup locations.

## Source Modification Rule

Never modify files inside the user's installed MATPOWER package.

When a requested change requires modifying MATPOWER source behavior, copy the relevant MATPOWER `.m` file or files into the current project/workspace and edit those project-local copies. Place the copies wherever fits the project structure, such as the project root, `src/`, a local overrides folder, a vendored source directory, or an existing MATLAB source folder. Follow the repository's conventions instead of imposing a fixed folder name.

Before editing a copied MATPOWER source file:

1. Identify the installed MATPOWER source file being copied.
2. Identify the project-local copy that will be edited.
3. Confirm the project-local copy is earlier on the MATLAB/Octave path than the installed MATPOWER package.
4. Use `which <function_name>` to verify MATLAB/Octave resolves the intended project-local function.
5. Identify the affected call chain and the regression cases to run after the change.

If an extension can be implemented cleanly with case data, `mpoption`, OPF extension fields, or `userfcn` callbacks, prefer that over source copying.

## Validation Expectations

For generated or modified MATLAB code, include a small runnable check when feasible:

```matlab
define_constants;
mpc = loadcase('case9');
results = runpf(mpc, mpoption('verbose', 0, 'out.all', 0));
assert(results.success == 1);
```

For OPF work, test at least `case9` or `case30` plus the user's target case when available. For project-local MATPOWER source overrides, verify `which <function_name>` before running tests.
