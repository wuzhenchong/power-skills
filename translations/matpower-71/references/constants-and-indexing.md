# 常量和索引

MATPOWER 的 `bus`、`gen`、`branch`、`gencost` 都是按列定义含义的矩阵。写代码时不要硬编码列号。

## 推荐写法

```matlab
define_constants;
mpc = loadcase('case30');
mpc.bus(2, PD) = 30;
mpc.branch(:, RATE_A) = 100;
```

## 内部编号和外部编号

MATPOWER 在计算前会转换为内部编号：

- 移除孤立母线、停运发电机和停运支路。
- 母线编号在内部变成连续编号。
- 原始数据和排序信息存入 `mpc.order`。
- `runpf`、`runopf` 等顶层函数返回前通常会转换回外部编号。

写 callback 或修改底层源码时，必须确认当前数据处于内部编号还是外部编号。
