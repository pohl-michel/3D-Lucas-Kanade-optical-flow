# Volumetric image registration using the pyramidal, iterative Lucas–Kanade optical-flow algorithm

## Overview

This repository contains MATLAB code for registering a sequence of 3D images by estimating deformation vector fields (DVFs) between an initial volume and subsequent volumes using the pyramidal, iterative Lucas–Kanade optical-flow algorithm.

The animation below shows a tumor moving (mainly) vertically due to breathing in a 4DCT sequence, along with the time-varying DVF computed using the code in this repository. 

| Tumor motion along the coronal plane (2D projection of the computed 3D DVFs) between the reference frame and the other frames of a 4DCT sequence, computed using the iterative and pyramidal Lucas–Kanade optical flow. | Coronal cross-section of the 3D reference frame of the 4DCT sequence along with the region of interest and tumor contour.|
| :--- | :---|
| <center><img src="3DOF_4DCT_111_HM10395.gif" style="width:50%; height:auto;"/></center> | <img src="Input images/111_HM10395 4DCT/coronal_cross_section_with_roi_tumor_contour.jpg"/> |

Our implementation is based on the following research article (there are some small differences though):
Bouguet, Jean-Yves, 
"Pyramidal implementation of the affine Lucas Kanade feature tracker description of the algorithm.", 
Intel corporation 5.1-10 (2001): 4. 

Note: An adaptation of the code in this repository for 2D image registration (instead of 3D) is available here: https://github.com/pohl-michel/2D-MR-image-prediction. That repository focuses mainly on video forecasting but it contains an implementation of the iterative and pyramidal Lucas–Kanade optical flow algorithm, used during the first step in the forecasting pipeline.

## How to run

The main function to execute is "Lucas_Kanade_Pyramidal_Optical_Flow_Main.m".
The input image sequence is placed in the "Input images" folder.
Parameters concerning the image sequence itself, the DVF calculation, and the DVF display
are located respectively in the "3Dim_seq_par.xlsx" file, the "3DOF_calc_par.xlsx" file, and the "3Ddisp_par.xlsx" file.

The behavior of the program is controlled by the `beh_par` structure defined in `load_behavior_parameters3D()`,
and its fields can be changed manually.
Also, the name of the input sequences whose DVF is computed needs to be specified in the `input_im_dir_suffix_tab` array.

The resulting DVF, the DVF visualization, and the evaluation log files 
will be saved respectively in the folders "Optical flow calculation results mat files",
"Optical flow projection images", and "Log files".
The root-mean-square error (RMSE) of the registration can be found in that log file.

Also, by running "Lucas_Kanade_Pyramidal_Optical_Flow_Optimization_Main.m", one can perform hyper-parameter optimization by grid search to find an accurate DVF.
The hyper-parameters grid is specified in the file "load_3DOF_hyperparameters.m".
The results of the optimization is saved in the files "DVF optim log file.txt" and "DVF hyperpar influence (date and time).txt" 


## Image data

We also included three 4DCT sequences of tumors of patients with lung cancer,
acquired by a 16-slice helical CT simulator (Brilliance Big Bore, Philips Medical System)
in Virginia Commonwealth University Massey Cancer Center.

These images come from the TCIA 4D-lung dataset, which is publicly available: https://wiki.cancerimagingarchive.net/display/Public/4D-Lung


## References

This repository supports the findings in the following article:

Michel Pohl, Mitsuru Uesaka, Kazuyuki Demachi, Ritu Bhusal Chhatkuli, "Prediction of the motion of chest internal points using a recurrent neural network trained with real-time recurrent learning for latency compensation in lung cancer radiotherapy",
Computerized Medical Imaging and Graphics,
Volume 91,
2021,
101941,
ISSN 0895-6111 [[Published version](https://doi.org/10.1016/j.compmedimag.2021.101941)] [[arXiv](https://doi.org/10.48550/arXiv.2207.05951)]

Two other repositories contain code components supporting the article above:
 - Multivariate time-series forecasting with an RNN trained with RTRL: https://github.com/pohl-michel/time-series-forecasting-rtrl
 - 3D image warping with Nadaraya–Watson kernel regression: https://github.com/pohl-michel/Nadaraya-Watson-3D-image-warping

Please kindly consider citing our article if you use this code in your research.


## License

This repository is released under the BSD-3-Clause license.
