# Power Flow

Use `runpf` for AC power flow and `rundcpf` for DC power flow.

## AC Power Flow

```matlab
define_constants;
mpc = loadcase('case30');
mpopt = mpoption('verbose', 0, 'out.all', 0);
[results, success] = runpf(mpc, mpopt);
assert(success == 1);
```

`runpf` accepts either a case struct or a case filename. With one or two output arguments, prefer the modern `results` struct style.

## DC Power Flow

```matlab
results = rundcpf('case30', mpoption('verbose', 0, 'out.all', 0));
```

DC PF is useful for fast active-power approximations and screening. Do not use DC PF when reactive power, voltage magnitude, or AC feasibility is central to the task.

## Reactive Power Limits

To enforce generator reactive limits during AC PF:

```matlab
mpopt = mpoption('pf.enforce_q_lims', 1);
results = runpf('case30', mpopt);
```

When Q-limit enforcement converts a PV bus to PQ, voltage magnitude may deviate from the original setpoint.

## Important Result Fields

- `results.success`: 1 on success, 0 on failure.
- `results.bus(:, VM)`, `results.bus(:, VA)`: solved bus voltage magnitude and angle.
- `results.gen(:, PG)`, `results.gen(:, QG)`: solved generator output.
- `results.branch(:, PF)`, `results.branch(:, QF)`, `results.branch(:, PT)`, `results.branch(:, QT)`: branch flows.
- `results.et`: elapsed time.

## Debugging Non-Convergence

Check data first: slack bus, in-service generators, branch status, voltage limits, load levels, disconnected islands, and suspicious impedance values. Then tune `mpoption` algorithms and tolerances.
