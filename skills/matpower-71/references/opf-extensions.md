# OPF Extensions

For custom OPF objectives or constraints, choose the least invasive mechanism that fits the task.

## Decision Order

1. Modify standard case data if enough (`gencost`, limits, loads, branch ratings, voltage limits).
2. Use MATPOWER OPF extension fields for additional linear/quadratic costs and constraints.
3. Use `userfcn` callbacks when data must be transformed, added to the OPF model, printed, or saved at specific stages.
4. Edit project-local copies of MATPOWER source files only when the above mechanisms cannot express the required behavior.

## Recognized OPF Extension Fields

MATPOWER OPF recognizes fields such as:

- `A`, `l`, `u`: linear constraints.
- `H`, `Cw`, `N`, `fparm`: generalized cost components.
- `z0`, `zl`, `zu`: user variable initial values and bounds.

Read the local or official `opf` help before generating complex formulations.

## Callback-Based Extension

Use callbacks for staged customization:

- `ext2int`: convert custom input data to internal indexing.
- `formulation`: add variables, constraints, or costs to the OPF model.
- `int2ext`: convert custom result data back to external indexing.
- `printpf`: add custom printed output.
- `savecase`: preserve custom fields in saved case files.

## Source Editing Boundary

If a custom objective requires changing how MATPOWER builds or executes the OPF and cannot be implemented through extension fields or callbacks, copy the relevant source file into the current project and edit the copy. Do not edit the installed MATPOWER package.
