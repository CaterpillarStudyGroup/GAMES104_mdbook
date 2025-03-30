
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


