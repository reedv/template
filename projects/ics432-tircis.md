---
layout: project
type: project
image: images/tircis_thumbnail.jpeg
title: Tircis Spectral Processing Optimization 
permalink: projects/Tirsic
date: 2017
labels:
  - C/C++
  - Concurrency
summary: Currently, a running list of things I've tried to speed up hyperspectral data processing software used by the Hawaii Institute of Geophysics and Planetology.
---
<img class="ui image" src="../images/tircis.jpeg">

TIRCIS is a project at the Hawaii Institute of Geophysics and Planetology (UHM), which has developed a prototype of a hyperspectral thermal infrared imager for Earth surface, to be used in small satellites. It allows the quantifying of the chemical composition of targets (e.g., volcano plumes).

This page is currently just a running list of some of things I tried to optimize the code used to process hyperspectral data obtained from TIRCIS.

* Just increasing gcc compiler optimization level from 0 to 3 (maximum) resulted in 60.5476% reduction of average execution time. After this change, many sequential optimizations such as reducing local variables on the stack and converting array accesses to dereferenced pointers seemed to have little effect on the execution latency. 
* There is a file named svdfit_d.c which uses approximately 6% of the total execution time. In it, there are several sections in which loops are used to process several arrays (eg. thresholding their values against a unit step or accumulating sums of their elements). Attempting to parallelize some of these sections resulted in increased latency and I found that the median iteration limits of some of these loops were no greater than 4 (so parallelizing, say an accumulator, resulted in waiting for many threads that were not able to do any meaningful work while still adding overhead). However, this did help in narrowing down the sections of the file that were significantly contributing to overall execution time (these loops were bounded by a variable 'ndata').
* The 'ndata' bounded loops in svdfit_d.c were parallelized using OpenMP directives. This reduced the time the program spent in this file (as reported by the gprof profiler) by 195.918%.
* The function GeneProcess::GeneFrame originall accounted for 3.61% of total execution time. Addeding OpenMP directive to outermost loop reduced time spent in this function by 199.345% (as recorded by gprof profiler). This change, in addition to the changes to svdfit_d.c, resulting in a 14.14% reduction in total execution latency. 
* Parallelized a large inner loop in function svbksb_d(), which had initially accounted for 2.06% of total program execution time, reducing the profiled time the program spent in this function by 198.792%.


(Final results to be posted. Since the profiler used for this project was gprof, the ratios of improvement in execution times will be listed rather than reported execution times)

[Project Homepage](http://www.higp.hawaii.edu/~harold/tircis_doc/index.html)


