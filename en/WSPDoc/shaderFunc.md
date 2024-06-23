## Table of contents
* [Overview](#overview)
* [PBR+](#pbr)
* * [standard](#standard)
* * [standardComb](#standardcomb)
* * [standardCombMask](#standardcombmask)
* [npr](#npr)
* * [simple](#simple)
* [Return](./menu.md)

# Overview
The new shaders in WSP are divided into two main parts: PBR+ (Physics-Based Rendering+), and NPR (Non-Realistic Rendering).  
PBR+ focuses on adding new features to physical rendering, such as emissive texture support, and adding developer-oriented QoL functions, such as improved AO rates and multi-channel textures support.  
NPR, on the other hand, focuses on adding non-realistic rendering techniques based on PBR, and is currently playing catch-up with the technical features of the Unity Toon Shader.  
We describe the characteristics of each shader based on the shader type. The shader name is not translated, and still keep the original English name for easy comparison.  

Notes:
* In order to unify the format, we use the camel case for all the naming, so the parameter names of some existing features have been changed, these parameters are not specifically described, just change the name according to the reference file.

# PBR+
## standard
Standard PBR+ shader, contains all added features.  
mtl reference: [standard.gsm](./mtl/pbrPlus/standard.mtl)

**Features**:  
1. ### Emissive  
We implement (and actually pass to the MOW II interface) the standard emissive feature.

Parameter description:
> **{emissive "X"}** | Specifies the emissive texture  
> **{emissiveRate 0}** | Indicates the emissive intensity, can be any value  
> **{emissiveColor 0xff0000}** | Indicates the emissive color  
> **{emissiveType 0}** | Toggle the emissive type, use emissiveColor as emissive color only when it is 0, and read the texture when it is 1  

2. ### Improved AO  
Now aORate is no longer expressed in a strange way: in the original method AO=0 the model is completely black, and AO=1 reads the AO texture normally.  
This is obviously not what we expect, after rewriting the function: 0 = no AO effect, 1 = read AO normally, 2 = model all black.  
The new parameter calculation method is more intuitive, and at the same time do not need to adjust the aORate greatly to simulate no AO.

Parameter description:
> **{aoRate 1}** | Improve AO, 0=no AO 1=standard AO 2=all black AO  
## standardComb
Multi-channel texture shader, no longer need to configure a separate file for each PBR texture, also support some QoL features.

mtl reference: [standardComb.gsm](./mtl/pbrPlus/standardComb.mtl)

**Features**:  
1. ### Multi-channel texture  
We implement a multi-channel texture containing roughness, metallicity and normals, only AO is still provided by a single texture.  

Parameter description:
> **{combine "X"}** | red channel = roughness, blue channel = metallic, green channel = normal texture green channel, alpha channel = normal texture red channel  

2. ### Improved AO  
Now aORate is no longer expressed in a strange way: in the original method AO=0 the model is completely black, and AO=1 reads the AO texture normally.  
This is obviously not what we expect, after rewriting the function: 0 = no AO effect, 1 = read AO normally, 2 = model all black.  
The new parameter calculation method is more intuitive, and at the same time do not need to adjust the aORate greatly to simulate no AO.

Parameter description:
> **{aoRate 1}** | Improve AO, 0=no AO 1=standard AO 2=all black AO 

3. ### Normal texture reading  
Support reading normal textures in both OpenGL or DirectX format (MOW II defaults to DirectX format), no longer need to manually convert or use two textures to determine if the normals are inverted.

Parameter description:
> **{bumpType 0}** | 0=DirectX 1=OpenGL  

Notes:
* This function can actually be realized by setting the bumpRate parameter to -1, but it is more intuitive to add an additional parameter.

4. ### Smoothness texture reading  
In some cases a smoothness texture is used instead of a roughness texture, smoothness being the inverse of roughness.

Parameter description:
> **{roughnessFlip 0}** | 0=No inverse 1=Inverse.  

## standardCombMask
Multi-channel texture shader with additional support for masking.  

mtl reference: [standardCombMask.gsm](./mtl/pbrPlus/standardCombMask.mtl)

**Features**:  
1. ### All [standardComb](#standardcomb) supported features  
I won't repeat the description here.

2. ### Diffuse masking  
Supports adding an extra texture to mask the base color, e.g. it can be used to create variable aging, freely adjusting the aging level of the carrier.

Parameter description:  
> **{mask "X"}** | The mask texture, where the RGB channel is the color to be masked on, and the Alpha channel is the mask intensity.  
> **{maskRate 1}** | mask rate 0=no mask 1=standard mask 2=fully use mask as base color  
# NPR
One or some samples of NPR usage are included in wzssShaderPlusLite\scene\entity\nprSample, as this is a completely new area for MOW II, and I will be using some of the models to show specific parameter choices.  
Thanks to @晓美烟 for providing the models.

## simple
A simple implementation of the NPR shader, containing what can be done so far.  

mtl reference: [simple.gsm](./mtl/npr/simple.mtl)

**Features**:  
1. ### Emissive  
We implement (and actually pass to the MOW II interface) the standard emissive feature.

Parameter description:
> **{emissive "X"}** | Specifies the emissive texture  
> **{emissiveRate 0}** | Indicates the emissive intensity, can be any value  
> **{emissiveColor 0xff0000}** | Indicates the emissive color  
> **{emissiveType 0}** | Toggle the emissive type, use emissiveColor as emissive color only when it is 0, and read the texture when it is 1  

2. ### Outline  
We have implemented a basic outline, which is most likely **only** suitable for character outlines, or other models with a lot of curves. Unfortunately, due to code access limitations, we most likely won't be able to implement any of the other mainstream outline methods in the existing supported features.

Parameter description:
> **{outlineRate 0.1}** | Outline width  

3. ### Diffuse tone separation  
We have implemented a standard tone separation method to slightly improve the styling of textures that were not originally designed for NPR. Note that this feature is often ineffective and is recommended to be used at your discretion.

Parameter description:
> **{colorScale 8}** | Number of color scales  
> **{toonRate 0.5}** | The mixing ratio of the color separation texture and the original texture, 0=original texture 1=tone texture  
> **{lightup 0}** | Increase the overall brightness of the diffuse texture, if you find the color is too dark after tone separation, you may consider enabling it  