# 结果和后处理

MATPOWER 顶层函数通常返回扩展后的 `results` struct，包含输入 case 数据、求解结果和元数据。

## 标准模式

```matlab
define_constants;
results = runopf('case30', mpoption('verbose', 0, 'out.all', 0));

if ~results.success
    error('MATPOWER solve failed');
end

vm = results.bus(:, VM);
pg = results.gen(:, PG);
pf = results.branch(:, PF);
```

## 常用字段

- `success`：成功标志。
- `et`：耗时。
- `order`：外部/内部编号转换信息。
- `f`：OPF 目标函数值。
- `bus`、`gen`、`branch`、`gencost`：求解后或继承的 case 数据。

## 后处理建议

- 先检查 `success`，再相信数值结果。
- 使用常量访问结果列。
- OPF 结果应同时报告目标函数、出力和关键约束。
- 图表和表格中明确单位。
