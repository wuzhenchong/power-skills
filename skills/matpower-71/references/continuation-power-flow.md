# Continuation Power Flow

Use `runcpf` for continuation power flow studies such as voltage stability and loadability analysis.

## Basic Pattern

```matlab
define_constants;
mpc_base = loadcase('case9');
mpc_target = mpc_base;
mpc_target.bus(:, [PD QD]) = 1.5 * mpc_base.bus(:, [PD QD]);

mpopt = mpoption('verbose', 0, 'out.all', 0);
results = runcpf(mpc_base, mpc_target, mpopt);
```

## Guidance

- CPF requires a base case and a target case.
- Keep the base case solvable before running CPF.
- Use target case changes that represent the intended loading or transfer direction.
- Read CPF-specific result fields only after confirming `results.success`.

## When Not To Use CPF

Use ordinary `runpf` for a single operating point. Use OPF when dispatch, costs, constraints, or control optimization are central.
