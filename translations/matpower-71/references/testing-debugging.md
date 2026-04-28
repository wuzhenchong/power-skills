# 测试和调试

先用小型官方 case 做快速检查，再用用户目标 case 做最终验证。

## 安装检查

```matlab
test_matpower
```

轻量检查：

```matlab
define_constants;
results = runpf('case9', mpoption('verbose', 0, 'out.all', 0));
assert(results.success == 1);
```

## 路径调试

```matlab
which runpf
which runopf
which caseformat
```

如果使用项目本地源码副本，`which` 必须指向预期的项目文件，而不是安装包文件。

## 常用回归 case

- `case9`：最小烟测。
- `case30`：常见 PF/OPF 检查。
- `case118`：较大的 DC/OPF 筛选。
- 用户目标 case：最终相关性验证。

## 常见失败原因

- MATLAB/Octave path 错误。
- 缺少参考母线或母线类型错误。
- 孤岛无合适发电。
- 发电机限值和负荷不一致。
- 支路阻抗异常。
- 单位或 baseMVA 假设错误。
- 选择了未安装的求解器。
- 把外部母线编号误当成内部矩阵行号。
