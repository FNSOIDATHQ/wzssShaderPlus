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

的顺序介绍每个函数。

# 数学函数
所有的数学函数都以"Math/"开头，然而GSM并不提供全部可能用到的数学函数，偶尔你可能需要手动实现它们。  

---
我们按照字典序介绍它们：

### Math/Abs  
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
### Parameter/Float  
### Parameter/Float2  
### Parameter/Float3  
### Parameter/Luminosity  
### Parameter/Texture
---
第二部分是变换参数，其中相机变换以Camera/开头：  
### Camera/Direction  
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