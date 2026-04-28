# Quick Start

Use MATPOWER 7.1 as a MATLAB/Octave library for steady-state power system simulation and optimization. Keep examples small and runnable, and prefer the legacy MATPOWER 7.1 function names.

## Basic Setup

```matlab
define_constants;
mpc = loadcase('case9');
mpopt = mpoption('verbose', 0, 'out.all', 0);
```

Use `define_constants` in scripts and examples so code can address matrix columns by names such as `PD`, `VM`, `PG`, `PF`, `RATE_A`, and `MODEL`.

## Common Tasks

Power flow:

```matlab
results = runpf(mpc, mpopt);
assert(results.success == 1);
```

DC power flow:

```matlab
results = rundcpf(mpc, mpopt);
```

Optimal power flow:

```matlab
results = runopf(mpc, mpopt);
f = results.f;
```

DC optimal power flow:

```matlab
results = rundcopf(mpc, mpopt);
```

Save a case:

```matlab
savecase('solved_case9.m', results);
```

## Coding Conventions

- Accept either a case struct or a case filename when wrapping MATPOWER functions.
- Return the `results` struct and preserve `results.success`.
- Keep user-facing examples compatible with MATLAB and Octave where practical.
- Avoid relying on MATPOWER 8.x classes unless the user asks for them.
- Do not edit installed MATPOWER source files; edit project-local copies only.
