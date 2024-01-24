# Brain Tissue Segmentation from Magnetic Resonance Image Using Probabilistic ATLAS

Author(s): **Mohammad Imran Hossain**, Muhammad Zain Amin
<br>University of Girona (Spain), Erasmus Mundus Joint Master in Medical Imaging and Applications

**Abstract:** The segmentation of brain tissues in Magnetic Resonance Imaging (MRI) is essential for investigating neurodegenerative diseases, including Alzheimer's, Parkinson's, and Multiple Sclerosis (MS). Beyond accurate diagnosis, this process facilitates quantitative analysis and contributes to advancements in personalized medicine and targeted therapies. In this project, we explored the effectiveness of statistical-based approaches, particularly Probabilistic ATLAS, along with different rigid and non-rigid image registration techniques for segmenting brain tissues from MRI, utilizing the IBSR18 dataset. The findings highlight the strong performance of Probabilistic ATLAS with affine registration, achieving an average Dice Score of **0.720 ± 0.036**, Hausdorff Distance **(6.349 ± 0.343)**, and Absolute Volumetric Difference **(6.349 ± 10.543)**.

# Dataset
The dataset, IBSR 18, used for this brain tissue segmentation challenge is a publicly available dataset by the Center for Morphometric Analysis at Massachusetts General Hospital in the United States of America. The dataset consists of 18 T1-weighted volumes with different slice thicknesses and spacing.

- Training Set : 10
- Validation Set: 05
- Test Set : 03

# Methodology
The brain tissue segmentation from MRI using Probabilistic ATLAS consists of several key steps including image pre-processing, image registration, label propagation, ATLAS generation, and segmentation. Each of the mentioned sections is explained with necessary figures and relevant information.

### Pre-processing
Segmenting brain tissue from medical images faces challenges due to scanner variations, causing uneven intensity, contrast, and noise. Essential preprocessing steps, including bias field correction and anisotropic diffusion, are pivotal for achieving uniform and precise tissue segmentation. While IBSR 18 volumes are already skull-stripped, we applied **N4 Bias Field Correction** and **Anisotropic Diffusion** to denoise intensities. Additionally, we performed **Image Normalization** aligns intensity scales, facilitating consistent processing for both statistical and deep learning methods.

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_Deep_Learning/assets/122020364/b1639ea9-dfdf-45d3-bf7f-cd7294e74104" width="700" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: Pre-processed Image.</p>

### Image Registration
There is a high possibility that the tissue region voxels of all the images for training will not be in the same alignment. Therefore, image registration is an essential and initial step for atlas generation. To begin the image registration, at first, a fixed image is chosen from the population based on the mutual information shown in Figure 4 which is considered as reference image. After that, all other images in the population are considered as moving images and perform registration against the fixed image to align them into a common. In this project, we have used **elastix** developed by SimpleITK for 3D Brain MR Image registration. The image registration can be varied due to the uses of different registration parameters. As image registration can be varied due to the uses of different registration parameters, choosing the appropriate registration parameter is important for better registration outcomes. In this project, we have used three different registration techniques such as rigid, affine, and b-spline.


<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/3dc252fa-eaba-4cf4-9452-8748b1e0087d" width="500" alt="Pre-processed Image">
</p>


### Label Propagation
After the completion of registration, the subsequent task is to propagate the label. In this step, the transformation parameters acquired in the first step are applied to the corresponding labels for each instance. This process ensures that labels are moved to the registration space in accordance with the transformations applied to the images. For propagating the labels, we used the **transformix** tool developed by SimpleITK.

### ATLAS Generation
Once the image registration and corresponding label propagation are completed for each sample in the training subset, the next step is to generate the atlas. The choice of the atlas template is pivotal and can be based on either the fixed image used during registration or a mean image derived from all the registered images. This mean image is calculated voxel-wise across the dataset, using the formula:

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/95dc338c-c5ba-4fdd-95c6-fe12baebfede" width="200" alt="Pre-processed Image">
</p>



### Tissue Model
Tissue models represent the conditional probabilities of voxel classes given their intensities, denoted as p(ω|x). These probabilities are derived by utilizing the ground truth label assignment for each volume. Subsequently, these tissue models can be employed for generating a brain segmentation solely based on intensities or for providing improved initialization in the Expectation-Maximization (EM) algorithm.




<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/83fa9902-2a98-4128-a5a6-6a21a4d9546f" width="900" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: Pre-processed Image.</p>


# Results 
To assess the performance of our brain tissue segmentation models, we employed three key metrics: Dice Coefficient Score (DSC), Hausdorff Distance (HD), and Absolute Volumetric Distance (AVD). The visual representation of predicted segmentation results from each deep-learning model is illustrated in Figure

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/c417d9a4-b911-4a12-9897-8e0cd5e23ba3" width="400" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: Pre-processed Image.</p>

Beyond visualizing the segmented results, a comprehensive performance analysis is presented in Table 1, outlining the computed metrics for each model. After analyzing all the computed metrics, it can be said that the affine transformation outperformed other registration techniques U-Net and LinkNet and provided outstanding segmentation results.

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/ed54e277-8722-4c7d-b232-e85dfd8a71ee" width="700" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: Pre-processed Image.</p>
