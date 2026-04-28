# 连续潮流

连续潮流使用 `runcpf`，常用于电压稳定和负荷裕度分析。

## 基本模式

```matlab
define_constants;
mpc_base = loadcase('case9');
mpc_target = mpc_base;
mpc_target.bus(:, [PD QD]) = 1.5 * mpc_base.bus(:, [PD QD]);

mpopt = mpoption('verbose', 0, 'out.all', 0);
results = runcpf(mpc_base, mpc_target, mpopt);
```

## 使用建议

- CPF 需要 base case 和 target case。
- base case 必须先能正常求解。
- target case 应代表要研究的负荷增长或传输方向。
- 读取 CPF 特有结果前，先检查 `results.success`。

单个运行点用 `runpf`；涉及经济调度、约束或控制优化时用 OPF。
