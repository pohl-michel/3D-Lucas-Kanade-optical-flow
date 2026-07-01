# Volumetric image registration using the pyramidal, iterative Lucas–Kanade optical-flow algorithm

## Overview

This repository contains MATLAB code for registering a sequence of 3D images by estimating deformation vector fields (DVFs) between an initial volume and subsequent volumes using the pyramidal, iterative Lucas–Kanade optical-flow algorithm.

The animation below shows a lung tumor moving mainly in the superior–inferior direction due to breathing in a 4D computed tomography (4DCT) sequence, along with the time-varying deformation vector fields (DVFs) estimated using this code.

<table align="center" border="0" cellspacing="0" cellpadding="4">
  <tr>
    <td align="center" valign="middle" width="38%">
      <img src="3DOF_4DCT_111_HM10395.gif" width="100%" alt="Estimated 3D lung-tumor motion projected onto a coronal plane">
    </td>
    <td width="4%"></td>
    <td align="center" valign="middle" width="58%">
      <img src="Input%20images/111_HM10395%204DCT/coronal_cross_section_with_roi_tumor_contour.jpg" width="100%" alt="Coronal cross-section of the reference 4DCT frame with region of interest and tumor contour">
    </td>
  </tr>
</table>

<p align="center">
  <em>
    Left: estimated 3D lung-tumor motion between the reference frame and subsequent frames of a 4DCT sequence, projected onto a coronal plane and displayed over the corresponding 4DCT cross-sections. Right: coronal cross-section from the reference frame of the same sequence, along with the region of interest and tumor contour.
  </em>
</p>

**Note:** An adaptation of this Lucas–Kanade optical-flow implementation for 2D image registration is available in the [`2D-MR-image-prediction`](https://github.com/pohl-michel/2D-MR-image-prediction) repository, where it is used as the first step of a cine-MR frame-forecasting algorithm. 


## Image data

The input images are located in the `Input images` folder. This repository includes three chest 4DCT sequences from patients with lung cancer, derived from the [TCIA 4D-Lung dataset](https://www.cancerimagingarchive.net/collection/4d-lung/).

The input image sequences to process are specified in the `input_im_dir_suffix_tab` array inside the main executable files:
 - `Lucas_Kanade_Pyramidal_Optical_Flow_Main.m`
 - `Lucas_Kanade_Pyramidal_Optical_Flow_Optimization_Main.m`


## How to run

### Optical-flow estimation with specified hyperparameters

Optical-flow calculation for a specified set of parameters can be performed by executing `Lucas_Kanade_Pyramidal_Optical_Flow_Main.m` in MATLAB from the repository root.

Program behavior is controlled by manually setting the boolean fields of the `beh_par` structure defined in `load_behavior_parameters3D.m`.

Parameters specific to each input image sequence can be found in the following configuration files, located in each input sequence folder:

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
      <td>Figure display and saving parameters</td>
    </tr>
  </tbody>
</table>

The output DVF arrays, visualization files, and performance log files, including the registration root-mean-square error, are saved in the folders `Optical flow calculation results mat files`, `Optical flow projection images`, and `Log files`, respectively. These folders are automatically created if they do not exist.

**Note:** An optional Python utility, `create_gif.py`, can be run to assemble selected optical-flow projection images into an animated GIF. The script expects JPG frames named `frame_1.jpg`, `frame_2.jpg`, etc. in the `Optical flow projection images/out` folder and writes `output.gif` in the repository root.


### Hyperparameter selection with grid search

Grid-search-based hyperparameter selection can be performed by executing `Lucas_Kanade_Pyramidal_Optical_Flow_Optimization_Main.m` in MATLAB from the repository root.

The hyperparameter grid can be configured in `load_3DOF_hyperparameters.m`.

Grid-search performance is recorded in `DVF optim log file.txt` and `DVF hyperpar influence [date and time].txt` inside the `Log files` folder.


## Requirements

Running the MATLAB code requires MATLAB's Image Processing Toolbox, as it calls functions such as `imgaussfilt3` and `dicomread`.

The optional GIF-generation utility `create_gif.py` requires Python with Pillow, imported as `PIL`.


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
"Pyramidal implementation of the affine Lucas Kanade feature tracker description of the algorithm", 
Intel corporation 5.1-10 (2001): 4
[[article]](https://robots.stanford.edu/cs223b04/algo_affine_tracking.pdf)


## License

This repository is released under the BSD-3-Clause license.
