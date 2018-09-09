**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

## Flocking with CUDA on the GPU
### Connie Chang
  * [LinkedIn](linkedin.com/in/conniechang44), [Demo Reel](vimeo.com/ConChang/DemoReel)
* Tested on: Windows 10, Intel Xeon CPU E5-1630 v4 @ 3.70 GHz, GTX 1070 8GB (SIG Lab)

This project consists of three different algorithms for flocking, and a performance analysis.

10,000 particles flocking together
![](images/10K.gif)

Screenshot of 5,000 particles
![](images/Screenshot.PNG)

The three algorithms are:  
* Naive: Querying every particle to find close neighbors.
* Uniform Grid: Breaking the space into voxels, and only checking nearby voxels.
* Coherent Uniform Grid: The same as Uniform Grid, but with structuring particle data as contiguously as possible  

A graph comparing the performance of each algorithm.  
![](images/Default_5000Boids_128BlockSize.png)  
  

A graph comparing the number of particles (boids).  
![](images/NumberBoidsComparison.png)  
  
  
A graph comparing the number of threads per block.  
![](images/BlockSizeComparison.png)  
  
  
A graph comparing the uniform grid's cell width, relative to the neighbor search distance.  
![](images/CellWidthComparison.png)  
