# 潮流计算

AC 潮流使用 `runpf`，DC 潮流使用 `rundcpf`。

## AC 潮流

```matlab
define_constants;
mpc = loadcase('case30');
mpopt = mpoption('verbose', 0, 'out.all', 0);
[results, success] = runpf(mpc, mpopt);
assert(success == 1);
```

`runpf` 可以接受 case struct，也可以接受 case 文件名。推荐使用返回 `results` struct 的调用方式。

## DC 潮流

```matlab
results = rundcpf('case30', mpoption('verbose', 0, 'out.all', 0));
```

DC 潮流适合快速有功近似和筛选；如果无功、电压幅值或 AC 可行性很重要，应使用 AC 潮流。

## 无功限值

```matlab
mpopt = mpoption('pf.enforce_q_lims', 1);
results = runpf('case30', mpopt);
```

启用无功限值后，违反 Q 限值的 PV 母线可能被转换为 PQ 母线，电压幅值可能偏离原设定值。

## 常用结果

- `results.success`：是否成功。
- `results.bus(:, VM)`、`results.bus(:, VA)`：电压幅值和相角。
- `results.gen(:, PG)`、`results.gen(:, QG)`：发电机出力。
- `results.branch(:, PF)`、`results.branch(:, QF)`、`results.branch(:, PT)`、`results.branch(:, QT)`：支路潮流。
- `results.et`：耗时。
