---
layout: post
title:  "The Dark Sky Project"
date:   2016-01-26 13:55:56 -0800
categories: jekyll update
---

![dark sky](http://deixismagazine.org/wp-content/uploads/2014/10/deixis_darksky.png)

## Background

The universe is populated by clusters of galaxies, connected by filaments, bordering cosmic voids. In cosmological research, the statistics of the distribution of these structures can be used in a variety of methods to constrain fundamental cosmological parameters. However, precisely modeling those parameters at these scales is an enormous challenge for numerical simulations, since both statistical and systematic errors conspire to prevent the emergence of an accurate theoretical model.

Another promising topic in cosmological research is about cosmic voids, which grow from regions of local divergence are the underdense regions that comprise most of the volume in the Universe. However, voids are typically studied from a theoretical perspective only in dark matter N-body simulations. To make direct contact with observed voids we must perform large-volume, high-resolution simulations to capture the structure and dynamics of dark matter.

This is where Dark Sky comes in, hoping to shed some light on the situation, by rendering the interactions between dark matter and dark energy visible. The Dark Sky team includes researchers from Stanford and Los Alamos. The Dark Sky Simulations are an ongoing series of cosmological N-body simulations designed to provide a quantitative and accessible model of the evolution of the large-scale Universe. Such models are essential for many aspects of the study of dark matter and dark energy, due to a lack of sufficiently accurate analytic model of non-linear gravitational clustering.

## Implementation

### Basic settings

The dark sky team used data collected from the Planck and WMAP spacecraft, probes which were launched to measure temperature fluctuations in the cosmic microwave background, the leftover thermal radiation from the Big Bang. The data collected by these spacecraft was used to set the initial conditions for the Dark Sky simulation.

Once these initial conditions were determined, the team plugged in the code that accounts for gravitational force and watched to see how particles interact. By the time the team was ready to run their Dark Sky simulations in 2014, they were operating with particles on the order of 10^12.

Due to the fact that the Dark Sky simulations were using 1 trillion particles, they had to make their "box" sufficiently large enough to handle the particles without messing up the gravitational dynamics. The result was a massive box which measures 8 gigaparsecs, about 25 billion light years, on each side.

### Hardware and Software

![Titan](http://cdn.knoxblogs.com/atomiccity/wp-content/uploads/sites/11/2014/09/titan.jpg)

The dark sky team managed to secure the use of Oak Ridge National Laboratory’s Titan, the second most powerful supercomputer in existence, for 80 million computing hours with a grant through the US Department of Energy. The group made use of two-thirds of Titan’s nodes for the ds14 simulation above, using up 10 million cpu hours over the course of 33 hours in April of 2014. After all 80 million cpu hours were exhausted by the team in December of 2014 and resulted in a total of 500-terabytes of simulation data.

> The complete Titan Cray XK7 system (Bland 2012) at Oak Ridge National Laboratory contains 18,688 compute
nodes, each containing a 16-core 2200 MHz AMD Opteron 6274 with 32 GB of 1600 MHz DDR3 memory, paired across a PCI-Express 2.0 bus with an NVIDIA Tesla K20x GPU with 6 GB of memory. The Cray Gemini interconnect (Alverson et al. 2010) provides roughly 8 Gbytes/sec of bi-directional bandwidth per node at the hardware level, with MPI latencies quoted as 1.5 microseconds or less. Titan host nodes currently run the SUSE Linux Enterprise Server 11 SP1 (x86 64) with current kernel 2.6.32.59. Processing nodes run Cray’s Compute Node Linux, designed to minimize interference between operating system services and application scalability.

All software was compiled with system installed gcc version 4.8.2, with the addition of NVIDIA’s LLVM-based nvcc V5.5.0 for two CUDA-specific files. Significant software dependencies used via the system modules interface include cray-mpich/6.2.0, fftw/3.3.0.3 and gsl/1.16. Additional packages were installed including gdb 7.6.90, Mercurial, git 1.8.5, Python 2.7.6, MPI for Python, Cython, Numpy and matplotlib.

### Algorithm and Implementation details

The codes undergirding the simulation began with Dark Sky’s lead investigator Michael Warren of the Los Alamos National Laboratory in the late 80s, namely 2HOT, although then the algorithm wasn’t specifically designed for Dark Sky. Warren’s codes have been used in a number of cosmological simulations throughout the years, their complexity only limited by the computing power available.

![n-body](http://icosmology.info/Nbody_Simulation_files/combo1.png)

2HOT is an adaptive treecode N-body method whose operation count scales as NlogN in the number of particles. Almost 30 years ago, the field of N-body simulations was revolutionized by the introduction of methods which allow N-body simulations to be performed on arbitrary collections of bodies in a time much less than O(N2). They have in common the use of a truncated expansion to approximate the contribution of many bodies with a single interaction. The resulting complexity is usually determined to be O(N) or O(NlogN), which allows computations using orders of magnitude more particles. These methods represent a system of N bodies in a hierarchical manner by the use of a spatial tree data structure. Aggregations of bodies at various levels of detail form the internal nodes of the tree (cells). These methods obtain greatly increased efficiency by approximating the forces on particles. Properly used, these methods do not contribute significantly to the total solution error.

The 2HOT code is written in the C programming language. A variety of gcc extensions are utilized, the most important of which is the vector_size attribute, which directs the compiler to use SSE or AVX vector instructions on Intel architectures. Using gcc with vector_size has eliminated the need to write the gravitational inner loops in assembly language to obtain good performance on CPU-only architectures.  The GPU portions of the code include both CUDA and OpenCL, with the CUDA versions performing somewhat better at present. A purely message-passing programming model is implemented in MPI. In order to hide latency, the tree-traversal phase uses an active message abstraction implemented inside MPI with a “asynchronous batched messages” routines.

The most scientifically relevant metric for evaluating gravitational N-body simulations is how many particles are updated per second, at an accuracy sufficient to accurately represent the physics involved. In 1997, the same team obtained a performance of 3 million particles updated per second at an RMS force accuracy better than 10^−3. Their current performance results are about 8 billion particles per second, with an equivalent force accuracy about 10 times better. Whether this accuracy is sufficient, or if accuracy can be sacrificed without adversely affecting the scientific results, is an area of current research.

## Intermediate results 

One of the major scientific progress is improving upon the spherical overdensity (SO) mass function. In the 30 years since the seminal work of Press and Schechter (1974), the number of particles in a simulation has increased from one thousand to one trillion (a factor of 10^9 ). They also reduce the effects of non-universality in applying the Tinker et al. (2008) mass function to the current standard cosmological model.  The matter power spectrum is a convenient statistic for probing the time-evolution of spatial clustering of matter in the Universe. Measuring the matter and galaxy power spectrum (and its inverse Fourier transform, the correlation function), on linear and non-linear scales, both directly and indirectly, is one of the major achivements of the dark sky project. Another major result is given by full sky galaxy cluster surveys. This simulation provides a unique opportunity for comparison with on-going surveys. In order to showcase this opportunity, the team also demonstrate its use in creating a simple mock SZ cluster catalog.



## Reference
+ [http://motherboard.vice.com/read/dark-sky-is-the-open-source-dark-matter-simulator](http://motherboard.vice.com/read/dark
-sky-is-the-open-source-dark-matter-simulator)
+ [http://darksky.slac.stanford.edu/edr.html](http://darksky.slac.stanford.edu/edr.html)
+ [http://www.symmetrymagazine.org/article/august-2014/open-access-to-the-universe](http://www.symmetrymagazine.org/article/august-2014/open-access-to-the-universe)
+ [http://arxiv.org/pdf/1310.4502v1.pdf](http://arxiv.org/pdf/1310.4502v1.pdf)
+ [http://arxiv.org/pdf/1407.2600v1.pdf](http://arxiv.org/pdf/1407.2600v1.pdf)


## About me

I am a second year Phd student in transportation engineering (CEE). My research interests include traffic simulation in large network, modeling and optimal control of transportation system.
I would like to learn parallel programming algorithms, models and tools from this class.