# 选项和求解器

使用 `mpoption` 管理 MATPOWER 选项，不要随意修改未文档化的内部选项结构。

## 常用选项

```matlab
mpopt = mpoption('verbose', 0, 'out.all', 0);
mpopt = mpoption(mpopt, 'pf.enforce_q_lims', 1);
mpopt = mpoption(mpopt, 'opf.ac.solver', 'MIPS');
```

## 输出控制

- `verbose`：控制过程信息。
- `out.all`：控制格式化输出。

测试和脚本中通常使用安静输出，便于得到稳定日志。

## 求解器建议

- MIPS 随 MATPOWER 提供，是示例和通用代码中较稳妥的默认选择。
- 可选商业或第三方求解器可能更快，但不能假定用户已安装。
- 可复用函数应允许调用者传入 `mpopt`。
- 调试时应检查实际 `mpopt`，不要盲目假设默认值。
