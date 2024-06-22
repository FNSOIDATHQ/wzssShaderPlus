## Catalog
* [Appendix 1 Automatic Shader Cache Clearing](#appendix-1-automatic-shader-cache-clearing)
* [Return](./menu.md)

# Appendix 1 Automatic Shader Cache Clearing
There are two .bat files in the mod root directory:.  
* ClearShaderCache.bat
* ClearShaderCacheMOWIIOnly.bat

Their original code comes from [Delete your shader cache to stop crashing](https://steamcommunity.com/app/553850/discussions/0/4295944344102442951/), thanks to @ DarthPotato for his work.  

ClearShaderCache.bat deletes all shader caches on your computer (except for the extra ones stored for specific games).  
ClearShaderCacheMOWIIOnly.bat will only delete MOW II's cache files stored separately in AppData\Local\Men of War II\shader_cache.  

Please run ClearShaderCacheMOWIIOnly.bat at least once after mod update, basically this will clear up any problems with MOWII due to inaccurate judgments causing shaders to not compile successfully.  

If the game throws strange errors, try running ClearShaderCacheMOWIIOnly.bat first, and if the problem persists try running ClearShaderCache.bat.  
If the problem still persists run all of them and the problem still occurs, send me feedback about the problem anywhere! I would like to thank you in advance for your help.
