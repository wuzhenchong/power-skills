# MATPOWER 7.1 Skill 工程说明

这个仓库不是单纯的 skill 文件夹，而是用于创建和改进 MATPOWER 7.1 skill 的工程。

## 目录结构

```text
skill/matpower-71/
  SKILL.md                 # 正式英文 skill 入口
  agents/openai.yaml       # skill UI 元数据
  references/*.md          # 正式英文渐进式参考资料

translations/zh-CN/
  SKILL.zh-CN.md           # 便于查看和修改的中文翻译
  references/*.md          # 中文参考资料翻译
```

正式发布和使用的 skill 是 `skill/matpower-71`。中文资料只作为维护和改进材料，故意不放进正式 skill 目录，避免中英文内容混在一起。

## 核心设计目标

- 指导使用 MATPOWER 7.1 编写 MATLAB/Octave 程序。
- 通过渐进式 references 覆盖常用功能，而不是每次加载整本手册。
- 在需要修改源码时，帮助 Codex 理解 MATPOWER 7.1 的关键调用链。
- 明确边界：不要修改用户电脑上安装的 MATPOWER 包；如需改源码，只改当前项目中的本地副本。
