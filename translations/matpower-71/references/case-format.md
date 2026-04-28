# Case 格式

MATPOWER 7.1 的 case 文件通常是返回 `mpc` struct 的 `.m` 文件，也可以是 `.mat` 文件。

## 常见字段

- `mpc.version`：通常是 `'2'`。
- `mpc.baseMVA`：系统 MVA 基准。
- `mpc.bus`：母线矩阵。
- `mpc.gen`：发电机矩阵。
- `mpc.branch`：支路矩阵。
- `mpc.gencost`：OPF 成本数据，常用可选字段。

## 扩展字段

OPF 可识别 `A`、`l`、`u`、`H`、`Cw`、`N`、`fparm`、`z0`、`zl`、`zu` 等字段。其他用户自定义字段可以由 `loadcase` 读入；如需被 `savecase` 保存，通常需要配合 `savecase` callback。

## 注意事项

- 负荷和有功发电通常是 MW，无功是 MVAr，电压幅值是 p.u.，角度是 degree。
- PF/OPF 运行后，`bus`、`gen`、`branch` 会包含额外结果列。
- 编写代码时用命名常量访问列，例如 `PD`、`VM`、`PG`、`PF`。
