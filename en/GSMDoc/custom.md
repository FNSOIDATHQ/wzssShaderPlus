## Catalog
* [Overview](#overview)  
* [Custom Code](#custom-code)
* [Custom Vertex Shader Code](#custom-vertex-shader-code)
* [Return](./menu.md)

# Overview
In this section we'll get into some advanced topics: how to insert HLSL code in GSM.  
Yes, the essence of CustomCode and CustomVertexCode is to insert a shader function written in HLSL into the GSM.
# Custom Code
Custom Code,or CustomCode, is the CodeBlock in which we write the most basic function: it is no different from a function in a common programming language. we need to accepts inputs, and gives outputs. The basic structure is as follows:
```
	{"CustomCode"
		{uid 1}
		{position 123 456}
		{properties
			{comments "temp"}
			{inputs 1}
			{out_type float}
			{code "out0=a;"}
		}
	}
```
* **{inputs x}** | specifies how many inputs the block can have, the number of acceptable inputs is known to be 1-4, the feasibility of more is unknown.
> The parameter names of inputs are always in dictionary order, i.e. parameter 0 = a, parameter 1 = b, and so on  
> I don't know if it's possible to write {inputs 0} or leave the parameter out to make the CodeBlock not accept inputs
* **{out_type x}** | specifies the type of output for the block, although actual HLSL supports outputting multiple parameters in the form of keywords, in GSM custom code the output is unique.
> The output parameter name is always out0  
> The type of the known output parameter can be float float2 float3
* **{code "x"}** | x i.e. the HLSL code and the main part of the custom code.
> You can use any DirectX built-in function directly here, and insertion of any type of code you can think of is also supported:
> > #define?#include? No problem! CodeBlocks as they are  
>
> But it's important to note that the implementation of the custom code is a function, which means that everything you define works only in the scope of the function  

The CodeBlock **doesn't support** line breaks! I'm not kidding, you can explicitly type \r\n to create a carriage return, but not a real one, the former will be read by the material editor to convert it to a carriage return when visualized, but with the current text manipulation, it seems to be impossible (there may be some way to convert it, but I don't know it yet).

Other useful information:
* I encourage developers to write comments in CustomCode whenever possible, whether in the form of comments or ; or in-code comments (although this may have limited readability)
* Want to have fun writing HLSL code using #include? You can happily use line breaks in included code. No problem, the root directory of the code specified at compile time is in shader\material.
> If you've seen shaders for old GEM engine games, it's not hard to guess the directory ;)
* Flow controls can be easily created in custom code, the recommended methods are the traditional if-else and ternary operators. Examples are as follows
```
out0=c;if(a>0.5f){out0=b;}  
out0=(a>0.5f)?b:c;
```
> Avoid creating a full if-else if possible, but don't worry too much, modern GPUs aren't that fragile
# Custom Vertex Shader Code
Custom vertex shader code, or CustomVertexCode, is a special variant of custom code. As the name suggests, it is used to control vertex shaders and is formally quite different from custom code. Its basic structure is as follows:
```
	{"CustomVertexCode"
		{uid 1}
		{position 123 456}
		{properties
			{type "After Transform"}
			{code "_out_pos.xyz=float3(0.0f,0.0f,0.0f);"}
		}
	}
```
First of all, it **probably** can't define inputs, that's not necessarily true, but it probably is.  

{type "x"} defines where the function is located:  
* **After Transform** | after the default vertex shader is executed.
* **Before Transform** | before the default vertex shader is executed.

The custom vertex shader code then almost always connects directly to the transform pins, which is completely different from the regular block of code, so we can't easily describe its output.  
According to my guess, this CodeBlock is simply inserting the code inside the main function of the vertex shader.

So, I think we will directly manipulate the variables in the main function in this section, the known variable names, data types and possible meanings are as follows:  
| _out_pos | _out_tc_3d | _out_haze |
| -------- | -------- | -------- |
| float4 | float3 | float |
| World Space Vertex Position | 3D Texture Coordinates | Battlefield Haze |

It's not hard to guess that there are also:
* _out_pos_proj | float4 | Projection Space Vertex Position
* _out_tc | float3 | Texture Coordinates

But I didn't delve into this section, it's too difficult to study them without source code.