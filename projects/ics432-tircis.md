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

* Just increasing gcc compiler optimization level from 0 to 3 (maximum) resulted in 60.5476% reduction of average execution time.
* There is a file named svdfit_d.c which uses approximately 6% of the total execution time. In it, there are several sections in which loops are used to process several arrays (eg. thresholding their values against a unit step or accumulating sums of their elements). Attempting to parallelize some of these sections resulted in increased latency and I found that the median iteration limits of some of these loops were no greater than 4 (so parallelizing, say an accumulator would, resulted in waiting for many threads that were not able to do any meaningful work). However, this did help in narrowing down the sections of the file that were significantly contributing to overall execution time.


(Final results to be posted. Since the profiler used for this project was gprof, the ratios of improvement in execution times will be listed rather than reported execution times)

[Project Homepage](http://www.higp.hawaii.edu/~harold/tircis_doc/index.html)


