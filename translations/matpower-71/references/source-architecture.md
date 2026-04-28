# 源码架构

修改 MATPOWER 内部逻辑或分析调用链前，应先阅读本文件。本文件关注 MATPOWER 7.1 传统 API。

## 潮流调用链

典型 `runpf` 流程：

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

实际路径取决于 AC/DC 模式和 PF 算法。

## OPF 调用链

典型 `runopf` 流程：

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

这是导航图，不替代阅读当前本地源码。

## 关键文件

- `runpf.m`：PF 顶层包装。
- `rundcpf.m`：DC PF 顶层包装。
- `runopf.m`：OPF 顶层包装。
- `rundcopf.m`：DC OPF 顶层包装。
- `runcpf.m`：CPF 顶层包装。
- `loadcase.m`、`savecase.m`：case 输入输出。
- `caseformat.m`：case 格式帮助。
- `ext2int.m`、`int2ext.m`：外部/内部数据转换。
- `idx_bus.m`、`idx_gen.m`、`idx_brch.m`、`idx_cost.m`：列常量。
- `makeYbus.m`、`makeSbus.m`、`makeBdc.m`：网络矩阵和注入。
- `newtonpf.m`、`fdpf.m`、`gausspf.m`、`dcpf.m`：潮流算法。
- `opf.m`、`opf_setup.m`、`opf_execute.m`：OPF 组织逻辑。
- `add_userfcn.m`、`run_userfcn.m`、`remove_userfcn.m`：callback 机制。
