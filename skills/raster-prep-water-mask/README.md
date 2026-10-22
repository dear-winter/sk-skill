# raster-prep-water-mask

一个用于地理栅格整理与去水域的 Agent Skill：统一网格、研究区及分区掩膜、按明确规则剔除水域，并逐文件检查几何、NoData和保留像元数值。

中文为主，English below.

## 功能

五种任务模式：Inspect 仅检查，Align 网格对齐，Zone 区域或分区掩膜，Water 去水域，Batch 按需串联。仅检查不会改写栅格，单阶段请求不会自动扩展为全流程。

处理约定：优先继承实际参考栅格；已对齐文件不重复插值；分类及等级使用最近邻；裁剪默认保留共同矩形网格；水域依据用户指定来源及判据，不能默认把0或NDVI异常当作水域。

质量核验：每张输出回读检查，区分区域外NoData、水域排除和原始内部缺值。纯掩膜及去水域要求保留像元值不变。新CSV保存为UTF-8 BOM并回读验证。

## 安装与调用

从 sk-skill 仓库下载后，将 skills/raster-prep-water-mask 目录复制到宿主的skills目录，保证目录下直接包含SKILL.md。Codex使用 `~/.codex/skills/raster-prep-water-mask`，Claude Code使用 `~/.claude/skills/raster-prep-water-mask`。

安装后新开会话，调用示例：

```text
使用 $raster-prep-water-mask，把这批TIF对齐到reference.tif，
按boundary.shp掩膜，再按指定water_mask.tif去水域。
保留原文件，输出检查表。
```

更多请求见 [examples/example-requests.md](examples/example-requests.md)。不支持skill加载的环境，可将SKILL.md及按需引用的规则文件作为指令提供给Agent。

## 环境要求

宿主Agent需要本地文件访问及shell执行能力。实际处理使用已有R/terra或GDAL等GIS工具；本skill不捆绑这些工具，也不包含预先写死的批处理程序。Agent按已确认输入生成任务脚本并核验结果。

## 限制

- 不包含土壤厚度预测、承载力计算、自动填洞或地图绘制。
- 不预设某个项目的CRS、分辨率、水域编码及有效像元数。
- 不能根据网格一致证明来源边界或水域识别的真实地理准确性。
- 无法确认CRS、分类含义或水域规则时，需要补充输入，不能猜测。

## 文件结构

```text
raster-prep-water-mask/
  SKILL.md
  README.md
  agents/openai.yaml
  references/processing-rules.md
  references/output-contract.md
  examples/example-requests.md
```

## English

Raster Prep and Water Mask is an Agent Skill for bounded raster preprocessing: inspect metadata, align to a shared reference grid, mask study areas or zones, remove water using explicit source data, and verify outputs. It preserves originals, distinguishes intentional exclusion from internal missing data, and checks retained values after masking.

It requires host access to GIS tools such as R/terra or GDAL. It contains workflow instructions rather than a bundled raster engine or fixed execution script. It does not train predictive models, calculate ecological scores, fill gaps automatically, or design maps. Reference-grid settings and water definitions are supplied per project.

## License

MIT License，Copyright (c) 2026 dear-winter。见 [LICENSE](LICENSE)。

## 致谢

本技能的整理与文档编写由 ChatGPT 辅助完成，维护者为 dear-winter。
