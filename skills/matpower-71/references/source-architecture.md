# Source Architecture

Read this before changing MATPOWER internals or reasoning about call chains. This reference focuses on MATPOWER 7.1 legacy APIs.

## Power Flow Call Chain

Typical `runpf` flow:

```text
runpf
  -> mpoption/loadcase
  -> ext2int
  -> bustypes
  -> makeYbus / makeSbus
  -> newtonpf OR fdpf OR gausspf OR dcpf
  -> pfsoln
  -> int2ext
  -> printpf / savecase
```

The exact branch depends on AC vs DC mode and selected PF algorithm.

## OPF Call Chain

Typical `runopf` flow:

```text
runopf
  -> opf
      -> opf_setup
          -> ext2int
          -> userfcn ext2int/formulation stages
          -> build OPF model
      -> opf_execute
      -> int2ext
      -> userfcn int2ext/printpf/savecase stages
  -> printpf / savecase
```

Use this as a navigation map, not as a substitute for reading the exact local source.

## Important Files and Roles

- `runpf.m`: top-level PF wrapper.
- `rundcpf.m`: top-level DC PF wrapper.
- `runopf.m`: top-level OPF wrapper.
- `rundcopf.m`: top-level DC OPF wrapper.
- `runcpf.m`: top-level CPF wrapper.
- `loadcase.m`, `savecase.m`: case input/output.
- `caseformat.m`: authoritative case format help.
- `ext2int.m`, `int2ext.m`: external/internal data conversion.
- `idx_bus.m`, `idx_gen.m`, `idx_brch.m`, `idx_cost.m`: column constants.
- `makeYbus.m`, `makeSbus.m`, `makeBdc.m`: network matrices and injections.
- `newtonpf.m`, `fdpf.m`, `gausspf.m`, `dcpf.m`: PF algorithms.
- `opf.m`, `opf_setup.m`, `opf_execute.m`: OPF orchestration.
- `add_userfcn.m`, `run_userfcn.m`, `remove_userfcn.m`: callback mechanism.

## Internal Ordering Warning

Many lower-level functions expect internal indexing. Do not pass external-numbered custom data into internal-stage code without conversion.
