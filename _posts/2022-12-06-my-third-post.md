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
  * The Dhollander algorithm is used to estimate the response functions of cerebrospinal fluid (CSF), gray, and white matter.
  * These are then used to estimate the fiber orientation distribution (FOD) by spherical deconvolution.
  * Intensity normalization is applied to each tissue FOD to enable comparison between subjects.

* Create a GM/WM boundary for seed analysis
  * A non-linear transformation is computed between the normalized white matter FOD and s-MRI (T1) aligned to the b0 image.
  * Then, a five-tissue-type (5TT) image segmentation is generated and registered to the DWI space, and a gray matter white matter interface mask is calculated. 

* Run the streamline analysis
  * First, a tractography with 10 million streamlines is generated using the iFOD2 algorithm
  * Next, spherical deconvolution informed filtering of tractograms [SIFT2] is applied to reconstruct whole brain streamlines weighted by cross-sectional multipliers. 











