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

然后，这份列表很可能有缺漏和错误，这是因为它们是依靠阅读硬编码文本得到的结果，很可能我遗漏了某些代码块没有介绍，或者理解错了代码块的实际含义，如果发现请提出issue。

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
* HLSL代码实现

的顺序介绍每个函数。
在输入输出参数中，我们记
* [参数类型]

为一个参数。  
一些参数可能实际上可以以不同的类型输入，在下个可能的文档版本我会添加多类型输入的记录。

---
在正文开始前，我们快速列出一个完整的代码块表供跳转：  
跳转表是为下载用户准备的，如果你在github直接查看请跳过。  

[数学函数](#数学函数)  

* [GSM新增](#gsm新增)  
* [Math/Add](#mathadd)  
* [Math/Div](#mathdiv)  
* [Math/OneMinus](#mathoneminus)  
* [Math/Sub](#mathsub)  
* [DX内部函数](#dx内部函数)  
* [Math/Abs](#mathabs)  
* [Math/Ceil](#mathceil)  
* [Math/Clamp](#mathclamp)  
* [Math/Cosine](#mathcosine)  
* [Math/Cross](#mathcross)  
* [Math/Dot](#mathdot)  
* [Math/Exp](#mathexp)  
* [Math/Floor](#mathfloor)  
* [Math/Fraction](#mathfraction)  
* [Math/Lerp](#mathlerp)  
* [Math/Max](#mathmax)  
* [Math/Min](#mathmin)  
* [Math/Mul](#mathmul)  
* [Math/Power](#mathpower)  
* [Math/Round](#mathround)  
* [Math/Saturate](#mathsaturate)  
* [Math/Smoothstep](#mathsmoothstep)  


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

* [Combine](#combine)  
* [DiffuseToAO](#diffusetoao)  
* [Extract](#extract)  
* [Fresnel](#fresnel)  
* [FromTangent](#fromtangent)  
* [GammaToLinear](#gammatolinear)  
* [Normalize](#normalize)  
* [Pan](#pan)  
* [Parallax](#parallax)  
* [Reflect](#reflect)  
* [ReflectCube](#reflectCube)  
* [Rotate2D](#rotate2D)  
* [SpecularToMetallicRoughness](#speculartometallicroughness)  
* [WorldToProjected](#worldtoprojected)  
* [Desaturate](#desaturate)  
* [SampleAtlas](#sampleatlas)  

[注释](#注释)  

* [Comments](#comments)  

[自定义代码块](#自定义代码块)  

* [CustomCode](#customcode)  
* [CustomVertexCode](#customvertexcode)  
---
# 数学函数
所有的数学函数都以"Math/"开头，然而GSM并不提供全部可能用到的数学函数，偶尔你可能需要手动实现它们。  
我们只介绍和标准DX内部函数不同的代码块，相同的仅标注是否确定与DX版本一样。  

---
我们按照是否为DX内部函数进行分类然后以字典序介绍它们：  
（实际上GSM就加了几个基础运算符）


---
## ***GSM新增***
## Math/Add  
```
	{"Math/Add"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float
* float

输出：
* float

>加法。
## Math/Div  
```
	{"Math/Div"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float
* float

输出：
* float

>除法。
HLSL代码实现：  
>out0=a/b;
## Math/OneMinus  
```
	{"Math/OneMinus"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float

输出：
* float

>1-输入0

HLSL代码实现：  
>out0=1-a;
## Math/Sub  
```
	{"Math/Add"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float
* float

输出：
* float

>减法。  

HLSL代码实现：  
>out0=a-b;


---
## ***DX内部函数***
## Math/Abs  
>等同于DX内部函数abs。
## Math/Ceil  
>没有使用样例，很可能等同于DX内部函数ceil。
## Math/Clamp  
>没有使用样例，很可能等同于DX内部函数clamp。
## Math/Cosine  
>没有使用样例，很可能等同于DX内部函数cos。
## Math/Cross  
>没有使用样例，很可能等同于DX内部函数cross。
## Math/Dot  
>等同于DX内部函数dot。
## Math/Exp
>等同于DX内部函数exp。
## Math/Floor  
>没有使用样例，很可能等同于DX内部函数floor。
## Math/Fraction  
>等同于DX内部函数frac。
## Math/Lerp  
>等同于DX内部函数lerp。
## Math/Max  
>等同于DX内部函数max。
## Math/Min  
>没有使用样例，很可能等同于DX内部函数min。
## Math/Mul  
>等同于DX内部函数mul。
## Math/Power  
>等同于DX内部函数pow。
## Math/Round  
>没有使用样例，很可能等同于DX内部函数round。
## Math/Saturate  
>等同于DX内部函数saturate。
## Math/Smoothstep  
>没有使用样例，很可能等同于DX内部函数smoothstep。



# 输入参数
输入参数大致被分为四部分：Parameter/，Camera/，Vertex/和其它。
然而这种分类不是完全严谨的，我会按照稍有调整的字典序+功能分类顺序介绍参数。  

---
第一部分是标准参数，以Parameter/开头，基本上来说他们都是float的变形：  
## Parameter/Color  
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
## Parameter/Float  
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
## Parameter/Float2  
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
## Parameter/Float3  
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
## Parameter/Luminosity  
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
## Parameter/Texture
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
                ;{normal_type}
                {filter}
		    }
		}
	}
```
常见使用样例：
```
	{"Parameter/Texture"
		{uid 4}
		{position 22 259}
		{properties
			{name diffuse}
			{srgb 0}
		}
	}
```
```
	{"Parameter/Texture"
		{uid 11}
		{position 330 580}
		{properties
			{name bump}
			{content normals}
		}
	}
```
```
	{"Parameter/Texture"
		{uid 11}
		{position -260 531}
		{properties
			{name bumpvolume_noise}
			{type VOLUME}
			{content signed}
			{srgb 0}
		}
	}
```
```
	{"Parameter/Texture"
		{uid 103}
		{position -133 741}
		{properties
			{name depth_tex}
			{file "@scene_depth"}
			{tile 0}
			{srgb 0}
		}
	}
```
输入：
* float3
* float3(存疑)

输出：
* float3
* float

>它代表一张可能包含透明度的四通道贴图。  
>输出0=颜色通道，输出1=Alpha通道。  
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
>
>{filter}用处未知，仅在progress.gsm&progress_mask.gsm中出现
>
>{normal_type}可能已经弃用

注意：  
* 该参数默认显示在前端，不需要手动开启，使用{file @x}时前端显示自动关闭
* 第0个输入一般为纹理坐标或者预乘了纹理坐标的数据，即Vertex/Texcoord
* 第1个输入内容未知，似乎仅有ReflectCube的输出接到这里
* 不同的参数组合不一定可以一起用，但是目前我没有深入研究
* 特殊参数：@sky_map 很可能可用直接获取当前场景的天空球贴图
---
第二部分是变换参数，其中相机变换以Camera/开头：  
## Camera/Direction  
```
	{"Camera/Direction"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出相机的世界空间方向向量。

HLSL代码实现：  
>Camera direction = normalize(camera_origin - _in_pos_world);
## Camera/DirectionTangentSpace  
```
	{"Camera/DirectionTangentSpace"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出相机的切线空间方向向量。

HLSL代码实现：  
>Camera direction TS = normalize(_in_view_tangent);
## Camera/Distance  
```
	{"Camera/Distance"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float

>输出物体与相机在世界空间的距离。  

HLSL代码实现：  
>Camera distance = distance(camera_origin, _in_pos_world);
## Camera/Origin  
```
	{"Camera/Origin"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出相机的世界空间位置。

HLSL代码实现：  
>Camera origin = camera_origin;
## ProjectedPosition  
```
	{"ProjectedPosition"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出物体的投影空间位置。

HLSL代码实现：  
>Projected position = _in_pos_proj;
## WorldPosition  
```
	{"WorldPosition"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>输出物体的世界空间位置。

HLSL代码实现：  
>World position = _in_pos_world;
---
第三部分是顶点参数，大部分以Vertex/开头：  
## Vertex/Color  
```
	{"Vertex/Color"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3
* float

>包含Alpha通道的顶点颜色。  
>输出0=颜色，输出1=Alpha。

## Vertex/Texcoord  
```
	{"Vertex/Texcoord"
		{uid 1}
		{position 123 456}
		{properties
			{set 0}
		}
	}
```
输出：
* float2

>2D纹理坐标，即UV。  
>一般作为贴图的输入。  
>{set x}指定了获取哪个通道的UV作为输入，一般为0。

## Texcoord3D  
```
	{"Texcoord3D"
		{uid 1}
		{position 123 456}
	}
```
输出：
* float3

>3D纹理坐标，实际上不是真的3D纹理，而是立方体贴图的纹理坐标。  
>一般作为天空盒贴图的输入。  
---
第四部分是场景参数，它们提供了来自环境或者系统或者屏幕的信息：  
## Depth
```
	{"Depth"
		{uid 1}
		{position 123 456}
		{properties
			{name depth}
			{file "@scene_depth"}
			{tile 0}
			{srgb 0}
		}
	}
``` 
输出：
* float
* float

>待补充
## Time  
```
	{"Time"
		{uid 1}
		{position 123 456}
		{properties
			{scale 1}
		}
	}
``` 
输出：
* float(猜测)

>用法不明，猜测为全局时间
>{scale x}为简单的值缩放，即output=x*SceneTime
## SceneTime 
```
	{"SceneTime"
		{uid 1}
		{position 123 456}
		{properties
			{scale 1}
		}
	}
``` 
输出：
* float

>场景时间  
>计量方法不明  
>{scale x}为简单的值缩放，即output=x*SceneTime
## SceneEnvironmentColor 
```
	{"SceneEnvironmentColor"
		{uid 1}
		{position 123 456}
	}
``` 
输出：
* float3

>即编辑器中的environmet/sky/color

注意：  
* 这不是天空盒的色彩，天空盒需要自己[采样](#reflectcube)获得
## WindAmplitude  
```
	{"WindAmplitude"
		{uid 1}
		{position 123 456}
	}
``` 
输出：
* float

>风振幅，即风的频率，反映风由小到大再变小的周期

注意：  
* 这不是风速，风速可能被记录为wind_params（硬编码中它再风速的旁边），但是没有进一步的信息
---
第五部分是特殊参数：  
## Scheme
```
	{"Scheme"
		{uid 1}
		{position 123 456}
		{properties
			{id 1}
		}
	}
```
输出：
* 视该Scheme的Material类型而定


>将一个GSM文件作为输入。  
>GSM很可能是“GEM Scheme Material”的简称，所以Scheme可以简单代指一个GSM文件。  
>{id x}参数是必要的，这和shader_combinations.set中的代码结构有关：  
>>大致上来说，在该文件中会以"该着色器：第1个着色器输入：第2个着色器输入"的格式编译这种嵌套着色器（实际输入顺序可能是反的，我没有具体研究）  
>>所以请按从1编号的顺序填写id
>
>输出引脚与指定Material的输出引脚严格对照。
# 流程控制
GSM仅提供IF作为流程控制，我们会在[自定义代码块](./custom.md)中提到其它进行流程控制的办法。  
值得注意的是：显卡并不擅长进行流程控制，巧妙地把流程控制转变成数学计算或者在编译阶段剔除它们更加合适。  
在GSM中，如果想要做两个互补的功能，最好把它们分散到两个着色器里。然而如果少量的流程控制能带来便利，这仍然是可以接受的代价。 

## If
```
	{"If"
		{uid 1}
		{position 123 456}
	}
```
输入：
* float?
* float?
* float?
* float?

输出：
* float?

>我不知道这个函数是怎么运作的，虽然它看起来很像流程控制，但是我还没猜出来具体要怎么用

# 工具集
这些函数执行从输入到输出的一组特定改变，往往具体实现由多行代码组成。  
它们或是简化了代码编写，或是提供了特殊的功能。  
我会按照字典序介绍参数。

---
以下是包含了特殊功能的代码块  
## Combine  
```
	{"Combine"
		{uid 1}
		{position 123 456}
		{properties
			{inputs 3}
		}
	}
``` 
输入：
* 视inputs参数而定

输出：
* 视inputs参数而定

>组合输入参数的通道。
>inputs的范围可能是1-4，4没有在任何地方使用过，所以不确定是否真的可用。

注意：  
* 不确定，但是猜测只能组合单通道输入，如果想要组合flot2+float或者float3+float这样的形式可能要预先拆分
## DiffuseToAO  
```
	{"DiffuseToAO"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3

输出：
* float3
* float

>基于预烘焙了阴影的漫反射计算AO。  
>输出0=漫反射  
>输出1=AO  
## Extract  
```
	{"Extract"
		{uid 1}
		{position 123 456}
		{properties
			{components A}
		}
	}
``` 
输入：
* float4

输出：
* 视组件类型而定

>分离输入参数的通道
>组件ABCD分别等于a.x|a.y|a.z|a.w

注意：  
* 你可以组合分离出的通道，格式为{components "A B C"}
## Fresnel  
```
	{"Fresnel"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3
* float

输出：
* float3

>我不知道这个代码块实际上在干什么，它的唯一使用在gem2/water.gsm中。
## FromTangent  
```
	{"FromTangent"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3
* float

输出：
* float3

>将切向空间法线转换到世界空间。  
>输入0=切向法线  
>输入1=法线强度  

注意：  
* 它隐含了法线强度计算
## GammaToLinear  
```
	{"GammaToLinear"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3

输出：
* float3

>进行gamma到线性转换

HLSL代码实现：  
>out0= pow(a, gamma_value);
## Normalize  
```
	{"Normalize"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float

输出：
* float

>等效于DX内部函数normalize

## Pan
```
	{"Pan"
		{uid 1}
		{position 123 456}
		{properties
			{speed_x 1}
			{speed_y 1}
			{speed_z 1}
		}
	}
``` 
输入：
* float3
* float
* float
* float
* float
* float（？）

输出：
* float3

>用于执行贴纹理坐标位移。  
>输入0=纹理坐标（透过Extract Component A或B解压）  
>输入1=总位移速度  
>输入2，3，4=分xyz方向的位移速度  
>似乎存在输入5=时间？不知道具体含义

注意：  
* 一般来说输入234被定义在properties中
* 看起来可以通过properties直接定义输入引脚的固定参数？我不知道者是否可以推广到别的代码块上
## Parallax
```
	{"Parallax"
		{uid 1}
		{position 123 456}
	}
``` 
>很复杂，待补充。

## Reflect
```
	{"Reflect"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3
* float3

输出：
* float3

>似乎仅仅是DX内部函数reflect的取反。  
>输入0=输入射线，输入1=法线。

注意：  
* 没有使用样例，输入输出是我按照硬编码猜的，我不确定我找到的是否确定是reflect的实现代码

HLSL代码实现：  
>out0=-reflect(a, b);
## ReflectCube  
```
	{"ReflectCube"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3
* float3

输出：
* float3

>用于计算反射。  
>输入0=相机的世界空间方向，输入1=?。  
>输出0为反射角度（？）。

注意：  
* 我很确定它有两个输入，但是第二个输入未知，所有的样例均不输入第二个参数
* 输出的实际内容未知，但是一般接到Texture的1号输入引脚上
## Rotate2D  
```
	{"Rotate2D"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float2
* float

输出：
* float2

>可能用来旋转UV。  
>输入0=UV，输入1=旋转角度。  
>输出0为旋转后的UV。  

注意：  
* 没有使用样例，输入输出是我按照硬编码猜的
## SpecularToMetallicRoughness
```
	{"SpecularToMetallicRoughness"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3

输出：
* float
* float

>将输入的高光贴图转换为金属度和粗糙度。  
>具体转换原理待补充，大致可以参考[官方文档](https://bestway-1.gitbook.io/dokumentaciya/tekstury-i-materialy/physically-based-rendering/redaktor-skhem-pbr-i-konversiya-tekstur)。  
>输出0为金属度，输出1为粗糙度。  
## WorldToProjected 
```
	{"WorldToProjected"
		{uid 1}
		{position 123 456}
	}
``` 
输入：
* float3

输出：
* float3

>将输入的世界空间坐标转换到投影空间。

---
这是几个额外的代码块，我在硬编码里看到了它，但是我不知道要怎么用，也不确定它是否真的有用
## Desaturate  
> 去饱和度？还是取消钳制？？
## SampleAtlas
# 注释
你可以放置一个只包含注释的代码块，被称为Comments，我相信这在材质编辑器中会很有用。

## Comments
```
	{"Comments"
		{uid 1}
		{position 123 456}
		{properties
			{comments "temp"}
			{size 100 100}
		}
	}
```

>猜测size参数决定2d视图中该块的大小。
# 自定义代码块
这两个代码块用于添加自定义代码，我们在[自定义代码块](./custom.md)详细阐述这两个代码块的使用。
## CustomCode  
>自定义代码。
## CustomVertexCode  
>自定义顶点着色器代码。