## 目录
*  [概述](#概述)
*  [PBR+](#pbr)
* * [standard](#standard)
* * [standardComb](#standardcomb)
* * [standardCombMask](#standardcombmask)
*  [NPR](#npr)
* * [simple](#simple)
*  [返回](./menu.md)

# 概述
WSP的新增着色器主要分为两个部分：PBR+（基于物理的渲染+）,NPR（非真实渲染）。  
其中PBR+专注于为物理渲染添加新特性，例如自发光贴图支持，和增加面向开发者的优化，例如改进型AO滑条和多通道纹理支持。  
而NPR专注于在PBR的基础上添加非真实渲染技术，目前主要在追赶Unity Toon Shader的技术特性。
我们以着色器类型为基础描述每个着色器的特性，着色器名字不做翻译，仍然保持英文原名，方便对照。

注意：
* 为了统一格式，我们在命名上全部使用了驼峰命名法，所以一些已有功能的参数名也发生了改变，这些参数不特别说明，按照参考文件修改名字即可。

# PBR+
## standard
标准PBR+着色器，包含了添加的所有特性。  
mtl参考：[standard.gsm](./mtl/pbrPlus/standard.mtl)

**特性**：  
1.  ### 自发光  
我们实现了（实际上是向MOW II的接口传递了）标准的自发光功能。

参数说明：
> {emissive "X"} 指定自发光贴图  
> {emissiveRate 0} 表示自发光强度，可以是任意值  
> {emissiveColor 0xff0000} 表示自发光颜色  
> {emissiveType 0} 切换自发光类型，为0时只使用emissiveColor作为自发光颜色，为1时额外读取贴图  

2.  ### 改进型AO  
现在AORate不再以奇怪的方式表达：在原方法中AO=0时模型全黑，AO=1时正常读取AO贴图。  
这显然不是我们期望的，重新编写功能后：0=无AO贴图影响，1=正常读取AO贴图，2=模型全黑。  
新的参数计算方法更符合直观，同时不需要把AORate调的极大来模拟无AO。

参数说明：
> {aoRate 1} 改进AO, 0=无AO 1=标准AO 2=全黑AO  
## standardComb
多通道纹理着色器，不再需要为每个PBR贴图单独配置一个文件，同时支持一些优化功能。

mtl参考：[standardComb.gsm](./mtl/pbrPlus/standardComb.mtl)

**特性**：  
1.  ### 多通道纹理  
我们实现了一张包含了粗糙度，金属度和法线的多通道贴图，只有AO仍然由单张贴图提供。  

参数说明：
> {combine "X"} 红色通道=粗糙度，蓝色通道=金属度，绿通道=法线贴图绿色通道，Alpha通道=法线贴图红色通道  

2.  ### 改进型AO  
现在AORate不再以奇怪的方式表达：在原方法中AO=0时模型全黑，AO=1时正常读取AO贴图。  
这显然不是我们期望的，重新编写功能后：0=无AO贴图影响，1=正常读取AO贴图，2=模型全黑。  
新的参数计算方法更符合直观，同时不需要把AORate调的极大来模拟无AO。

参数说明：
> {aoRate 1} 改进AO, 0=无AO 1=标准AO 2=全黑AO  

3.  ### 法线贴图读取  
支持读取OpenGL或DirectX格式的法线贴图（MOW II默认为DirectX格式），不再需要手动转换或使用两张贴图来确定法线是否反转。

参数说明：
> {bumpType 0} 0=DirectX 1=OpenGL  

注意：
* 该功能实际上也可以设bumpRate参数为-1实现，不过额外添加参数更直观。

4.  ### 光滑度贴图读取  
某些情况下会使用光滑度（smoothness）贴图而不是粗糙度贴图，光滑度是粗糙度的反相。

参数说明：
> {roughnessFlip 0} 0=不反相 1=反相  

## standardCombMask
额外支持蒙版的多通道纹理着色器。  

mtl参考：[standardCombMask.gsm](./mtl/pbrPlus/standardCombMask.mtl)

**特性**：  
1.  ### 全部[standardComb](#standardcomb)支持的特性  
不重复说明

2.  ### 漫反射蒙版  
支持额外添加一个贴图对基础色做蒙版，例如可以用于制作可变做旧，自由调整载具的做旧程度。

参数说明：
> {mask "X"} 蒙版贴图，其中RGB通道为要遮罩上去的颜色，Alpha通道为蒙版强度  
> {maskRate 1} 遮罩强度 0=无遮罩 1=标准遮罩 2=完全使用遮罩作为基础色  
# NPR
在wzssShaderPlusLite\scene\entity\nprSample中包含一个或一些NPR的使用样例，因为这对MOW II来说是一个全新的领域，我会使用一些模型来展示具体的参数选择。  
在此感谢 @晓美烟 提供模型。

## simple
简单的NPR着色器实现，包含了目前可以做到的功能。  

mtl参考：[simple.gsm](./mtl/npr/simple.mtl)

**特性**：  
1.  ### 自发光  
我们实现了（实际上是向MOW II的接口传递了）标准的自发光功能。

参数说明：
> {emissive "X"} 指定自发光贴图  
> {emissiveRate 0} 表示自发光强度，可以是任意值  
> {emissiveColor 0xff0000} 表示自发光颜色  
> {emissiveType 0} 切换自发光类型，为0时只使用emissiveColor作为自发光颜色，为1时额外读取贴图  

2.  ### 描边  
我们实现了最基础的描边，该描边很可能**只适用于**人物描边，或者其它曲面较多的模型。很可惜，由于代码访问限制，我们很可能无法在现有支持的功能中实现任何一种其他的主流描边方法。

参数说明：
> {outlineRate 0.1} 描边宽度  

3.  ### 漫反射色调分离  
我们实现了一个标准的色调分离方法，用于稍稍改善那些原本不是为NPR设计的贴图的风格样式注意该功能往往效果不佳，建议酌情使用。

参数说明：
> {colorScale 8} 色阶数量  
> {toonRate 0.5} 色调分离贴图和原贴图的混合比例，0=原贴图 1=色阶贴图  
> {lightup 0} 提高漫反射贴图的整体亮度，如果发现色调分离后颜色过暗可以酌情考虑启用  