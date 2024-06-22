## Catalog
* [Overview of the GSM File Structure](#overview-of-the-gsm-file-structure)
* [CodeBlock Basic Structure](#codeblock-basic-structure)
* [Link Statements](#link-statements)
* [Input/Output](#inputoutput)
* [MTL File Structure](#mtl-file-structure)
* [Other Basics](#other-basics)
* [Return](./menu.md)

# Overview of the GSM File Structure
All GSM files are stored in the **"set\material\lib"** directory, where subfolders have no particular significance to the code itself, and can be created with any name as preferred.  
**"Men of War II\modeling\resource\set\material\lib"** stores the official GSM code as reference.

The standard GSM file is shown below

```
;This is a comment
{"Material"
	{"CodeBlockX"
		{uid a}
		{position xxx xxx}
	}
  	{"Material"
		{uid b}
		{position xxx xxx}
		{properties
			{lighting_model metallic_roughness}
		}
	}
  
  {l1 a b}; link statement
}
```
I'm not going to teach MOW II script syntax from scratch here, see [Basic knowledge about configuration files | GEM RTS v1 | Documentation (gitbook.io)](https://bestway-1.gitbook.io/documentation/foundational-knowledge/basic-knowledge-about-configuration-files)  
So let's start directly with the structure of the GSM itself:  
  
If we call what a curly brace {} includes a **CodeBlock** (I'll explain why I call it that later), then  
> A GSM file always consists of a CodeBlock with the name "Material", which we call the main block;
>
> A GSM file must contain a sub-block with the same name "Material" in the main block, which is the **Output Block** of the whole GSM. All possible outputs are summarized in this block;
>
> A GSM file, in most cases, contains more than one mutually independent sub-block, each of which must have >= 1 output parameter and >= 0 input parameter, which(the parameters) we will refer to in the following as **Pins**;
>> If there is more than one sub-block, each (needed) output of the current block is connected to the input of the next using a **Link Statement** between them.

# CodeBlock Basic Structure
Why is it called a CodeBlock? Because the GSM file is actually converted to HLSL code at runtime and then added to the rest of the shader code to be compiled together. A CodeBlock essentially represents one or more lines of HLSL statements. The details of this process are described in [Appendix 1 GSM Code Compilation Process](./other.md#appendix-1-gsm-code-compilation-process).  
In addition, each CodeBlock is really displayed as a block in the material editor.

The structure of a CodeBlock is shown below
```
    {"CodeBlock"
        {uid a}
        {position xxx xxx}
        {properties
            {comments "tihs is a comment"}
            {m n}
        }
    }
```
It contains several main sections:
* **"CodeBlock"** | the type of the CodeBlock
* **uid** | The unique identifier of the CodeBlock, the uid **can't be duplicated in a single GSM file** and is used to index the corresponding CodeBlock in the link statement.
* **position** | the position of the block in the 2d visualization window of the material editor.
* **properties** | The parameters of this CodeBlock, each parameter is represented as {m n}, m is the parameter name and n is the parameter.  
    * This section is optional.
    * where comments is a special parameter that will be treated as a comment, similar to a direct ; symbol comment. I guess using this way comments can be displayed in the material editor.
    * It might be possible to define the values of input parameters directly through properties, see [Example of a Pan block](./CodeBlock.md#pan)

In addition, except for [a few cases](./custom.md) the inputs and outputs of CodeBlocks are not directly displayed.  
The specific available CodeBlock types are listed in [here](./CodeBlock.md).

# Link Statements

GSM supports two types of pin links between CodeBlocks
```
    {l1 a b}
    {l2 a x b y}
```
l1 = simple pin link  
{l1 a b} = use the 0th output of block **a** as the 0th input of block **b**  
l2=specific pin link  
{l2 a x b y} = use the **x**th output of block **a** as the **y**th input of block **b**  
Pins are counted from 0  

You might see this in the actual official code
```
    {l1 a b m n o}
    {l2 a x b y m n o}
```
but unfortunately I don't yet know what the significance of the extra parameters m n o etc. is.  
A likely guess is that these parameters are being used in the material editor: to define some kind of anchor point that fixes the visualization link lines or something.  
In any case, ignoring these parameters is currently harmless as far as I can observe.

# Input/Output
For now, you can **assume** that the inputs to the GSM are all introduced by the CodeBlock, so we'll implement a function in the GSM that takes input parameters from the CodeBlock, performs various calculations and ultimately sums them up in the output block.  
Note that this assumption is **erroneous**, and we'll make it clear in [Built-in helper functions and variables](./helper.md) and [custom CodeBlocks](./custom.md) sections mention what does not fit this assumption.  
But at the moment, there is no problem with thinking and writing code this way.

The inputs are varied, and we'll elaborate them in [available CodeBlock types](./CodeBlock.md). Here we will focus only on the output of GSM, i.e., knowledge about output blocks.  
Output blocks enable the GSM to support the following four different types of output by defining the lighting_model string in the properties:
* no_parameters
* phong
* phong_gem2
* metallic_roughness
* marschner

* Note that  
    * It is possible to output properly without parameters, and the five lighting model types define different output pins.
    * marschner is most likely an uncompleted light model for an anisotropic hair shader, whose literal points to the paper [《Light Scattering from Human Hair Fibers》](https://www.graphics.stanford.edu/papers/hair/hair-sg03final.pdf), which I won't go into too much detail here
    * phong and phong_gem2 are traditional light models based on empirical formulas; gem2 may have some more advanced features, but I have not delved into the differences. Similarly, I have not documented the pin types of these lighting models, which will be added later on
    * metallic_roughness is the metallic-roughness based PBR lighting model, which we'll refer to as the PBR or PBR model for short, and that's the next key thing to focus on

Refer to this figure  
<img src=../../img/mted.png width=50% />  
The pinout comparison table for the PBR model is as follows   
| diffuse | emissive| opacity | normal | transform | translucence | roughness | metallic | shadow | fresnel |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 0     | 1     | 2     | 5     | 6     | 8     | 9     | 20     | 21     | 22     |

The table means that if you set the link statement {l2 a 0 output block 2}, it will output pin 0 of **a** to opacity  
The specific meaning of each pin is described next:

### diffuse
Data type: float3  
The diffuse pin, generally outputs data as information containing three channels of RGB color.  
It is used to display the most basic inherent color of the object.  
### emissive
Data type: float3  
The emissive pin, it can be interpreted as outputting a diffuse map which is superimposed on the diffuse map and is not affected by the ambient lighting, since the MOW II emissive map does not participate in the global lighting calculation.  
### opacity
Data type: float  
Transparency pin, note that it is only responsible for outputting transparency, the transparency calculation is still controlled by the {blend X} parameter in mtl.
### normal
Data type: float3  
Normal pin, output the world space normal(probably, not sure).
### transform
Vertex transform pin, outputs changes to the vertex information.  
This pin is too special to briefly describe its data type, which we'll mention its specific use in [custom CodeBlock](./custom.md). Normally there is no need to connect this pin, the rest of the shader will do the general flow involved with this pin.  
### translucence
Data type: float3  
Translucence pin  
I don't have an in-depth understanding of this pin, see [Translucent Shader
](https://help.autodesk.com/view/3DSMAX/2022/ENU/?guid=GUID-67CD32E8-A0D5-4A14-8179-FB11D3E3DD28) for the general effect.  
Note that it is most likely not related to subsurface scattering.  
It is not usually necessary to connect this pin.  
### roughness
Data type: float  
Roughness pin, the higher the value the rougher the surface of the object, and vice versa the smoother it is.  
Occasionally we refer to another term, smoothness, which is the inverse of roughness.  
### metallic
Data type: float  
The metallic pin. The higher the value, the more metallic the surface is, and the less smooth it is.
### shadow
Data type: float  
Should actually be called Ambient Occlusion, and incomplete change the shadow.  
The larger its value, the heavier the shadow.
### fresnel
Data type: float  
It represents the value of f0 in PBR, the meaning of f0 can be found in [Material creation parameter F0](https://liangz0707.github.io/whoimi/blogs/Art/%E6%9D%90%E8%B4%A8%E5%88%B6%E4%BD%9C%E5%8F%82%E6%95%B0F0.html), its related to the refractive index of the object.  
If you don't know what it is, don't connect that pin, the shader will call a default value.

# MTL File Structure
```
{ "npr/simple"
	;texture parameters
	;If you don't need the corresponding mapping, use placeholders instead
	;There are three placeholders in $/pbr 0_ao=ambient light masking 0_nm=normal 0_met=metallicity
	{diffuse "turret_Base_Color"}
	


	;Optional parameters
    {diffuseColor 0xffffff}
	{opacity 1}
	{blend none};none=Opaque blend=Semi-transparent test=Cull out add=?
}
```
Not much has changed from the MTL of the old GEM engine,  
the main parameter types are:
* {xxx "tex"} texture parameter [[bugs that must be watched out for]](#known-issues)
* {xxx 0xffffff} hexadecimal color parameter
* {xxx 1} floating point parameter
* {blend none} Transparency calculation method
* To be added
# Other Basics

### Types of shaders actually controlled by the GSM
We can control vertex shaders, and early stages of pixel shaders through the GSM.  
This is largely a guess, as I don't have access to the actual shader code, but there is a basis for thinking so:
* Because of the transform pin, we have full control over the vertex shader without any problems
* We don't seem to have access to the actual lighting computation stage, to get information about the light source, to control shadows, light reflections or any of the actual pixel processing stages.
    * In reality we just compute the necessary information and drop it into a black box.

### The disadvantages of GSM
Specifically, I'll tell you what GSM can't do:
* There's no way to define new output pins. We can't pass new information into the black box.
* Obviously, there's no access to the post-processing stage at all.
* No access to the surface subdivision shader, which theoretically appears in shader_combinations.set, meaning we might have access. But I haven't found the code available anywhere. This information is most likely hardcoded in the executable file
* There are no multi-PASS calculations (or maybe I'm just missing the point), meaning that some actually widely used graphics methods can't be implemented
* No access to shader parameters from the rest of the engine, you can't access shader parameters via trigger or MOW style SDL or anything else, except for manual tweaking in the editor or writing to an MTL file
* Parameters have no range control: yes, we can normalize numbers through math, but this is all hidden on the frontend. Also the only available parameter types are float and its variants, although you can actually use other data types inside a CodeBlock.
* Lack of clear hints of faults, most of them are generated during the compile phase, when the code has been compiled into a completely different HLSL code, and identifiers such as name or uid have been erased, which makes it difficult to track down the problem.

### Known Issues
* Using write MTL mode to control shaders leads to inconsistent behavior: all texture parameters must be predefined, otherwise the engine will call the shader with an unspecified fault.