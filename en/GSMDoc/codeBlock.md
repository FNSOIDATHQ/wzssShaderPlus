## Catalog  
* [Overview](#overview)  
* [Math Function](#math-function)
* [Input Parameter](#input-parameter)
* [Control Flow](#control-flow)
* [Toolset](#toolset)
* [Comment](#comment)  
* [Custom CodeBlock](#custom-codeblock)  
* [Return](./menu.md)  

# Overview
First, I suggest you consult [NVIDIA's CG Function Documentation](https://developer.download.nvidia.com/cg/index_stdlib.html), where there is a more detailed explanation of the same name CodeBlock, especially the math function section.  
I'll add actual code implementations of non-single-argument CodeBlocks later. (They're not easy to decipher, since they're all hard-coded into the executable.)

Then, this list is likely to have omissions and errors, this is because they are the result of reading the hardcoded text, and it is likely that I have missed some CodeBlocks that are not presented, or misunderstood what the CodeBlocks actually mean, if you find them, please send an issue.

Roughly speaking, the CodeBlocks provided by GSM are divided into 6 sections:
* Math Function
* Input Parameter
* Control Flow
* Toolset
* Custom CodeBlock
* Output Block

Where output block as a basic concept have been explained in detail in the previous chapter and will not be listed in this chapter.  

I'll introduce each function in the order of:  
* Name
* Sample CodeBlock
* Input and output parameters
* Functional overview
* Notes
* HLSL code implementation

Among the input and output parameters, we note
* [parameter type]

for a parameter.  
Some parameters may actually be able to be entered as different types, and in the next possible version of the documentation I'll add documentation for multiple types of input.

---
Before the main text begins, let's quickly list a complete table of CodeBlocks for jumping:  
The jump table is for download users, so if you're viewing it directly at github please skip it.  

[Math Function](#math-function)

* [***GSM added***](#gsm-added)  
* [Math/Add](#mathadd)  
* [Math/Div](#mathdiv)  
* [Math/OneMinus](#mathoneminus)  
* [Math/Sub](#mathsub)  
* [***DX Internal Functions***](#dx-internal-functions)  
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


[Input Parameter](#input-parameter)  

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

[Control Flow](#control-flow)  

* [If](#if)  

[Toolset](#toolset)  

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

[Comment](#comment)  

* [Comments](#comments)  

[Custom CodeBlock](#custom-codeblock)  

* [CustomCode](#customcode)  
* [CustomVertexCode](#customvertexcode)  
---
# Math Function
All math functions start with "Math/", however GSM does not provide all possible math functions and occasionally you may need to implement them manually.  
We only present code blocks that differ from the standard DX internal functions, the same ones are only labeled if they are determined to be the same as the DX version.  

---
We categorize them by whether they are DX internal functions or not and then present them in dictionary order:  
(Actually, GSM adds just a few basic operators)


---
## ***GSM added***
## Math/Add  
```
	{"Math/Add"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float
* float

Output(s)：
* float

>Addition.  
## Math/Div  
```
	{"Math/Div"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float
* float

Output(s)：
* float

>Division.  

HLSL code implementation：  
>out0=a/b;
## Math/OneMinus  
```
	{"Math/OneMinus"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float

Output(s)：
* float

>1-Input0.

HLSL code implementation：  
>out0=1-a;
## Math/Sub  
```
	{"Math/Add"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float
* float

Output(s)：
* float

>Subtraction.  

HLSL code implementation：  
>out0=a-b;


---
## ***DX Internal Functions***
## Math/Abs  
>Equivalent to the DX internal function abs.
## Math/Ceil  
>No samples used, probably equivalent to the DX internal function ceil.
## Math/Clamp  
>No samples used, probably equivalent to the DX internal function clamp.
## Math/Cosine  
>No samples used, probably equivalent to the DX internal function cos.
## Math/Cross  
>No samples used, probably equivalent to the DX internal function cross.
## Math/Dot  
>Equivalent to the DX internal function dot.
## Math/Exp
>Equivalent to the DX internal function exp.
## Math/Floor  
>No samples used, probably equivalent to the DX internal function floor.
## Math/Fraction  
>Equivalent to the DX internal function frac.
## Math/Lerp  
>Equivalent to the DX internal function lerp.
## Math/Max  
>Equivalent to the DX internal function max.
## Math/Min  
>No samples used, probably equivalent to the DX internal function min.
## Math/Mul  
>Equivalent to the DX internal function mul.
## Math/Power  
>Equivalent to the DX internal function pow.
## Math/Round  
>No samples used, probably equivalent to the DX internal function round.
## Math/Saturate  
>Equivalent to the DX internal function saturate.
## Math/Smoothstep  
>No samples used, probably equivalent to the DX internal function smoothstep.



# Input Parameter
Input parameters are roughly categorized into four sections: Parameter/, Camera/, Vertex/ and others.  
However, this categorization is not completely rigorous, and I will present the parameters in a slightly adjusted dictionary-order + function-categorization order.  

---
The first part is the standard parameters, starting with Parameter/, which are basically variations of float:  
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
Output(s)：
* float3
* float(doubtful)

>Color parameter  
> Support for adjusting this variable on the front end of the editor after including external.  
>Values are expressed using hexadecimal numbers, refer to [this site](https://htmlcolorcodes.com/) for the conversion.

Notes:  
* There is only {on} but no {off} within external
* This parameter most likely does not support Output(s) transparency, even though the eight-bit hexadecimal representation is used in all GSMs. If it is in fact supported, the second Output(s) pin is transparent
* In mtl, use six bits of hexadecimal for the value, eight bits will read only the last six bits resulting in inconsistent behavior
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
Output(s)：
* float

> Single float parameter.  
> Support for tweaking this variable in the editor frontend after including external.  

Notes:  
* There is only {on} but no {off} within external
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
Output(s)：
* float2

> Double float parameter, a = first float value, b = second float value.  
> Support for tweaking this variable in the editor frontend after including external.  

Notes:  
* There is only {on} but no {off} within external
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
Output(s)：
* float3

> Three float parameter, a = first float value, b = second float value, c = third float value.  
> Support for tweaking this variable in the editor frontend after including external.  

Notes:  
* There is only {on} but no {off} within external
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
Output(s)：
* float

>It is theoretically a single float parameter.  
> Support for tweaking this variable in the editor frontend after including external.  

Notes:  
* There is only {on} but no {off} within external
* Normally this parameter appears in the flow from output to the emissive pins, but I didn't see any difference in the actual test
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
Common Usage Examples：
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
Input(s)：
* float3
* float3(doubtful)

Output(s)：
* float3
* float

> It represents a four-channel map that may contain transparency.  
>Output0 = color channel, Output1 = Alpha channel.  
>
>>Using {file x} parameter to point the texture to a file at the specified path, where the texture will be hidden on the front end.  
>>Using {file @x} creates a texture called x based on the Inputs.  
>
> The {tile x} parameter specifies which tile on the uv the texture acts on, see [UV tiles](https://helpx.adobe.com/substance-3d-painter/features/uv-tiles.html) for tiles.  
>
>{srgb x} parameter specifies whether the texture is processed according to the srgb color space:
>> A value of 0 reads the texture in linear space.  
>>See [Linear, Gamma and sRGB Color Spaces](https://matt77hias.github.io/blog/2018/07/01/linear-gamma-and-sRGB-color-spaces.html) for the difference between srgb and linear spaces.  
>
>{type x} determines the type of input texture, there are three states:
>>2D | Default parameter, i.e. standard 2D texture.  
>>VOLUME | To be added, generally used in conjunction with {content signed}.  
>>CUBE | Cube texture, generally used for skyboxes.  
>  
>{content x} determines the special reading method for input texture, three states exist:
>>default | Default parameter, i.e. color texture.  
>>normals | Normal texture, seems to be able to specify the reading method, to be added.
>>signed | To be added, usually used in conjunction with {type VOLUME}.  
>  
>{filter} unknown, only used in progress.gsm & progress_mask.gsm.
>
>{normal_type} may be deprecated.

Notes:  
* This parameter is displayed on the frontend by default and does not need to be turned on manually, the frontend display is automatically turned off when {file @x} is used
* The 0th input are usually texture coordinates or pre-multiplied texture coordinates, i.e. Vertex/Texcoord
* The content of the 1st Input is unknown, it seems that only the output of the ReflectCube is received here
* Different parameter combinations don't always work together, but I haven't looked into it so far
* Special parameter: @sky_map can probably be used to directly get the sky sphere map of the current scene
---
The second part is the transform parameters, where the camera transform starts with Camera/:  
## Camera/Direction  
```
	{"Camera/Direction"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

>Outputs the world space direction vector of the camera.  

HLSL code implementation：  
>Camera direction = normalize(camera_origin - _in_pos_world);
## Camera/DirectionTangentSpace  
```
	{"Camera/DirectionTangentSpace"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

>Outputs the tangent spatial direction vector of the camera.

HLSL code implementation：  
>Camera direction TS = normalize(_in_view_tangent);
## Camera/Distance  
```
	{"Camera/Distance"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float

>Outputs the distance between the object and the camera in world space.  

HLSL code implementation：  
>Camera distance = distance(camera_origin, _in_pos_world);
## Camera/Origin  
```
	{"Camera/Origin"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

>Outputs the world space position of the camera.  

HLSL code implementation：  
>Camera origin = camera_origin;
## ProjectedPosition  
```
	{"ProjectedPosition"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

>Outputs the projected spatial position of the object.  

HLSL code implementation：  
>Projected position = _in_pos_proj;
## WorldPosition  
```
	{"WorldPosition"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

>Outputs the world space position of the object.  

HLSL code implementation：  
>World position = _in_pos_world;
---
The third part is the vertex parameters, most of which start with Vertex/:  
## Vertex/Color  
```
	{"Vertex/Color"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3
* float

> Vertex color containing the Alpha channel.  
> Output 0 = color, output 1 = Alpha.

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
Output(s)：
* float2

> 2D texture coordinates, i.e. UV.  
>Generally used as input for texture.  
>{set x} specifies which channel's UV to get as input, typically 0.

## Texcoord3D  
```
	{"Texcoord3D"
		{uid 1}
		{position 123 456}
	}
```
Output(s)：
* float3

> 3D texture coordinates, which are not really 3D textures, but texture coordinates for cube texture.  
> Generally used as input for skybox texture.  
---
The fourth section is the scene parameters, which provide information from the environment or system or screen:  
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
Output(s)：
* float
* float

>to be added.
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
Output(s)：
* float (guess)

> Usage unknown, guessing global time  
>{scale x} is simple value scaling, i.e. output=x*SceneTime  
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
Output(s)：
* float

>Scene time  
>Measurement method unknown  
>{scale x} is simple value scaling, i.e. output=x*SceneTime  
## SceneEnvironmentColor 
```
	{"SceneEnvironmentColor"
		{uid 1}
		{position 123 456}
	}
``` 
Output(s)：
* float3

>i.e. environmet/sky/color in the editor

Notes:  
* This is not the color of the skybox, the skybox needs to be [sampled](#reflectcube) by yourself to get it
## WindAmplitude  
```
	{"WindAmplitude"
		{uid 1}
		{position 123 456}
	}
``` 
Output(s)：
* float

>Wind amplitude, the frequency of the wind, reflects the period of time in which the wind goes from small to large and back again.

Notes:  
* This is not the wind speed, which may be recorded as wind_params (hardcoded next to wind_speed), but no further information is available
---
The fifth part is special parameters:  
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
Output(s)：
* Depends on the Material type of the Scheme


> Take a GSM file as input.  
>GSM is most likely short for "GEM Scheme Material", so the Scheme can simply refer to a GSM file.  
>{id x} parameter is necessary, depending on the structure of the code in shader_combinations.set:  
>> Roughly speaking, the nested shaders are compiled in that file in the format "this shader: shader input 1: shader input 2" (the actual order of inputs may be reversed, I haven't looked into it)  
>> So please fill in the ids in order of numbering from 1.
>
> Output pins are strictly aligned with the output pins of the specified material.
# Control Flow
GSM only provides IF as control flow, we will mention other ways to do control flow in [Custom CodeBlock](./custom.md).  
It's worth noting that GPUs are not very good at doing flow of controls, and it's more appropriate to subtly turn them into mathematical calculations or to eliminate them at the compilation stage.  
In GSM, if you want to do two complementary functions, it's better to spread them over two shaders. However if a small amount of control flow is convenient, it's still an acceptable price to pay. 

## If
```
	{"If"
		{uid 1}
		{position 123 456}
	}
```
Input(s)：
* float?
* float?
* float?
* float?

Output(s)：
* float?

>I don't know how this function works, and while it looks a lot like control flow, I haven't guessed exactly how it's going to work yet.

# Toolset
These functions perform a specific set of changes from input to output, often with specific implementations consisting of multiple lines of code.  
They either simplify code writing or provide special functionality.  
I'll introduce parameters in dictionary order.  

---
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
Input(s)：
* Depending on the input parameters

Output(s)：
* Depending on the input parameters

> Combine the channels of the input parameters.  
> Inputs may range from 1-4, 4 has not been used anywhere so not sure if it is really available.

Notes:  
* Not sure, but guessing that only single channel inputs can be combined, if you want to combine something like flot2+float or float3+float you may have to pre-split it
## DiffuseToAO  
```
	{"DiffuseToAO"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3

Output(s)：
* float3
* float

> Calculate AO based on diffuse with pre-baked shadows.  
>Output 0 = diffuse texture  
>Output 1 = AO  
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
Input(s)：
* float4

Output(s)：
* Depends on component type

> Separate channels for input parameters.  
> component ABCD is equal to a.x|a.y|a.z|a.w respectively.

Notes:  
* You can combine the separated channels in the format {components "A B C"}
## Fresnel  
```
	{"Fresnel"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3
* float

Output(s)：
* float3

>I have no idea what this code block is actually doing, its only use is in gem2/water.gsm.
## FromTangent  
```
	{"FromTangent"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3
* float

Output(s)：
* float3

> Convert tangential space normal to world space.  
>Enter 0 = tangential normal  
>Enter 1 = normal strength  

Notice:  
* It implicitly calculates the normal strength
## GammaToLinear  
```
	{"GammaToLinear"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3

Output(s)：
* float3

>Perform a gamma to linear conversion.

HLSL code implementation：  
>out0= pow(a, gamma_value);
## Normalize  
```
	{"Normalize"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float

Output(s)：
* float

>Equivalent to the DX internal function normalize.

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
Input(s)：
* float
* float
* float
* float
* float
* float（？）

Output(s)：
* float3

> Used to perform texture coordinate displacement.  
>Input 0 = single texture coordinates  
>Input 1 = total displacement velocity  
>Inputs 2, 3, 4 = displacement velocity in xyz direction.  
>It seems that there is an input 5 = time? I don't know what it means.

Notes:  
* In general input 234 is defined in properties  
* It looks like you can define fixed parameters for input pins directly from properties? I don't know if this can be generalized to other code blocks
## Parallax
```
	{"Parallax"
		{uid 1}
		{position 123 456}
	}
``` 
>It's complicated, to be added.

## Reflect
```
	{"Reflect"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3
* float3

Output(s)：
* float3

> It seems to be simply the inverse of the DX internal function reflect.  
> Input 0 = input ray, input 1 = normal.

Notes:  
* No samples were used, the inputs and outputs are my guesses according to the hardcoding, I'm not sure if what I found is definitively the implementation code for reflect

HLSL code implementation：  
>out0=-reflect(a, b);
## ReflectCube  
```
	{"ReflectCube"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3
* float3

Output(s)：
* float3

> For calculating reflections.  
>Input 0 = world space direction of the camera, input 1 = ?  
>Output 0 is the reflection angle (?) .

Notes:  
* I'm pretty sure it has two inputs, but the second input is unknown, and all samples do not input the second parameter
* The actual content of the output is unknown, but it is generally connected to input pin 1 of Texture
## Rotate2D  
```
	{"Rotate2D"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float2
* float

Output(s)：
* float2

>May be used to rotate UVs.  
>Input 0 = UV, input 1 = rotation angle.  
>Output 0 is the rotated UV.  

Notes:  
* No samples were used, the inputs and outputs are my guesses according to hardcoding
## SpecularToMetallicRoughness
```
	{"SpecularToMetallicRoughness"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3

Output(s)：
* float
* float

> Convert the input specular map to metallic and roughness.  
> Specific conversion principles to be added, roughly refer to [official documentation](https://bestway-1.gitbook.io/dokumentaciya/tekstury-i-materialy/physically-based-rendering/redaktor-skhem-pbr-i-konversiya-tekstur).  
> Output 0 is metallic, output 1 is roughness.  
## WorldToProjected 
```
	{"WorldToProjected"
		{uid 1}
		{position 123 456}
	}
``` 
Input(s)：
* float3

Output(s)：
* float3

>Converts the input world space coordinates to projected space.

---
Here's a couple extra blocks of code, I saw it in the hardcoding, but I'm not sure what to do with it or if it's actually helpful.
## Desaturate  
> Desaturate? or de-saturate??
## SampleAtlas
# Comment
You can place a block of code containing only comments, called Comments, which I'm sure will be useful in the material editor.

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

>Guess the size parameter determines the size of the block in the 2d view.
# Custom CodeBlock
These two CodeBlocks are used to add custom code, and we elaborate on the use of these two CodeBlocks in [Custom CodeBlock](./custom.md).
## CustomCode  
> Custom code.
## CustomVertexCode  
> Custom vertex shader code.