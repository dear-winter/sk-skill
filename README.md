# sk-skill

个人 Agent Skills 合集，每个技能可独立安装和使用。

## Skill 一览

| Skill | 用途 | 环境要求 |
| --- | --- | --- |
| [raster-prep-water-mask](skills/raster-prep-water-mask/) | 栅格网格对齐、研究区和分区掩膜、去水域及输出核验 | 宿主文件与shell访问；R/terra或GDAL等GIS工具 |

## 安装与调用

下载本仓库后，将所需技能目录复制到宿主的skills目录，保持SKILL.md位于技能目录根部。

- Codex：`~/.codex/skills/raster-prep-water-mask/`
- Claude Code：`~/.claude/skills/raster-prep-water-mask/`

新开会话后调用：

```text
使用 $raster-prep-water-mask，把这批TIF按参考栅格统一网格，
按指定边界掩膜并去水域，保留原文件并输出核验结果。
```

输入、依赖、限制及调用示例见各技能的README.md。技能不包含项目数据或固定的本地路径。

## License

[MIT License](LICENSE)，Copyright (c) 2026 dear-winter。
每个技能目录也保留LICENSE，便于独立分发。
