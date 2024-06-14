## 目录  
*  [概述](#概述)  
*  [数学函数](#数学函数)
*  [输入参数](#输入参数)
*  [流程控制](#流程控制)
*  [工具集](#工具集)
*  [自定义代码块](#自定义代码块)  
*  [返回](./menu.md)  

# 概述
首先，我建议你查阅[英伟达的CG函数文档](https://developer.download.nvidia.com/cg/index_stdlib.html)，那里有关于同名代码块更加详细的解释，特别是数学函数部分。我会在稍后补充非单个参数代码块的实际代码实现。（它们不容易被解读，因为全部被硬编码进可执行文件中了）

然后，这份列表很可能有缺漏，这是因为它们是依靠阅读硬编码文本得到的结果，很可能我遗漏了某些代码块没有介绍，如果发现请提出issue。

大致上来说，GSM提供的代码块分为6个部分：
* 数学函数
* 输入参数
* 流程控制
* 工具集
* 自定义代码块
* 输出块

其中输出块作为基础概念已经在上一章详细解释过，不再在本章列出。  

我会按照：
* 名字
* 代码块样例
* 输入输出参数
* 功能概述
* 注意事项

的顺序介绍每个函数。
在输入输出参数中，我们记
* [参数类型]

为一个参数

---
在正文开始前，我们快速列出一个完整的代码块表供跳转：  

[数学函数](#数学函数)  
* [Math/Abs](#mathabs)  
* [Math/Add](#mathadd)  
* [Math/Ceil](#mathceil)  
* [Math/Clamp](#mathclamp)  
* [Math/Cosine](#mathcosine)  
* [Math/Cross](#mathcross)  
* [Math/Div](#mathdiv)  
* [Math/Dot](#mathdot)  
* [Math/Exp](#mathexp)  
* [Math/Floor](#mathfloor)  
* [Math/Fraction](#mathfraction)  
* [Math/Lerp](#mathlerp)  
* [Math/Max](#mathmax)  
* [Math/Min](#mathmin)  
* [Math/Mul](#mathmul)  
* [Math/OneMinus](#mathoneminus)  
* [Math/Power](#mathpower)  
* [Math/Round](#mathround)  
* [Math/Saturate](#mathsaturate)  
* [Math/Smoothstep](#mathsmoothstep)  
* [Math/Sub](#mathsub)  

[输入参数](#输入参数)  
* [Parameter/Color](#parametercolor)  
* [Parameter/Float](#parameterfloat)  
* [Parameter/Float2](#parameterfloat2)  
* [Parameter/Float3](#parameterfloat3)  
* [Parameter/Luminosity](#parameterluminosity)  
* [Parameter/Texture](#parametertexture)  
* [Camera/Direction](#cameradirection)  
* [Camera/DirectionTangentSpace](#cameradirectiontangentspace)  
* [Camera/Distance](#cameradistance)  
* [Camera/Origin](#cameraorigin)  
* [ProjectedPosition](#projectedposition)  
* [WorldPosition ](#worldposition )  
* [Vertex/Color](#vertexcolor)  
* [Vertex/Texcoord](#vertextexcoord)  
* [Texcoord3D ](#texcoord3d)  
* [Depth](#depth)  
* [Time](#time)  
* [SceneTime](#scenetime)  
* [SceneEnvironmentColor](#sceneenvironmentcolor)  
* [WindAmplitude ](#windamplitude)  
* [Scheme](#scheme)  

[流程控制](#流程控制)  

* [If](#if)  

[工具集](#工具集)  

* [DiffuseToAO](#diffusetoao)  
* [Extract](#extract)  
* [Fresnel](#fresnel)  
* [Pan](#pan)  
* [Parallax](#parallax)  
* [ReflectCube](#reflectCube)  
* [Rotate2D](#rotate2D)  
* [SpecularToMetallicRoughness](#speculartometallicroughness)  
* [Combine](#combine)  
* [Desaturate](#desaturate)  
* [FromTangent](#fromtangent)  
* [GammaToLinear](#gammatolinear)  
* [Normalize](#normalize)  
* [Reflect](#reflect)  
* [WorldToProjected](#worldtoprojected)  
* [SampleAtlas](#sampleatlas)  

[注释](#注释)  

* [Comments](#comments)  

[自定义代码块](#自定义代码块)  

* [CustomCode](#customcode)  
* [CustomVertexCode](#customvertexcode)  
---
# 数学函数
所有的数学函数都以"Math/"开头，然而GSM并不提供全部可能用到的数学函数，偶尔你可能需要手动实现它们。  

---
我们按照字典序介绍它们：

### Math/Abs  
```
	{"Math/Abs"
		{uid 1}
		{position 123 456}
	}
```
输入：
* float

输出：
* float

>计算输入参数的绝对值。

### Math/Add  
### Math/Ceil  
### Math/Clamp  
### Math/Cosine  
### Math/Cross  
### Math/Div  
### Math/Dot  
### Math/Exp
### Math/Floor  
### Math/Fraction  
### Math/Lerp  
### Math/Max  
### Math/Min  
### Math/Mul  
### Math/OneMinus  
### Math/Power  
### Math/Round  
### Math/Saturate  
### Math/Smoothstep  
### Math/Sub  

# 输入参数
输入参数大致被分为四部分：Parameter/，Camera/，Vertex/和其它。
然而这种分类不是完全严谨的，我会按照稍有调整的字典序+功能分类顺序介绍参数。  

---
第一部分是标准参数，以Parameter/开头，基本上来说他们都是float的变形：  
### Parameter/Color  
```
	{"Parameter/Color"
		{uid 1}
		{position 123 456}
		{properties
			{external
				{on}
				{name sample}
			}
			{value 0xffffffff}
		}
	}
```
输出：
* float3
* float(存疑)

>颜色参数  
>在包含external之后支持在编辑器前端调整该变量。  
>值使用十六进制数表示，具体转换方法参考[该网站](https://www.runoob.com/html/html-colorvalues.html)。

注意：  
* external内只有{on}没有{off}
* 该参数很可能不支持输出透明度，尽管在所有GSM里都使用八位十六进制表示。如果实际上支持，则第二个输出引脚为透明度
* 在mtl中，请使用六位十六进制表示值，八位会只读取后六位导致行为不一致
### Parameter/Float  
```
	{"Parameter/Float"
		{uid 1}
		{position 123 456}
		{properties
			{external
				{on}
				{name sample}
			}
			{value 1}
		}
	}
```
输出：
* float

>单浮点参数。  
>在包含external之后支持在编辑器前端调整该变量。  

注意：  
* external内只有{on}没有{off}
### Parameter/Float2  
```
	{"Parameter/Float2"
		{uid 1}
		{position 123 456}
		{properties
			{external
				{on}
				{name sample}
			}
			{a 0}
			{b 1}
		}
	}
```
输出：
* float2

>双浮点参数，a=第一个浮点值，b=第二个浮点值。  
>在包含external之后支持在编辑器前端调整该变量。  

注意：  
* external内只有{on}没有{off}
### Parameter/Float3  
```
	{"Parameter/Float3"
		{uid 1}
		{position 123 456}
		{properties
			{external
				{on}
				{name sample}
			}
			{a 0}
			{b 1}
			{c 2}
		}
	}
```
输出：
* float3

>三浮点参数，a=第一个浮点值，b=第二个浮点值，c=第三个浮点值。  
>在包含external之后支持在编辑器前端调整该变量。  

注意：  
* external内只有{on}没有{off}
### Parameter/Luminosity  
```
	{"Parameter/Luminosity"
		{uid 1}
		{position 123 456}
		{properties
			{external
				{on}
				{name sample}
			}
			{value 1}
		}
	}
```
输出：
* float

>它理论上是一个单浮点参数。  
>在包含external之后支持在编辑器前端调整该变量。  

注意：  
* external内只有{on}没有{off}
* 一般该参数出现在输出到自发光引脚的流程中，但是实际测试时我没有看到任何差异
### Parameter/Texture
```
	{"Parameter/Texture"
		{uid 1}
		{position 123 456}
		{properties
		    {properties
			    {name sample}
                {file "@sample"}
        		{type VOLUME}
			    {content signed}
                {tile 0}
			    {srgb 0}
		    }
		}
	}
```
输入：
* float3
* float(存疑)

输出：
* float3
* float

>它代表一张可能包含透明度的四通道贴图。  
>输出的第一个参数为颜色通道，第二个参数为Alpha通道。  
>
>使用{file x}参数可以将贴图指向指定路径的文件，此时贴图在前端被隐藏。  
>>使用{file @x}时会根据输入创建一个叫x的贴图：  
>
>{tile x}参数规定了贴图作用于uv上的哪个tile，关于tile参见[UV tiles](https://helpx.adobe.com/substance-3d-painter/features/uv-tiles.html)。  
>
>{srgb x}参数规定了是否按照srgb色彩空间来处理贴图：
>>为0则按照线性空间读取  
>>srgb和线性空间的差异参见[Linear, Gamma and sRGB Color Spaces](https://matt77hias.github.io/blog/2018/07/01/linear-gamma-and-sRGB-color-spaces.html)  
>
>{type x}决定了输入贴图的类型，存在三种状态：
>>2D 默认参数，即标准2D贴图  
>>VOLUME 待补充，一般与{content signed}连用  
>>CUBE 立方体贴图，一般用于天空盒  
>
>{content x}决定输入贴图的特殊读取方法，存在三种状态：
>>default 默认参数，即正常贴图  
>>normals 法线贴图，似乎可以指定读取方式，待补充
>>signed 待补充，一般与{type VOLUME}连用  

注意：  
* 该参数默认显示在前端，不需要手动开启
* 输入不是必须的，只有使用{file @x}时需要输入，如果没有则使用前端输入的贴图
* 不同的参数组合不一定可以一起用，但是目前我没有深入研究
---
第二部分是变换参数，其中相机变换以Camera/开头：  
### Camera/Direction  
```
	{"Camera/Direction"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出相机的世界空间方向向量
### Camera/DirectionTangentSpace  

### Camera/Distance  
### Camera/Origin  

### ProjectedPosition  
### WorldPosition  
---
第三部分是顶点参数，大部分以Vertex/开头：  
### Vertex/Color  
### Vertex/Texcoord  
### Texcoord3D  
---
第四部分是场景参数，它们提供了来自环境或者系统或者屏幕的信息：  
### Depth
### Time  
### SceneTime  
### SceneEnvironmentColor  
### WindAmplitude  
---
第五部分是特殊参数：  
### Scheme

# 流程控制
GSM仅提供IF作为流程控制，我们会在[自定义代码块](./custom.md)中提到其它进行流程控制的办法。  
值得注意的是：显卡并不擅长进行流程控制，巧妙地把流程控制转变成数学计算或者在编译阶段剔除它们更加合适。  
在GSM中，如果想要做两个互补的功能，最好把它们分散到两个着色器里。然而如果少量的流程控制能带来便利，这仍然是可以接受的代价。 

### If

# 工具集
这些函数执行从输入到输出的一组特定改变，往往具体实现由多行代码组成。  
它们或是简化了代码编写，或是提供了特殊的功能。  
我会按照字典序+功能分类顺序介绍参数。

---
以下是包含了特殊功能的代码块  
### DiffuseToAO  
### Extract  
### Fresnel  
### Pan
### Parallax
### ReflectCube  
### Rotate2D  
### SpecularToMetallicRoughness

---
以下是简化编写的代码块  
### Combine  
### Desaturate  
### FromTangent  
### GammaToLinear  
### Normalize  
### Reflect
### WorldToProjected  
---
这是一个额外的代码块，我在硬编码里看到了它，但是我不知道要怎么用，也不确定它是否真的有用
### SampleAtlas
# 注释
你可以放置一个只包含注释的代码块，被称为Comments，我相信这在材质编辑器中会很有用。

### Comments


# 自定义代码块
这两个代码块用于添加自定义代码，我们在[自定义代码块](./custom.md)详细阐述这两个代码块的使用。
### CustomCode  
自定义代码
### CustomVertexCode  
自定义顶点着色器代码