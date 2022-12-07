---
title: "Generate structural connectome (SC) from DWI using MRtrix3"
date: 2022-12-06
---


I learned from these two sources:

https://andysbrainbook.readthedocs.io/en/latest/MRtrix/MRtrix_Introduction.html 

https://micapipe.readthedocs.io/en/latest/pages/02.dwiproc/index.html

### DWI Processing

Required Documents: 3 types of files `.nii`, `.bval`, `.bvec`.

* `.nii` contains the brain image
* `.bval` indicates the b value of each volume
* `.bvec` indicates the b vector

Usually, we need to specify the predominant phase-encoding direction (e.g., Anterior to Posterior, or AP) and the reverse phase-encoding direction (e.g., PA) and extract the two sets of images.

Required Softwares:

* MRtrix3 (You can find how to download [here](https://andysbrainbook.readthedocs.io/en/latest/MRtrix/MRtrix_Course/MRtrix_01_Download_Install.html))
* [FSL](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki) 
* ANTS (Installation instructions [here](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS))
* FreeSurfer (if you need surface base parcellation, download [here](https://surfer.nmr.mgh.harvard.edu/fswiki/DownloadAndInstall))



