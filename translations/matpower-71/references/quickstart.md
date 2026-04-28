# 快速开始

MATPOWER 7.1 作为 MATLAB/Octave 库，用于稳态电力系统仿真和优化。示例应尽量小、可运行，并采用 MATPOWER 7.1 的传统函数名。

## 基本模式

```matlab
define_constants;
mpc = loadcase('case9');
mpopt = mpoption('verbose', 0, 'out.all', 0);
```

潮流：

```matlab
results = runpf(mpc, mpopt);
assert(results.success == 1);
```

最优潮流：

```matlab
results = runopf(mpc, mpopt);
f = results.f;
```

## 编码习惯

- 包装函数应接受 case struct 或 case 文件名。
- 返回 `results` struct，并保留 `results.success`。
- 使用 `define_constants` 或 `idx_*`，不要硬编码列号。
- 不要修改安装目录里的 MATPOWER 源码；需要修改时只改项目本地副本。
