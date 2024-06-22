
        I don't know why I can't do header navigation across pages at github, so I'll just have to rewrite the mtl structure here for now.
## Catalog
* [MTL File Structure](#mtl-file-structure)
* [Return](./menu.md)

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
* {xxx "tex"} texture parameter
* * Using write MTL mode to control shaders leads to inconsistent behavior: all texture parameters must be predefined, otherwise the engine will call the shader with an unspecified fault.
* {xxx 0xffffff} hexadecimal color parameter
* {xxx 1} floating point parameter
* {blend none} Transparency calculation method
* To be added