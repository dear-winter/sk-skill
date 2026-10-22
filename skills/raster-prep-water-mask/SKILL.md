---
name: raster-prep-water-mask
description: "Prepare geospatial rasters on a common reference grid, mask study areas or zones, remove water using explicit source data and rules, and verify geometry, NoData, and retained values. Chinese: 栅格整理、TIF批量对齐、统一坐标系和分辨率、分区掩膜裁剪、去水域与输出核验。Use for raster preprocessing, not predictive modeling, ecological scoring, automatic gap filling, or map design."
---

# Raster Prep and Water Mask

整理地理栅格、按研究区或分区掩膜、剔除水域并核验成果。按用户需要执行单个阶段或串联流程，复用已经合格的数据。

## Scope gate

先检查项目说明和已有数据。实际处理需要输入栅格、目标网格或建网参数、输出位置；掩膜处理还需要区域边界，去水域需要明确的数据来源及判据。仅检查任务不要求用户提供无关输入。

缺失关键设置时先查元数据及项目记录，仍不能确定再询问。不要从文件名猜CRS、把有效0当NoData、或以NDVI等替代用户指定的水域数据。

不自动训练模型、计算承载力、填补空洞或绘制专题图。未经用户指定，不将单个项目的分辨率、CRS、水域编码和像元数量推广到其他项目。

## Mode shortcut

- **Inspect**：检查元数据、网格、NoData和缺失值，输出问题清单；不修改栅格。
- **Align**：统一到参考网格；已对齐文件不再插值。
- **Zone**：研究区或分区掩膜；默认保留完整共同矩形网格。
- **Water**：按明确规则剔除水域，保持其余像元不变。
- **Batch**：按需串联以上阶段；不因批处理扩大输入范围。

## Workflow

1. **明确本批输入**：记录文件清单、变量类型、参考栅格、区域边界、水域规则和输出目录。读取 [处理规则](references/processing-rules.md) 对应章节。
2. **检查目标几何**：核对CRS、范围、像元大小、行列数及对齐位置；仅行列相同不等于同网格。
3. **最少转换**：不同CRS时直接投影到模板；同CRS但不同网格时按模板重采样；分类和等级使用最近邻，连续变量依据含义选择方法。
4. **应用区域及水域掩膜**：沿用既定边界规则，保留区域外、水域及原始缺失的区别。非水域内部空洞单独报告，不静默填补或取交集缩小范围。
5. **保存与回读**：新目录输出无损压缩GeoTIFF，逐张重新打开，依 [输出与核验约定](references/output-contract.md) 检查几何、掩膜和数值。
6. **交付**：提供结果路径、实际使用的方法、复现脚本及检查表，说明未解决的问题。

## Invariants

- 保留原始数据；不自动覆盖输入、清空已有目录或修改无关成果。
- 不用修改CRS标签代替真实投影转换；不对已对齐栅格为裁剪再次重采样。
- 水域与区域外NoData不填回；只有用户明确授权且来源可核实时才修正背景编码。
- 掩膜和去水域必须保持保留像元值不变；重采样则验证方法、类别和值域，不伪称数值未变。
- 结果验收只报告已执行的检查。结构或语法检查不能代替实际栅格回读。

## Outputs and traceability

小请求直接给检查结论或结果文件。实际转换任务保存可复现脚本、GeoTIFF及逐文件检查表；字段见 [输出与核验约定](references/output-contract.md)。

对任何未完成检查的文件，明确标记原因。数据源本来存在内部缺值时，区分“按规则处理成功”和“输入已满足后续完整覆盖要求”。

## Reference map

- [处理规则](references/processing-rules.md)：对齐、重采样、分区和去水域的判断与执行约束。
- [输出与核验约定](references/output-contract.md)：成果结构、逐文件检查字段和CSV编码。
- [调用示例](examples/example-requests.md)：单阶段、批处理及仅检查请求。

## Limits

本skill提供处理约定，不内置GIS引擎或数据。执行依赖宿主可调用的R/terra或GDAL等环境。缺少依赖时说明缺项，不自动安装；缺少真实CRS、边界或水域来源时不猜测。无法证明来源正确时，不能保证掩膜具有实际地理精度。
