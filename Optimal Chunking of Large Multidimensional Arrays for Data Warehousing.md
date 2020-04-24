# Optimal Chunking of Large Multidimensional Arrays for Data Warehousing
- DOLAP '07: Proceedings of the ACM tenth international workshop on Data warehousing and OLAP
- ACM: Association for Computing Machinery

## Abstract
- Very large multidimensional arrays are **commonly used in** data intensive scientific computations as well as MOLAP
- **MOLAP**: on-line analytical processing applications
- The **storage organization of such arrays** on disks is done by partitioning the large global array into fixed size sub-arrays called chunks or tiles that form the units of data transfer between disk and memory.
- Typical queries involve the retrieval of sub-arrays in a manner that access all chunks that overlap the query results. An important **metric of the storage efficiency is the expected number of chunks retrieved over all such queries**.

Question:
“what shapes of array chunks give the minimum expected number of chunks
over a query workload?”

## Introduction
- Numerous applications in scientific domains such as Physics, Astronomy, Geology, Earth Sciences, Statistics, etc., map their problems space onto matrices and multi-dimensional arrays on which mathematical tools such as linear, non-linear equations
solvers and differential equation solvers can be applied.
- These arrays are required to be persistent on disks and subsequently accessed efficiently for scientific analysis.
- In general, both scientific and MOLAP datasets can be considered as a collection of multi-dimensional arrays that reside on secondary storage and queries on an array involve an orderly access of either the entire array or a hyper-rectangular sub-array.

### To store array elements on disk
#### A/ Naive strategy
One can naively utilize the mapping of multi-dimensional array indices onto linear storage.
  - example: C-order / F-order. Accessing the elements in a different order [...] gives very poor performance [16].
  - Such a layout is only worth considering if the array is generally dense, i.e., almost every array entry exists.
  - it is not extendible without storage reorganization.
  - considerations: 
    - array can be very large hence requiring lot of space and creating huge files
    - possible that arrays are sparse in which case we allocate much more space than necessary
    - in both scientific data storage and MOLAP storage, the data incrementally grows over time and as such the array storage mapping must be extendible. See Note (*)
    
Note (*):
We say an array is extendible if the array bounds are allowed to grow by admitting new array elements that
are appended to the storage space but without reorganizing previously allocated elements.

#### B/ Chunking
Persistent storage organization of multi-dimensional arrays is typically done by partitioning them into coarse-grained hyper-rectangular blocks called **chunks** or **tiles** which form the units of array transfers between disk and memory [15, 16, 5, 9].

- A chunk is characterized by two parameters: 
  - 1) the chunk size
  - 2) the chunk shape
  - The size is defined as the number of elements that can be contained in a chunk.
- **A query over the dataset for analysis retrieves** either the entire array or a sub-array in which case all the array chunks that overlap the query result are retrieved.
- Even though the elements contained in each chunk, are stored either in row-major order, or column major order, **the layout of the chunks on disk can be done using some other linear mapping** function such as the Morton sequence, Hilbert scan, or Peano scan order [8].

Benefits for multidim array storage: 
• array chunks with all zero entries are not stored and chunks with fewer entries below a
specified threshold can be compressed. This results in an improved storage utilization.
• Allocating chunks through an index scheme, e.g., B + -tree, allows for arbitrary array ex-
pansions without storage reorganization.

### Optimal chunk problem
- large chunk sizes may cause unnecessary data to be read
- small chunk sizes may require more disk accesses to retrieve all chunks to answer a query 
- the chunk shape influences the number of chunks retrieved in answering a query

An important metric of the storage efficiency is the expected number of chunks retrieved by queries under the access workload.

>>> Personal warning: if chunk is too big we only use 1 chunk for every query but we read more than necessary. Therefore the question should be stated as reducing chunk shape while maintaining the number of accessed chunks low.

#### Previous work 
The problem of optimal chunking was first introduced by
Sarawagi and Stonebraker [15], who gave an approximate solution to this problem. We show
that the optimal shape derivation given by Sarawagi and Stonebraker is only approximate and
under certain circumstances can deviate significantly from the true answer.

#### Contributions 
- The development of two accurate mathematical models of the chunking problem;
- show how the chunking parameters should be determined based on the probabilistic access patterns of sub-array queries.
- Derivation of exact solutions, one using steepest descent and another using geometrical programming method which lead to accurate retrieval costs and optimal chunk shape calculation.
- Experimental comparison of the estimation errors induced by the models using synthetic workloads on real life datasets.

## Related work

In nearly all applications that use disk resident large scale multi-dimensional arrays, the physical
organization of the array is by chunking.

