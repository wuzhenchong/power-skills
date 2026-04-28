# Testing and Debugging

Use small official cases for quick checks and the user's target case for final validation.

## Installation Check

```matlab
test_matpower
```

For a lighter check:

```matlab
define_constants;
results = runpf('case9', mpoption('verbose', 0, 'out.all', 0));
assert(results.success == 1);
```

## Path Debugging

```matlab
which runpf
which runopf
which caseformat
```

When project-local overrides are used, `which` must point to the intended project file, not the installed MATPOWER package.

## Regression Cases

- `case9`: minimal smoke test.
- `case30`: common PF/OPF sanity check.
- `case118`: larger DC/OPF screening.
- User target case: final relevance check.

## Common Failure Causes

- Missing or wrong MATLAB/Octave path.
- No reference bus or invalid bus types.
- Disconnected islands without suitable generation.
- Generator limits inconsistent with load.
- Very small or zero branch impedance.
- Incorrect units or base MVA assumptions.
- Solver not installed or selected incorrectly.
- Custom code using external bus numbers as internal row indices.
