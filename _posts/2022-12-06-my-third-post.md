---
title: "Generate structural connectome (SC) from DWI using MRtrix3"
date: 2022-12-06
---


I learned from these two sources:

https://andysbrainbook.readthedocs.io/en/latest/MRtrix/MRtrix_Introduction.html 

https://micapipe.readthedocs.io/en/latest/pages/02.dwiproc/index.html


Required Softwares:

* MRtrix3 (You can find how to download [here](https://andysbrainbook.readthedocs.io/en/latest/MRtrix/MRtrix_Course/MRtrix_01_Download_Install.html))
* [FSL](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki) 
* ANTS (Installation instructions [here](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS))
* FreeSurfer (if you need surface base parcellation, download [here](https://surfer.nmr.mgh.harvard.edu/fswiki/DownloadAndInstall))


### DWI Processing

Required Documents: 3 types of files `.nii`, `.bval`, `.bvec`.

* `.nii` contains the brain image
* `.bval` indicates the b value of each volume
* `.bvec` indicates the b vector

Usually, we need to specify the predominant phase-encoding direction (e.g., Anterior to Posterior, or AP) and the reverse phase-encoding direction (e.g., PA) and extract the two sets of images. 

From Preprocessing to fiber tractography streamline, we need to go through the following steps:
* Denoising:
  * MP-PCA denoising
  * A reverse phase encoding
  * Optional: Correction of susceptibility distortion (e.g., Gibbs’ ringing artifacts),
  * Eddy current-induced distortions and motion,
  * Non-uniformity bias field correction.

Then DTI scalars such as the Fractional Anisotropy (FA) and Mean Diffusivity (MD) can be computed. 

* Basis function for each tissue type






