## 目录
*  [附录1 自动清除着色器缓存](#附录1-自动清除着色器缓存)
*  [返回](./menu.md)

# 附录1 自动清除着色器缓存
在mod根目录有两个.bat文件:  
* ClearShaderCache.bat
* ClearShaderCacheMOWIIOnly.bat

它们的原始代码来自[Delete your shader cache to stop crashing](https://steamcommunity.com/app/553850/discussions/0/4295944344102442951/)，感谢 @DarthPotato 的工作。  

ClearShaderCache.bat会删除你电脑上所有的着色器缓存（除了特定游戏额外存放的）。  
ClearShaderCacheMOWIIOnly.bat只会删除MOW II单独存储在AppData\Local\Men of War II\shader_cache中的缓存文件。  

在mod更新之后，请至少运行ClearShaderCacheMOWIIOnly.bat，基本上来说这会清除MOWII因为判断不准确导致着色器未能成功编译导致的问题。  

如果游戏产生奇怪的报错，可以尝试首先运行ClearShaderCacheMOWIIOnly.bat，如果问题仍然存在尝试运行ClearShaderCache.bat。
如果全部执行依旧出现问题，在任意地方向我反馈这个问题！我在此提前感谢您的帮助。

