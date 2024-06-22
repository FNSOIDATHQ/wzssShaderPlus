## Catalog
* [Overview](#overview)  
* [Auxiliary Functions](#auxiliary-functions)
* [Auxiliary Variables](#auxiliary-variables)
* [Return](./menu.md)

# Overview
This section focuses on helper functions and variables that can be used directly in **Custom CodeBlocks**. 
These are either built-in DirectX or globally defined variables and functions that you can call from anywhere.  
To a certain extent I encourage developers to use these tools instead of CodeBlocks: the readability of CodeBlocks is a real concern when writing GSM files directly.

# Auxiliary Functions
## All DirectX built-in functions

Reference links:

[Internal Functions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-intrinsic-functions)

[CG function documentation](https://developer.download.nvidia.com/cg/index_stdlib.html)

## pow_safe
GSM modified pow function, changes unknown.

## ParallaxOcclusionMapping
Calculate parallax mapping, usage to be added.
## Depth
Calculate the screen space depth, usage to be added.
## RotateVector2D
Rotate texture coordinates, usage to be added.
## ReflectCube
Compute global reflection, usage to be added.

---
# Auxiliary Variables
## _in_color
Vertex color.
## _in_tc_3d
3d texture coordinates.
## _in_normal_world
World space normal.  
Notes:  
* Does not contain normal maps, only normal information from the model itself.

## _in_tangent_world
World space tangent.

## _in_binormal_world
World space bi-tangent vector.
## _in_position
position in projected space (unsure).

## _in_pos_proj
position in projection space.

## _in_pos_world
World space position.

## _in_view_tangent
Tangent space direction vector.

## camera_origin
World camera position.

## scene_time
Scene time.

## scene_environment_color
The color of the scene environment.

## wind_amplitude
Wind frequency.

## DEPTH_DIVIDER
```
scene_depth = a.r * DEPTH_DIVIDER.
```
helper variable to calculate the screen space depth.

## camera_viewproj
```
float4 proj = mul(float4(world_pos, 1), camera_viewproj);
```
Calculate world space to projection space.

## gamma_value
```
out0=pow(a, gamma_value);
```
Gamma value.