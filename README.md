# Volumetric image registration using the pyramidal, iterative Lucas–Kanade optical-flow algorithm

## Overview

This repository contains MATLAB code for registering a sequence of 3D images by estimating deformation vector fields (DVFs) between an initial volume and subsequent volumes using the pyramidal, iterative Lucas–Kanade optical-flow algorithm.

The animation below shows a lung tumor moving mainly in the superior–inferior direction due to breathing in a 4D computed tomography (4DCT) sequence, along with the time-varying deformation vector fields (DVFs) estimated using this code.


<p align="center"> <img src="3DOF_4DCT_111_HM10395.gif" width="30%" alt="Estimated 3D lung tumor motion projected onto a coronal plane"> </p>

<p align="center"> <em> Estimated 3D lung-tumor motion between the reference frame and the subsequent frames of a 4DCT sequence, projected onto a coronal plane. The corresponding 4DCT cross-sections are displayed in the background. </em> </p>

<p align="center"> <img src="Input images/111_HM10395 4DCT/coronal_cross_section_with_roi_tumor_contour.jpg" width="50%" alt="Reference 4DCT frame with region of interest and tumor contour"> </p>

<p align="center"> <em> Coronal cross-section of the reference frame of the same 4DCT sequence, along with the region of interest and tumor contour. </em> </p>

**Note:** An adaptation of this Lucas–Kanade optical-flow implementation for 2D image registration is available in the [`2D-MR-image-prediction`](https://github.com/pohl-michel/2D-MR-image-prediction) repository, where it is used as the first step of a cine-MR frame-forecasting algorithm. 


## Image data

The input images are located in the `Input images` folder. This repository includes three chest 4DCT sequences from patients with lung cancer, derived from the [TCIA 4D-Lung dataset](https://www.cancerimagingarchive.net/collection/4d-lung/).

The input image sequences to process are specified in the `input_im_dir_suffix_tab` array inside the main executable files:
 - `Lucas_Kanade_Pyramidal_Optical_Flow_Main.m`
 - `Lucas_Kanade_Pyramidal_Optical_Flow_Optimization_Main.m`

Each input sequence folder contains the following configuration files:

<table align="center">
  <thead>
    <tr>
      <th>Filename</th>
      <th>Parameters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>3Dim_seq_par.xlsx</code></td>
      <td>Image-sequence properties</td>
    </tr>
    <tr>
      <td><code>3DOF_calc_par.xlsx</code></td>
      <td>Optical-flow calculation parameters</td>
    </tr>
    <tr>
      <td><code>3Ddisp_par.xlsx</code></td>
      <td>Figure display and saving</td>
    </tr>
  </tbody>
</table>


## How to run

### Optical-flow estimation with specified hyperparameters

Optical-flow calculation for a specified set of parameters can be performed by executing `Lucas_Kanade_Pyramidal_Optical_Flow_Main.m` in MATLAB from the repository root.

Program behavior is controlled by manually setting the boolean fields of the beh_par structure defined in `load_behavior_parameters3D.m`.

The output DVF arrays, visualization files, and performance log files, including the registration root-mean-square error, are saved in the folders `Optical flow calculation results mat files`, `Optical flow projection images`, and `Log files`, respectively. These folders are automatically created if they do not exist.


### Hyperparameter selection with grid search

Grid-search-based hyperparameter selection can be performed by executing `Lucas_Kanade_Pyramidal_Optical_Flow_Optimization_Main.m` in MATLAB from the repository root.

The hyperparameter grid can be configured in `load_3DOF_hyperparameters.m`.

Grid-search performance is recorded in `DVF optim log file.txt` and `DVF hyperpar influence [date and time].txt` inside the `Log files` folder.


## Requirements

Running this code requires MATLAB and the Image Processing Toolbox, as it calls functions such as `imgaussfilt3` and `dicomread`.


## References

This repository supports the findings in the following article:

Michel Pohl, Mitsuru Uesaka, Kazuyuki Demachi, Ritu Bhusal Chhatkuli, "Prediction of the motion of chest internal points using a recurrent neural network trained with real-time recurrent learning for latency compensation in lung cancer radiotherapy",
Computerized Medical Imaging and Graphics,
Volume 91,
2021,
101941,
ISSN 0895-6111 [[published version](https://doi.org/10.1016/j.compmedimag.2021.101941)] [[arXiv](https://doi.org/10.48550/arXiv.2207.05951)]

Two other repositories contain code components supporting the article above:
 - Multivariate time-series forecasting with an RNN trained with RTRL: https://github.com/pohl-michel/time-series-forecasting-rtrl
 - 3D image warping with Nadaraya–Watson kernel regression: https://github.com/pohl-michel/Nadaraya-Watson-3D-image-warping

Please kindly consider citing our article if you use this code in your research. Our implementation of the Lucas–Kanade optical-flow algorithm is based on the pyramidal, iterative framework described in the following article:

Jean-Yves Bouguet, 
"Pyramidal implementation of the affine Lucas Kanade feature tracker description of the algorithm.", 
Intel corporation 5.1-10 (2001): 4. 
[[article]](https://robots.stanford.edu/cs223b04/algo_affine_tracking.pdf)


## License

This repository is released under the BSD-3-Clause license.
