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

Performance Graphs
------------------
A graph comparing the performance of each algorithm.  
![](images/Default_5000Boids_128BlockSize.png)  
  

A graph comparing the number of particles (boids).  
![](images/NumberBoidsComparison.png)  
  
  
A graph comparing the number of threads per block.  
![](images/BlockSizeComparison.png)  
  
  
A graph comparing the uniform grid's cell width, relative to the neighbor search distance.  
![](images/CellWidthComparison.png)  

Performance Questions and Answers: 
-----------------------
*For each implementation, how does changing the number of boids affect performance? Why do you think this is?*  

**Naive**: Increasing boids slows down the simulation. This makes sense because the algorithm takes more time to search through every boid.  
**Uniform Grid**: Surprisingly, performance gets better as the number of boids increases. I've heard that GPUs can perform faster when larger blocks of threads are used. My guess is that's why my results are like so.  
**Coherent**: Similar to uniform grid, the performance increases as the number of boids increases.  
  
*For each implementation, how does changing the block count and block size affect performance? Why do you think this is?*   

For all implementations, changing the block size did not significantly affect performance. I think this is because we need much more than 1024 threads, the maximum block size. Therefore, the blocks must be queued even at max block size, and we cannot go any faster.  

*For the coherent uniform grid: did you experience any performance improvements with the more coherent uniform grid? Was this the outcome you expected? Why or why not?*  

No, I did not see any performance improvements. In fact, it was sometimes slower to use the coherent uniform grid. This was not what I expected because I thought it would be faster. I expected lining up the boids more contiguously in memory would mean faster access to them. Seeing my results, my guess is that the extra steps to reshuffle the position/velocity data were too slow. It slowed down the process enough that the contiguous memory could not make up for it.  

*Did changing cell width and checking 27 vs 8 neighboring cells affect performance? Why or why not? Be careful: it is insufficient (and possibly incorrect) to say that 27-cell is slower simply because there are more cells to check!*  

No, changing the cell width did not affect performance. Even though we are checking more neighboring cells with a smaller width, we check for less boids in each cell. In other words, a large cell has a higher chance of encompassing more boids that are too far to influence the current boid. On the other hand, a small cell has a lower chance of encountering such boids.
