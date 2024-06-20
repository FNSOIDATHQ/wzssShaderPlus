
        不知道为什么在github没法跨网页做标题导航，我暂时只能在这里把mtl结构再写一遍。
## 目录
*  [MTL文件结构](#mtl文件结构)
*  [返回](./menu.md)

# MTL文件结构
```
{"npr/simple" 
	;贴图参数
	;如果你不需要对应的贴图，使用占位符替代
	;在 $/pbr 有三个占位符 0_ao=环境光遮蔽 0_nm=法线 0_met=金属度
	{diffuse "turret_Base_Color"}
	


	;可选参数
    {diffuseColor 0xffffff}
	{opacity 1}
	{blend none};none=不透明 blend=半透明 test=剔除 add=?
}
```
与旧GEM引擎的MTL相比变化不大，主要参数类型为：
* {xxx "tex"} 贴图参数
* * 采用写入MTL模式控制着色器时，会导致不一致行为：必须预定义全部贴图参数，否则调用着色器时引擎会反馈一个意义不明的故障
* {xxx 0xffffff} 16进制颜色参数
* {xxx 1} 浮点参数
* {blend none} 透明计算方法
* 待补充