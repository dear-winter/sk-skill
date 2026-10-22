# 调用示例

以下为占位路径和示例规则，调用时替换为真实输入。

## 仅检查

> 使用 $raster-prep-water-mask 检查 inputs/ 下的TIF与reference.tif是否同网格，列出区域内额外缺值，先不修改文件。

## 批量对齐

> 使用 $raster-prep-water-mask 将 inputs/ 中这批TIF按 reference.tif 对齐。landcover.tif是类别，其他已说明为连续值。输出到 outputs/aligned/，保留原文件。

## 分区掩膜

> 使用 $raster-prep-water-mask 按 zones/ 中的山区和平原矢量提取已对齐栅格，保持参考网格范围和行列数，检查分区是否交叠。

## 指定规则去水域

> 使用 $raster-prep-water-mask，以 landcover.tif 中有效编码80作为水域，统一剔除指定批次栅格的水域，检查非水域值未变；不要填补其他缺值。

编码80仅为本例输入条件，不是默认水域编码。

## 串联处理

> 使用 $raster-prep-water-mask 先核对变量类型和网格，以 reference.tif 为模板统一输入，再按 boundary.shp 掩膜，最后按 water_mask.tif 的有效值1剔除水域。新成果写入 outputs/run_01/，输出复现脚本和检查表。
