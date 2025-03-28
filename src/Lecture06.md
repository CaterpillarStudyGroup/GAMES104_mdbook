
P8    
## Simple Idea - Heightfield

![](./assets/06-1.png)   

P10   
## Render Terrain with Heightfield

![](./assets/06-2.png)   

P12    
## Adaptive Mesh Tessellation

![](./assets/06-3.png)   

P13    
## Two Golden Rules of Optimization

**View-dependent error bound**   
- Distance to camera and FoV   
- Error compare to ground truth (pre-computation)   

![](./assets/06-4.png)   

P14   
## Triangle-Based Subdivision

![](./assets/06-5.png)   

P15   
## Subdivision and T-Junctions    

Continuously partitioning triangles and their    
children based on the idea of binary trees     

![](./assets/06-6.png)   

P17    
## QuadTree-Based Subdivision

Pros    
- Easy to construct     
- Easy management of data under geospatial, including objects culling and data streaming     

Cons    
- Mesh subdivision is not as flexible as triangle mesh    
- The grid level of the leaf nodes needs to be consistent   

![](./assets/06-7.png)   

P19    
## Solving T-Junctions among Quad Grids

![](./assets/06-8.png)   

P21    
## Triangulated Irregular Network (TIN)

![](./assets/06-9.png)   

P23    
## Triangulated Irregular Network vs. Adpative Tessellation

Pros   
- Easy in runtime rendeirng   
- Less triangls in certain terrain types   

![](./assets/06-10-1.png)   

Cons    
- Requires certain pre-processing steps   
- Poor reusability   

![](./assets/06-10-2.png)   

GDC2021 Boots on the Ground: The Terrain of Call of Duty      

P27    
## Mesh Shader Pipeline 

- Amplification Shader Stage - decides how many Mesh shader groups to run and passes data to those groups   
- Mesh Shader Stage - produces a semi-regular tessellation pattern for each patch, and outputs comprise vertices and primitives    

![](./assets/06-11.png)   

P30    
Non-Heightfield Terrain

P31   
## Dig a Hole in Terrain

![](./assets/06-12.png)   

P32   
## Crazy Idea - Volumetric Representation

In 3D computer graphics, a voxel represents a value on a regular grid in three-dimensional space. As pixels in a 2D     
bitmap, voxels themselves do not typically have their position (i.e. coordinates) explicitly encoded with their values     

P33   
## Marching Cubes

![](./assets/06-13.png)   

MARCHING CUBES: A HIGH RESOLUTION 3D SURFACE CONSTRUCTION ALGORITHM'; Computer Graphics, Volume 21, Number 4, July 1987    

P36    
Paint Terrain Materials

P38    
## Terrain Materials

![](./assets/06-14.png)   

P39    
## Simple Texture Splatting

![](./assets/06-15-1.png)   

![](./assets/06-15-2.png)   

P40    
## Advanced Texture Splatting

![](./assets/06-16-1.png)   

Blending with Height    

float3 blend(float4 texture1, float height1, float4 texture2, float height2)    
{    
return height1 > height2 ? texture1.rgb : texture2.rgb;    
}       

![](./assets/06-16-2.png)   

P41    
## Advanced Texture Splatting - Biased

![](./assets/06-17-1.png)   

![](./assets/06-17-3.png)   

![](./assets/06-17-2.png)   

**Links:**     
<https://www.gamedeveloper.com/programming/advanced-terrain-texture-splatting>    

P42   
## Sampling from Material Texture Array

![](./assets/06-18.png)   

P43   
## Parallax and Displacement Mapping

![](./assets/06-19-1.png)   

![](./assets/06-19-2.png)   

Parallax Mapping: Due to the height of the surface, the eye sees point B instead of point A. It creates a sense of dimensionality     

P44    
## Expensive Material Blending    

- **Many Texturing** - Low performance when multiple materials are sampled too many times    

- Huge Splat Map - We only see a small set of terrain, but we load splat maps for 100 square km into video memory      

![](./assets/06-20.png)   

P45    
## Virtual Texture

- Build a virtual indexed texture to represent all blended terrain materials for whole scene    
- Only load materials data of tiles based on view- depend LOD   
- Pre-bake materials blending into tile and store them into physical textures   

![](./assets/06-21.png)   

P46    
## VT Implementation, DirectStorage & DMA

![](./assets/06-22-1.png)   

![](./assets/06-22-2.png)   

![](./assets/06-22-3.png)   

P47    
## Floating-point Precision Error

P48    
## Camera-Relative Rendering

- Translates objects by the negated world space camera position before any other geometric transformations affect them    
- It then sets the world space camera position to 0 and modifies all relevant matrices accordingly    

P49    
Integration with other world elements (rocks, trees, grass)    

P50    
## Tree Rendering

P51    
## Decorator Rendering

P52    
## Road and Decals Rendering

P54   
## Procedure Terrain Creation

P55    
## Sky and Atmosphere

P59    
## How to "Paint" Everything in the Sky

![](./assets/06-23.png)   

P60   
## Atmosphere

P61    
## Analytic Atmosphere Appearance Modeling

$$
\mathbb{F} (\theta ,\gamma )=(1+Ae^{\frac{B}{\cos \theta +0.01}})\cdot (C+De^{E\gamma }+F\cos ^2\gamma +G\cdot \chi (H,\gamma )+I\cdot \cos ^ {\frac{1}{2} }\theta )  
$$

$$
L_\lambda =\mathbb{F} (\theta ,\gamma )\cdot L_{M\lambda} 
$$

![](./assets/06-24-1.png)   

Pros
• Calculation is simple and efficient
Cons
• Limited to ground view
• Atmosphere parameters can’t be
changed freely    

![](./assets/06-24-2.png)   

![](./assets/06-24-3.png)   

An Analytic Model for Full Spectral Sky-dome Radiance, ACM Trans 2012    

P62   
## Participating Media

- Volume filled with particles    
- Interact differently with light depending on its composition   