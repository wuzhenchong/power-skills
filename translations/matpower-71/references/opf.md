# 最优潮流

AC OPF 使用 `runopf`，DC OPF 使用 `rundcopf`。

## AC OPF

```matlab
define_constants;
mpc = loadcase('case30');
mpopt = mpoption('verbose', 0, 'out.all', 0);
[results, success] = runopf(mpc, mpopt);
assert(success == 1);
fprintf('Objective = %.6f\n', results.f);
```

## DC OPF

```matlab
results = rundcopf('case118', mpoption('verbose', 0, 'out.all', 0));
```

DC OPF 适合线性化的有功调度和阻塞分析；如果无功、电压、网损或 AC 可行性重要，应使用 AC OPF。

## 发电成本

`mpc.gencost` 定义 OPF 成本。常见模型：

- 多项式成本，`MODEL = 2`。
- 分段线性成本，`MODEL = 1`。

访问成本列时使用 `idx_cost` 或 `define_constants`。

## 常见结果

- `results.f`：目标函数值。
- `results.success`：是否成功。
- `results.bus`、`results.gen`、`results.branch`：求解后的系统数据。
- `results.raw`：部分配置下的底层求解器输出。
