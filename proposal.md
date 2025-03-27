# Project Proposal

## Project Title: Exploiting SIMD in Quadtree Querying
Team Member: Rong Mu (rongmu), Ziruo Xiao (ziruox)

### SUMMARY
This project proposes to develop SIMD-parallel techniques for accelerating the querying of Point-Range quadtree. The primary goal is to enable batch processing of multiple nodes in a query simultaneously, by exploiting SIMD-friendly traversal logic and memory optimization techniques. We also hope to explore the parallelism across queries and compare the parallelism performance of different spatial indexing tree structures.

### Background


### The Challenge
The nature of the PR quadtree provides space for SIMD utilization, but the inherent complexity and irregularity of the PR quadtree data structure can make it difficult to efficiently parallelize and optimize for SIMD processing. The main challenges include:

**Data Dependencies:** The traversal of the PR quadtree exhibits data dependencies, as the search path through the tree depends on the query point. This can lead to divergent execution, making it challenging to leverage SIMD parallelism effectively, as the constraint of systems in balancing parallelism and overhead.

**Memory Access Patterns:** The memory access patterns in the PR quadtree are irregular and lack spatial locality, as the nodes are distributed across memory based on the spatial partitioning of the tree. This can result in inefficient use of some SIMD operations and poor cache utilization, as constrained by the way a tree structure is stored. The way the nodes are organized in memory can significantly impact the performance of SIMD-friendly memory access techniques, such as prefetching and memory compression. Optimizing the tree storage structure for SIMD processing may require non-trivial modifications to the data structure.

**Communication to Computation Ratio:** Constrained by the heterogeneous nature of query points, the PR quadtree traversal involves a significant amount of pointer chasing and conditional branching, leading to a high communication to computation ratio. Irregular access patterns and varying levels of spatial coherence among the query points can further impact the effectiveness of SIMD parallelization.

### RESOURCES
Compute resource (details given in platform choice session): We need SIMD and system profiling supported computers and plan to first work on GHC machines, but may benefit from machines with wider SIMD/more cores/allows profiling tools installation.

**Starter code:** We are going to build upon this baseline implementation of Quadtree: https://github.com/pvigier/Quadtree

**Dataset:** The baseline implementation also contains a test data generator. Plus, we plan to test on real-world datasets. We haven’t figured out the existing spatial database with quadtree constructed, and we may try to construct one by ourselves from geospatial datasets.

**Related works:**

SIMD-ified R-tree Query Processing and Optimization:
https://dl.acm.org/doi/pdf/10.1145/3589132.3625610

Parallel Quadtree Coding of Large-Scale Raster Geospatial Data on GPGPUs:
https://dl.acm.org/doi/pdf/10.1145/2093973.2094047

### GOALS AND DELIVERABLES

**Plan To Achieve**
- Implement SIMD parallel within query:
    - Batch-process multiple query points simultaneously using SIMD instructions.
    - Exploit SIMD-friendly traversal logic (e.g., search order benefiting gather/scatter, selective load/store)
- Exploit SIMD-friendly memory tricks:
    - Tree storage structure (how nodes are organized in the range of address)
    - Memory compression methods
    - Study possible prefetching techniques
- Compare performance with baseline on generated dataset, including speedup and system performance analyzation

Given the nature that PR quadtree has 4 child nodes per non-leaf node, we hope to achieve a speedup close to 4x, or otherwise understand the bottleneck for speedup.

**Hope To Achieve:**
- Conduct experiments on real-world dataset (likely need to construct first)
- SIMD/multi-thread parallel (Vectorization) across query
- A speedup and system profiling comparison with other tree structure SIMD (e.g., R-tree https://dl.acm.org/doi/pdf/10.1145/3589132.3625610).

### PLATFORM CHOICE

#### Language

We are going to use C++ for implementation, as C++ is a common and efficient language for system development with SIMD.

#### Computation resource
We are going to start with implementing and conducting experiments on the GHC machine with SIMD support. The GHC machines have 8 cores and 8 SIMD ALUs per core. This supports AVX2 which provides 256-bit wide registers that can process, e.g., 4 double-precision numbers in parallel, and is capable of conducting basic system profiling.

We think this is adequate for our experiments for at least the Plan To Achieve goals. But we may need machines with wider SIMD/more cores/allows profiling tools installation. We may then switch to AWS machines.

### SCHEDULE

| Time | TODOs |
|----------|--------------------|
|  Week 1 (3.26-4.1) p.s. Midterm 2 on 4.2 |  Value C |
|  Week 2 (4.2-4.8) |   Value F |
|  Week 3 (4.9-4.15) p.s. Milestone report on 4.15 | |
|  Week 4 (4.16-4.22) | |
|  Week 5 (4.23-4.29) | |







