## 目录
*  [概述](#概述)  
*  [辅助函数](#辅助函数)
*  [辅助变量](#辅助变量)
*  [返回](./menu.md)

# 概述
本章节主要介绍可以在**自定义代码块**中直接使用的辅助函数及变量。 
它们要不然是DirectX内置的，要不然是全局定义的变量和函数，你可以再任何地方直接调用它们。 
一定程度上我鼓励开发者使用这些工具而不是代码块：因为对于直接编写GSM文件来说代码块的可读性是非常堪忧的。

# 辅助函数
## 全部DirectX内置函数

参考链接：

[内部函数](https://learn.microsoft.com/zh-cn/windows/win32/direct3dhlsl/dx-graphics-hlsl-intrinsic-functions)

[CG函数文档](https://developer.download.nvidia.com/cg/index_stdlib.html)

## pow_safe
GSM修改过的pow函数，改动未知。

## ParallaxOcclusionMapping
计算视差贴图用，用法待补充。
## Depth
计算屏幕深度用，用法待补充。
## RotateVector2D
旋转纹理坐标用，用法待补充。
## ReflectCube
计算全局反射，用法待补充。

---
# 辅助变量
## _in_color
顶点颜色。
## _in_tc_3d
3d纹理坐标。
## _in_normal_world
世界空间法线。  
注意：  
* 不包含法线贴图，只有模型本身的法线信息。

## _in_tangent_world
世界空间切线。

## _in_binormal_world
世界空间双切向量。
## _in_position
投影空间坐标位置（不确定）。

## _in_pos_proj
投影空间坐标位置。

## _in_pos_world
世界空间坐标位置。

## _in_view_tangent
切线空间方向向量。

## camera_origin
世界空间相机位置。

## scene_time
场景时间。

## scene_environment_color
场景环境颜色。

## wind_amplitude
风频率。

## DEPTH_DIVIDER
```
scene_depth = a.r * DEPTH_DIVIDER;
```
计算屏幕空间深度用。

## camera_viewproj
```
float4 proj = mul(float4(world_pos, 1), camera_viewproj);
```
计算世界空间转投影空间用。

## gamma_value
```
out0=pow(a, gamma_value);
```
伽马值。
