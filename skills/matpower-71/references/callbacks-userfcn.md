# Userfcn Callbacks

MATPOWER `userfcn` callbacks let project code participate in simulation stages without directly changing installed MATPOWER source files.

## Registration Pattern

```matlab
mpc = add_userfcn(mpc, 'formulation', @my_formulation_callback, args);
results = runopf(mpc, mpopt);
```

Use function handles or function names consistent with the user's MATLAB version and project style.

## Stages

- `ext2int`: after MATPOWER converts the case to internal indexing.
- `formulation`: while building the OPF model.
- `int2ext`: after results are converted back to external indexing.
- `printpf`: when pretty-printing results.
- `savecase`: when saving custom fields.

## Callback Guidance

- Keep callback functions small and stage-specific.
- Treat `ext2int` and `formulation` data as internal-order data.
- Preserve unrelated fields in `mpc` and `results`.
- Pass configuration through the optional `args` argument rather than global variables.
- Add a `savecase` callback if custom fields must survive `savecase`.

## When To Prefer Callbacks

Prefer callbacks when the task is an extension of MATPOWER behavior, such as adding reserve constraints, interface limits, custom output, or custom saved fields. Prefer project-local source copies only when callbacks and extension fields are inadequate.
