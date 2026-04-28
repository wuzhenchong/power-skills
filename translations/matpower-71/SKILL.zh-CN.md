---
name: matpower-71
description: 指导使用 MATPOWER 7.1 编写、审查、调试 MATLAB/Octave 程序，并在必要时理解和修改项目本地的 MATPOWER 源码副本。正式发布和机器触发以英文 SKILL.md 为准，本文件用于中文查看和维护。
---

# MATPOWER 7.1

这个 skill 用于依赖 MATPOWER 7.1 的 MATLAB/Octave 工作。除非用户明确要求 MATPOWER 8.x 的 MP-Core，否则优先采用 MATPOWER 7.1 的传统 API 和行为。

## 核心规则

1. 优先使用 `define_constants` 或 `idx_bus`、`idx_gen`、`idx_brch`、`idx_cost` 提供的命名常量，不要硬编码矩阵列号。
2. 使用 `loadcase` 读取 case，使用 `savecase` 保存 case，并保持 MATPOWER case struct 的语义。
3. 潮流使用 `runpf` 或 `rundcpf`，连续潮流使用 `runcpf`，最优潮流使用 `runopf` 或 `rundcopf`。
4. 使用 `mpoption` 配置求解器、算法、容差、日志和输出。
5. 自定义 OPF 行为时，先检查标准扩展字段 `A`、`l`、`u`、`H`、`Cw`、`N`、`fparm`、`z0`、`zl`、`zu` 和 `gencost` 是否足够；需要分阶段插入逻辑时再用 `userfcn` callback。
6. 不要原地修改用户电脑上安装的 MATPOWER 包。如果必须改变 MATPOWER 源码行为，只修改当前项目中的 `.m` 文件副本，并确认 MATLAB/Octave 优先解析到这些本地副本。

## 渐进式引用

- 普通编程模式：`references/zh-CN/quickstart.md`
- case 格式：`references/zh-CN/case-format.md`
- 常量、列索引、内部/外部编号：`references/zh-CN/constants-and-indexing.md`
- PF/CPF/OPF、callback、求解器、结果处理、调试和源码修改：英文正式参考在 `references/` 下；中文摘要可继续补充在 `references/zh-CN/`。

## 源码修改边界

绝不修改用户电脑上原本安装的 MATPOWER 包。

如果任务确实需要修改 MATPOWER 源码行为，应把相关 `.m` 文件复制到当前项目或工作区中，再修改这个项目本地副本。副本放在哪里要服从项目结构，例如项目根目录、`src/`、本地 overrides 目录、vendored 目录或已有 MATLAB 源码目录，不强制固定目录名。

编辑前需要确认：

1. 原始安装包中的 MATPOWER 源码路径。
2. 将要编辑的项目本地副本路径。
3. 本地副本在 MATLAB/Octave path 中排在安装包之前。
4. `which <function_name>` 指向预期的本地副本。
5. 受影响的调用链和修改后的回归测试 case。
