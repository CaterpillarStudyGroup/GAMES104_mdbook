
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


