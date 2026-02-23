# VTD 指标测试说明（按固定 disp2depth 参数）

根据你的要求，VTD 的深度换算使用固定参数：

- `f = 1634.5`
- `B = 0.5`
- `min_disp = 0.01`
- `max_depth = 100`

评估时使用：

`depth = f * B / (disp * width + min_disp)`

其中 `disp` 是网络输出的归一化视差（按图像宽度尺度），`width` 是当前评估分辨率下的图像宽度。

## 当前代码行为

- `depth_vtd`：使用上述公式将 `outputs[('disp','s')]` 转为深度后再算指标，`max_depth=100`。
- `depth_vtd_mono`：同上，但启用 median scaling。

## 使用建议

1. 评估 VTD 时使用：`--metric_name depth_vtd` 或 `--metric_name depth_vtd_mono`。
2. 确保 `inputs['depth']` 单位为米。
3. 避免复用 KITTI 的 crop mask 到 VTD。
