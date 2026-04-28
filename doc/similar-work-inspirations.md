# 类似工作与启发

检查时间：2026-04-28。信息来源：GitHub 插件读取的仓库文件。

## 看了哪些项目

- [BenchFlow SkillsBench: `ac-branch-pi-model`](https://github.com/benchflow-ai/skillsbench/tree/main/tasks/energy-ac-optimal-power-flow/environment/skills/ac-branch-pi-model)
- [Power-Agent PowerSkills](https://github.com/Power-Agent/PowerSkills)

## 他们的 skill 在做什么

### BenchFlow `ac-branch-pi-model`

这是一个很窄但很精确的公式型 skill。它只解决 AC 支路 pi 模型这件事：MATPOWER 支路字段、变压器 `TAP`、`SHIFT`、双向支路潮流、MVA 限值、支路 loading 百分比、节点注入汇总，以及符号和单位错误排查。

它的启发是：高风险数学细节值得单独做成小 reference 或小 skill。不要把所有内容都塞进一个大而全的入口文件。

### PowerSkills

PowerSkills 更接近本项目未来想做的方向。它把电力系统 agent skill 分成两层：

- 软件工具 skill：ANDES、Egret、LTSpice、OpenDSS、PSLF、PSSE、PowerWorld、PyPSA、pandapower、surge。
- 问题缓解 skill：电压越限、热过载、预想事故、动态稳定、运行规划问题。

它的核心方法是渐进式工作流：先打开或创建模型，再检查数据，再求解可信的基准工况，之后才进入预想事故、OPF、投资优化、动态仿真或通用 API 调用。缓解类 skill 则从已经观察到的问题出发，按工程师常用顺序给出动作：先确认问题真实，再尝试低风险控制手段，验证修复效果，最后才讨论加强改造。

## 对本项目的帮助

- 当前 MATPOWER skill 应继续保持“先可靠工作流，后高级细节”的结构：加载 case、检查数据、运行 PF/OPF/CPF、解释结果、调试收敛，最后才进入源码级理解或局部覆写。
- 可以增加更窄的数学参考页，例如 MATPOWER 支路潮流公式、`TAP`/`SHIFT` 处理、`PF/QF/PT/QT` 方向、`RATE_A` 限值和 loading 计算。
- 未来做 pandapower 时，应作为独立 skill，而不是塞进 MATPOWER skill。可参考 PowerSkills 的顺序：加载/创建网络、检查表结构、运行基准潮流、查看电压和线路负载，再决定是否做预想事故或 OPF。
- 可以补充问题导向的 playbook，例如电压越限、线路/变压器过载、OPF 不可行、预想事故结果归类、MATPOWER 源码覆写边界。
- 输出要求要更明确。PowerSkills 要求 agent 报告模型/网络、是否收敛、关键发现、是否需要升级到高级分析；这对 MATPOWER 结果总结也很有价值。

## 可以落地的设计

1. 新增 `skills/matpower-71/references/branch-flow-formulas.md`，专门写 MATPOWER 7.1 支路潮流方向、`TAP`、`SHIFT`、`BR_B`、`RATE_A`、`PF/QF/PT/QT`。
2. 新增 `skills/matpower-71/references/mitigation-playbooks.md`，或者未来拆成独立问题类 skill，覆盖电压、热过载、预想事故、OPF 不可行等场景。
3. 未来新增 `skills/pandapower/` 时，采用“创建/加载网络 -> 检查网络 -> 基准潮流 -> 结果筛查 -> 高级分析”的阶梯。
4. 继续保持 `SKILL.md` 简洁，公式、例子、诊断细节放进 progressive reference 文件。
5. 只在能明显减少 agent 重复错误时加入本地脚本，例如结构化 MATPOWER 结果摘要、支路 loading 检查、收敛失败诊断。

## 边界

- 不把中文翻译材料放进 `skills/matpower-71/`。
- 不把 MATPOWER 7.1 skill 变成多软件大集合。
- 不指导用户直接修改已安装的 MATPOWER 包。
- 继续优先使用 MATPOWER 7.1 legacy API，并用项目本地示例和验证步骤支撑修改。
