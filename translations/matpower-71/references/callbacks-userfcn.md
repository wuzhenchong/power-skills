# Userfcn 回调

MATPOWER 的 `userfcn` callback 允许项目代码在仿真阶段插入逻辑，而不需要直接修改安装包源码。

## 注册模式

```matlab
mpc = add_userfcn(mpc, 'formulation', @my_formulation_callback, args);
results = runopf(mpc, mpopt);
```

## 阶段

- `ext2int`：case 转换为内部编号后。
- `formulation`：构造 OPF 模型时。
- `int2ext`：结果转换回外部编号后。
- `printpf`：打印结果时。
- `savecase`：保存 case 时。

## 建议

- callback 函数应小而专注。
- `ext2int` 和 `formulation` 阶段的数据通常是内部编号。
- 不要破坏无关的 `mpc` 和 `results` 字段。
- 用 `args` 传配置，避免全局变量。
- 自定义字段需要被 `savecase` 保存时，添加 `savecase` callback。
