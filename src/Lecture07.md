
P34   
## Post-process

Post-process in 3D Graphics refers to any algorithm that will be applied to the final image. It can be done for stylistic reasons (color correction, contrast, etc.) or for realistic reasons (tone mapping, depth of field, etc.)    

![](./assets/07-24.png)   

P35   
## Bloom Effect

P36   
## What is Bloom

- The physical basis of bloom is that, in the real world, lenses can never focus perfectly    
- Even a perfect lens will convolve the incoming image with an <u>**Airy disk**</U>    

![](./assets/07-25.png)   

P37   
## Detect Bright Area by Threshold

![](./assets/07-26.png)   

Find Luminance (Y) apply the standard coefficients for sRGB:    

$$
Y=R_{lin}\ast 0.2126+G_{lin}\ast 0.7152+B_{lin}\ast 0.0722
$$

> 取出非常亮的部分，做与 5\\(\times \\)5 高斯 blur。    

P38   
## Gaussian Blur   

![](./assets/07-27.png)   

P39    
## Pyramid Guassian Blur

![](./assets/07-28.png)   

We can't do all that filtering at high resolution, so we need a way to **downsample** and **upsample** the image Need a weight coefficient to tweak final effect   

> 在低精度图上 blur 再放大，可以得到大区域的 blur 效果同时较小的计算量。   

P40   
## Bloom Composite

![](./assets/07-29.png)   

P41   
![](./assets/07-30.png)   

P42   
## Tone Mapping

> 真实世界的亮度 range 非常大，如果曝光没调好，会出现亮部过亮或暗部过暗的效果。    

P43   
## Tone Mapping

- No way to directly display HDR image in a SDR device    
- The purpose of the **Tone Mapping** function is to map the wide range of high dynamic range (HDR) colors into standard dynamic range (SDR) that a display can output    

![](./assets/07-31.png)   

> 用一条曲线把 HDR 映射到 SDR。   
filmic curve 是一个拟合出来的所项式曲线。      

P45   
## ACES

- **A**cademy **C**olor **E**ncoding **S**ystem    
  - Primarily for Film & Animation    
  - Interesting paradigms and transformations   
- The useful bits   
  - Applying Color Grading in HDR is good   
  - The idea of a fixed pipeline up to the final OTD transforms stage is good   
     - Separates artistic intent from the mechanics of supporting different devices   

![](./assets/07-32-1.png)   

> ACES 曲线不但效果更好，还可以通注增加一个后处理，无差别适配到任何终端。    

P46    
## HDR and SDR Pipeline

- Visual consistency between HDR / SDR   
- Similar SDR results to previous SDR color pipeline   
- High quality   
- High performance   
- Minimal disruption to art teams   
  - Simple transition from current color pipeline   
  - Minimal additional overhead for mastering HDR *and* SDR   

![](./assets/07-33.png)   

P47    
## Tone Mapping Curve Comparison

![](./assets/07-34-1.png)   
![](./assets/07-34-2.png)   

P48   
## Color Grading

P49   
## Lookup Table (LUT)

- LUT is used to remap the input color values of source pixels to new output values based on data contained within the LUT    

- A LUT can be considered as a kind of color preset that can be applied to image or footage    

![](./assets/07-35.png) 

> 用一个表格实现从原始色相空间到目标色相空间的映射。    

P53    
## Rendering Pipeline

P59   
## Rendering Pipeline

- **Rendering pipeline** is the management order of all rendering operation execution and resource allocation    

![](./assets/07-36.png) 

P60   
## Forward Rendering

for n meshes   
\\(\quad\\) for m lights    
\\(\quad \quad\\)color += shading(mesh, light)    

P61    
## Sort and Render Transparent after Opaque Objects

![](./assets/07-37.png) 

> 透明物质必须最后绘制。    
多个透明物质则由远及近绘制，因为不同绘制顺序产生的结果是不一样的。   
透明物体的排序很容易引起各种 BuG。    
十几年前的主流 Pipeline。    

P64    
## Deferred Rendering

![](./assets/07-38-1.png)   

![](./assets/07-38-2.png) 

![](./assets/07-38-3.png) 

![](./assets/07-38-4.png) 

> 由于光的种类非常复杂，引入延迟渲染技术，即先绘制物体，再考虑与光的关系。   
近十年最主流的 Pipeline.    

P65    
## Deferred Rendering

**Pros**   
- Lighting is only computed for visible fragments     
- The data from the G-Buffer can be used for post- processing   

**Cons**   
- High memory and bandwidth cost     
- Not supporting transparent object     
- Not friendly to MSAA    

![](./assets/07-39.png)   

P66   
## Pilot Engine Deferred Rendering

![](./assets/07-40.png)   

P67    
## Tile-based Rendering

![](./assets/07-41-1.png)     

![](./assets/07-41-2.png)     

![](./assets/07-41-3.png)     

> 这个 pipeline 用于移动端。因为移动端最关心发热问题。    
DRAM 存储大、速度慢、功耗高。On-chip 中的 SRAM 则相反。    
因此，把整个 G-buffer 切成小的 tile 在 SRAM 计算，算好存成framebuffer。    

P68   
## Light Culling by Tiles

![](./assets/07-42.png)     

P69    
## Depth Range Optimization

- Get Min/Max depth per tile from Pre-z pass    
- Test depth bounds for each light   

![](./assets/07-43.png)     

> tile-based 是现代引擎的主流方案。    
tile 的额外好处是简化光的计算。    

P71    
## Forward+ (Tile-based Forward) Rendering

- Depth prepass (prevent overdraw / provide tile depth bounds)     
- Tiled light culling (output: light list per tile)   
- Shading per object (PS: Iterate through light list calculated in light culling)    

P72   
## Cluster-based Rendering

![](./assets/07-44.png)     

> 对 Z 空间也做切分。一个小块称为 cluster。    

P73    
## Visibility Buffer

![](./assets/07-45.png)     

> 几何信息 (V-Buffer) 和材质信息 (G-Buffer) 剥离开。   
因为现在的几何越来越复杂，甚至几何密度超过像素密度。    
这是现代引擎的发展方向。     

P74   
![](./assets/07-46.png)     

P75   
## Challenges

- Complex parallel work needs to synchronize with complex resource dependency    
- Large amount of transient resource whose lifetime is shorter than one frame    
- Complex resource state management     
- Exploit newly exposed GPU features without extensive user low level knowledge    

P76   
## Frame Graph

A Directed Acyclic Graph (DAG) of pass and resource dependency in a frame, not a real visual graph    

![](./assets/07-47.png)     

> Frame Graph 是未来重要的发展方向。    

P77    
## Render to Monitor

P78    
## Screen Tearing

![](./assets/07-48.png)     

P79    
## Screen Tearing

In most games your GPU frame rate will be highly volatile    
When new GPU frame updates in the middle of last screen frame, screen tearing occurrs       

![](./assets/07-49.png)     


P80    
## V-Sync Technology

Synchronizing buffer swaps with the Vertical refresh is called V-sync    
V-Sync can be used to prevent tearing but framerates are reduced, the mouse is lagging & stuttering ruins gameplay   

![](./assets/07-50.png)     

P81    
## Variable Refresh Rate

![](./assets/07-51.png)     


