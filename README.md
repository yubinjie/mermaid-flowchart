# mermaid-flowchart skill

用自然语言描述需求，生成可直接使用的 Mermaid 代码。支持流程图、时序图、类图、状态图、甘特图、饼图等。

## 适用场景

- 画算法流程图、系统架构图、数据管线
- 技术文档、论文方法论配图（与 nature-writing / nature-paper2ppt 配合）
- 项目讨论时快速可视化逻辑
- 代码输出为文本，可版本管理（Git diff 友好）

## 使用方式

在对话中用中文/英文描述你要画的图，例如：
- "帮我画一个 3D Gaussian Splatting 的训练流程图"
- "画一个 ORB-SLAM 的跟踪-建图-回环三条线程交互的时序图"
- "画我论文里多传感器融合的架构图"

Skill 会自动选型并生成 Mermaid 代码。

## Skill 结构

```
mermaid-flowchart/
  SKILL.md                       # 核心 skill 指令
  README.md                      # 本文件
  references/
    syntax.md                    # Mermaid 语法速查表
    examples.md                  # 遥感/DL/SLAM 方向实用模板
```

## 渲染工具

| 工具 | 方式 |
|-----|------|
| GitHub / GitLab | Markdown 中直接写 ```mermaid 代码块 |
| VS Code | 插件 bierner.markdown-mermaid |
| Mermaid Live | https://mermaid.live (在线) |
| Notion | /mermaid 命令 |
| 命令行导出 | `mmdc -i input.mmd -o output.png` |
