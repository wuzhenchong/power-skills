# Results and Postprocessing

Top-level MATPOWER functions generally return a `results` struct that extends the input `mpc` with solved values and metadata.

## Standard Pattern

```matlab
define_constants;
results = runopf('case30', mpoption('verbose', 0, 'out.all', 0));

if ~results.success
    error('MATPOWER solve failed');
end

vm = results.bus(:, VM);
pg = results.gen(:, PG);
pf = results.branch(:, PF);
```

## Useful Fields

- `success`: solve success flag.
- `et`: elapsed time.
- `order`: external/internal ordering metadata.
- `f`: OPF objective value, when applicable.
- `bus`, `gen`, `branch`, `gencost`: solved or carried case data.

## Postprocessing Practices

- Check `success` before trusting numerical outputs.
- Use constants for result columns.
- For OPF, report `results.f` along with dispatch and binding constraints when relevant.
- Keep units explicit in plots and tables.
- When comparing cases, ensure both are in the same ordering and base units.
