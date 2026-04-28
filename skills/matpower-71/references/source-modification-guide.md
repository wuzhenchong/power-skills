# Source Modification Guide

Use this guide when the task requires changing MATPOWER source behavior.

## Non-Negotiable Boundary

Do not edit the user's installed MATPOWER package in place.

Copy the relevant MATPOWER `.m` file or files into the current project/workspace and edit those project-local copies. The destination is flexible: use the project root, `src/`, an overrides directory, a vendored directory, or whatever source layout the project already uses. The important rule is that the installed MATPOWER package remains unchanged.

## Before Editing

Identify and record:

1. The installed MATPOWER source file being copied.
2. The project-local copy that will be edited.
3. The reason a normal extension mechanism is insufficient.
4. The affected call chain.
5. The tests or cases that will be run afterward.

Verify path precedence:

```matlab
which runpf
which opf_setup
```

The relevant `which` result must point to the project-local copy when an override is intended.

## Prefer Extension Points First

Before copying source, check whether the task can be done by:

- editing case data,
- setting `mpoption`,
- changing `gencost`,
- using OPF extension fields,
- adding a `userfcn` callback.

Source copying is appropriate when the behavior is truly inside MATPOWER's algorithm or wrapper logic and cannot be expressed through supported extension points.

## Editing Practice

- Keep the copied file as close to upstream MATPOWER 7.1 as practical.
- Add a short comment near each intentional local modification.
- Avoid broad refactors while changing numerical logic.
- Preserve function signatures unless the local project controls every caller.
- Be careful with internal vs external indexing.
- Use small regression cases first, then the user's target case.

## Common Project-Local Overrides

- `runpf.m`: wrapper behavior, logging, result handling.
- `runopf.m`: wrapper behavior around OPF calls.
- `opf_setup.m`: OPF model construction.
- `opf_execute.m`: solver execution path.
- `makeYbus.m`, `makeSbus.m`: network model construction.
- `newtonpf.m`: Newton PF iteration logic.
- `printpf.m`: output formatting.
- `savecase.m`: custom case serialization.

Do not assume these are the only valid files to copy; follow the actual call chain for the requested change.
