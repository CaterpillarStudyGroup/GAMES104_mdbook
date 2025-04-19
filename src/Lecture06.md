P5   
## Terrain Rendering

> 地形系统

P8    
[Simple Idea - Heightfield](https://caterpillarstudygroup.github.io/GAMES101_mdbook/TerrainRendering/HeightField.html)

P27    
## Mesh Shader Pipeline 

- Amplification Shader Stage - decides how many Mesh shader groups to run and passes data to those groups   
- Mesh Shader Stage - produces a semi-regular tessellation pattern for each patch, and outputs comprise vertices and primitives    

![](./assets/06-11.png)   

> 只持 DX12 及以上。    
可以基此实现动态高度的地形效果。  

P45    
## Virtual Texture

[Virtual Texture](https://caterpillarstudygroup.github.io/GAMES101_mdbook/TerrainRendering/PaintTerrainMaterials.html)

P46   

> 这个方法涉及 GPU、内存、硬盘之间切换。    

![](./assets/06-22-1.png)   

新显卡的方式：

- DirectStorage: 硬盘数据只是从内存过一下，到 GPU 才解压，提升传输效率。  

![](./assets/06-22-2.png) 

- DMA：硬盘直接往 GPU 写数据。   

![](./assets/06-22-3.png)   

## Production Friendly Quick Sky and Atmosphere Rendering

Simplify Multi-scattering Assumption   
- Scattering events with order greater or equal to 2 are executed using an **isotropic phase function**   
- All points within the neighborhood of the position we currently shade **receive the same amount of second order scattered light**   
- **Visibility is ignored**     

$$
G_{n+1}=G_n\ast f _{ms}
$$

$$
\mathbf{ F_{ms}=1+f_{ms}+f^2_{ms}+f^3_{ms}+\dots = \frac{1}{1-\mathbf{f_{ms}} } }
$$

$$
\mathbf{\Psi _ {ms} }=\mathbf{L_ {2^{nd}order} F_ {ms} }
$$

> 假设“散射是各向同性的”。那么，“均匀的入射光到均匀的出射光”的过程，只是一个简单的能衰减过程。所以只需要求出衰减比例，每 bounce 一次就按这个比例衰减就可以了。    

P83 

Fixed view position and sun position to remove 2 dimensions out of LUT   

![](./assets/06-40.png)  

> 对上文中的 LUT 的简化：    
(1) 假设人所在的高度不变，去掉 height 维    
(2) 假设太阳位置不变，去掉入射角的维度仅留下出射光的维度(天顶角、环角)     

P84    

- Generated a 3D LUT to evaluate aerial-perspective effects by ray marching    

![](./assets/06-41.png)  

> 这个方法不保证物理正确，但好处是：    
(1) 艺术家友好      
(2) 可以创造异星世界效果     
(3) 硬件友好     

P85    
## Good Balance of Performance and Effect

- Scalable from mobile to high-end PCs

![](./assets/06-42.png)  

Performance for each step of method, as measured on PC (NV 1080) and a mobile device (iPhone 6s)    

P87   
## "Paint" Cloud

P88    
## Cloud Type

![](./assets/06-43.png)  

P89   
## Mesh-Based Cloud Modeling

![](./assets/06-44.png)  

**Pros**   
- High quality   

**Cons**   
- Overall expensive   
- Do not support dynamic weather   

> Mesh＋腐蚀等算法。    
现在已经没人用了。      

P90   
## Billboard Cloud

**Pros**   
- Efficient   

**Cons**   
- Limited visual effect   
- Limited cloud type    

![](./assets/06-45.png)  

> 半透明插片＋\\(\alpha \\) 混合    
十年前常用     

P91    
## Volumetric Cloud Modeling

![](./assets/06-46.png)  

**Pros**    
- Realistic cloud shapes    
- Large scale clouds possible    
- Dynamic weather supported    
- Dynamic volumetric lighting and shadowing   

**Cons**   
- Efficiency must be considered    

> 优点：(1) 全动态，CPU 实时生成。(2) 云可以表现出很多变化    
局限性：(1) 复杂 (2) expensive    

P92    
## Weather Texture

![](./assets/06-47-1.png)  

![](./assets/06-47-2.png)  

>  texture＋厚度 channel    
对 texture 做挠动可以产生云的变化。     

P93   
## Noise Functions

![](./assets/06-48-1.png)  

![](./assets/06-48-2.png)  

P94    
## Cloud Density Model

![](./assets/06-49.png)  

>  用低频 noise 雕刻出造型，再用高频加上细节。    

P95    
## Rendering Cloud by Ray Marching

![](./assets/06-50.png)  

> 不会把云转成 Mesh 去渲染，而是当作大气来渲染。     
但由于云的通透性很低，可以对公式作大量假设和简化。     