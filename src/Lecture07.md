
P2     
## Ambient Occlusion

P3   
## AO Off

![](./assets/07-1.png)   

P4   
## AO On

![](./assets/07-2.png)   

P5   
## Ambient Occlusion

- Approximation of attenuation of ambient light due to occlusion    

![](./assets/07-3.png)   

P6    
## Precomputed AO

Using ray tracing to compute the AO offline and store the result into texture, which is widely used in object modeling process   
- Extra storage cost    
- Only apply to static object    

![](./assets/07-4.png)   

P7    
## Screen Space Ambient Occlusion (SSAO)

![](./assets/07-5.png)   

$$
A(p)=1-\frac{Occlusion}{N} 
$$

![](./assets/07-5-1.png)   

- Generate N random samples in a sphere around each pixel p in view space   
- Test sample occlusions by comparing depth against depth buffer   
- Average visibility of sample points to approximate AO   

P8    
## SSAO+

- Recall the AO equation is acutally done on the normal-oriented hemisphere    

![](./assets/07-6-1.png)   

![](./assets/07-6-2.png)   

P11    
## HBAO - Horizon-based Ambient Occlusion

- Use the depth buffer as a heightfield on 2D surface   
- Rays that below the horizon angle are occluded   

![](./assets/07-7-1.png)   

![](./assets/07-7-2.png)   

P12    
## HBAO Implementation

- Use the depth buffer as a heightfield on 2D surface    
- Trace rays directly in 2D and approximate AO from horizon angle   

![](./assets/07-8.png)   

P13    
## GTAO - Ground Truth-based Ambient Occlusion

GTAO introduces the missing cosine factor, removes the attenuation function, and add a fast approximation of multi bounce    

![](./assets/07-9-1.png)   

![](./assets/07-9-2.png)   

![](./assets/07-9-3.png)   

P14    
## GTAO - Ground Truth-based Ambient Occlusion

Add **multiple bounces** by fitting a cubic polynomial per albedo   

![](./assets/07-10-1.png)   

![](./assets/07-10-2.png)   

![](./assets/07-10-3.png)   

P15    
## Ray-Tracing Ambient Occlusion

- Casting rays from each screen pixel using RTT hardware    
- 1 spp(sample per-pixel) works well for far-field occlusion    
- With 2-4 spp, can recover detailed occlusion in contact region   

P16    
## Fog Everything

P17    
## Depth Fog

Linear fog:   
factor = (end-z)/(end-start)   
Exp fog:   
factor = exp(- density\\(\ast \\)z)   
**Exp Squared fog:**    
factor = exp(- (density\\(\ast \\)z)^2)   

![](./assets/07-11.png)   

P18    
## Height Fog

- Height Fog integration along view diretion   

$$
\mathrm{D(h)=D _{max}\cdot e ^{-\sigma \cdot max(h-H _s,0)}}
$$

FogDensitylntegration      

$$
\begin{align*}
 = & \mathrm {D_{\max}\cdot d\int_{0}^{1}e^{-\sigma \cdot \max (v_z+t\ast d_z-H_s,0)}dt} \\\\
  = & \mathrm {D_{\max}\cdot d\cdot e^{-\sigma \cdot \max(v_z-Hs,0)}\frac{1-e^{-\sigma \cdot d_z}}{\sigma \cdot d_z}}
\end{align*}
$$

- Fog color after transmission

FogInscatter=1-exp\\(^{-FogDensityIntegration}\\)    
FinalColor=FogColor\\(\cdot\\) FogInscatter    

![](./assets/07-12.png)   

P19   
## Voxel-based Volumetric Fog

![](./assets/07-13-1.png)   

P20   
## Anti-aliasing

P21   
## Reason of Aliasing

- Aliasing is a series of rendering artifact which is caused by high-frequency signal vs. insufficient sampling of limited rendering resolutions   

![](./assets/07-14.png)   

P22    
## Anti-aliasing

- The general strategy of screen-based antialiasing schemes is using a sampling pattern to **get more samples** and then **weight and sum samples** to produce a pixel color    

$$
p(x,y)=\sum_{i=1}^{n}  w_ic(i,x,y)
$$

![](./assets/07-15-1.png)   

![](./assets/07-15-2.png)   

P23    
## Super-sample AA (SSAA) and Multi-sample AA (MSAA)

- Super sampling is the most straightforward solution to solve **AA**    

![](./assets/07-16-1.png)   

![](./assets/07-16-2.png)   

P24    
##　FXAA (Fast Approximate Anti-aliasing)

Anti-aliasing based on 1x rendered image　　　

－ Find edge pixels by luminance　　　
－ Compute offset for every edge pixel　　　
－ Re-sample edge pixel by its offset to blend with a neighbor　　　

![](./assets/07-17-1.png)   

![](./assets/07-17-2.png)   

P26   
## Edge Searching Algorithm

![](./assets/07-18-1.png)   

![](./assets/07-18-2.png)   

P27    
## Calculate Blend Coefficient

- Compute blender coefficient

**targetP** is the nearer edge end of **CurrentP**    

![](./assets/07-19-2.png)   

![](./assets/07-19-3.png)   

![](./assets/07-19-4.png)   

P28   
## Blend Nearby Pixels

- Compute blender coefficient   

![](./assets/07-20.png)   

**PixelNewColor = Texture(CurrentP_UV + offset_direction * offset_magnitude )**

P29   
## FXAA Result

![](./assets/07-21.png)   


P30   
## TAA (Temporal Anti-aliasing)

Utilize spatial-**temporal** filtering methods to improve AA stability **in motion**   

![](./assets/07-22-1.png)   

![](./assets/07-22-2.png)   

P31   
## TAA (Temporal Anti-aliasing)

![](./assets/07-23.png)   

