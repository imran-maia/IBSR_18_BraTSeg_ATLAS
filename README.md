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
In our training data, tissue region voxels might not naturally align, necessitating image registration as a pivotal step for atlas generation. The process starts by selecting a fixed image from the population based on mutual information (refer to Figure 2), serving as the reference image. Subsequently, all other images in the dataset are treated as moving images and undergo registration against this fixed image to achieve alignment. We utilized elastix, developed by SimpleITK, for 3D Brain MR Image registration in this project. The choice of registration parameters can significantly influence the registration outcome. Consequently, we emphasize the importance of selecting appropriate parameters for optimal registration results. Our project employed three distinct registration techniques: **rigid, affine, and b-spline**.

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/3dc252fa-eaba-4cf4-9452-8748b1e0087d" width="700" alt="Pre-processed Image">
</p>
<p align="center">Figure 2: Mutual Information of Training Images.</p>


### Label Propagation
Following the registration process, the next crucial step involves label propagation. Here, the transformation parameters obtained in the initial step are applied to the respective labels for each instance. This ensures that labels are appropriately relocated in the registration space, aligning with the transformations applied to the images. To facilitate label propagation, we employed the transformix tool developed by SimpleITK. This tool plays a key role in applying the transformations to the labels, ensuring their accurate placement in the newly registered space.

### ATLAS Generation
Once image registration and label propagation are completed for each sample in the training subset, the subsequent step involves atlas generation. The choice of the atlas template is critical, and it can be based on either the fixed image used during registration or a mean image derived from all the registered images. The mean image is calculated voxel-wise across the dataset using the formula:

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/95dc338c-c5ba-4fdd-95c6-fe12baebfede" width="200" alt="Pre-processed Image">
</p>

Here, I<sub>M</sub> (x, y, z) represents the value of the voxel at position (x, y, z) in the mean image, N is the total number of images in the dataset, and I (x, y, z) denotes the value of the voxel at position (x, y, z) in the i-th registered image. For label synthesis, the propagated labels from the training images are combined using methods like voxel-wise averaging or majority voting. These approaches ensure accurate and representative labeling in the atlas, reflecting the most common or average features across the dataset.


### Tissue Model
Tissue models represent the conditional probabilities of voxel classes given their intensities, denoted as p(ω|x). These probabilities are derived by utilizing the ground truth label assignment for each volume. Subsequently, these tissue models can be employed for generating a brain segmentation solely based on intensities or for providing improved initialization in the Expectation-Maximization (EM) algorithm. Figure 3 demonstrates the entire pipeline of brain tissue segmentation using Probabilistic ATLAS.

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/83fa9902-2a98-4128-a5a6-6a21a4d9546f" width="1100" alt="Pre-processed Image">
</p>
<p align="center">Figure 3: Pipeline for Brain Tissue Segmentation Using Probabilistic ATLAS.</p>


# Results 
We assessed our brain tissue segmentation models using three key metrics: Dice Coefficient Score (DSC), Hausdorff Distance (HD), and Absolute Volumetric Distance (AVD). Figure 4 visually illustrates segmented images with the Probabilistic ATLAS using rigid, affine, and b-spline registration techniques, providing an overview of performance variations across different registrations.

<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/c417d9a4-b911-4a12-9897-8e0cd5e23ba3" width="500" alt="Pre-processed Image">
</p>
<p align="center">              Figure 4: Segmentation Results of Probabilistic ATLAS.</p>

Beyond visualizing the segmented results, a comprehensive performance analysis is presented in Table 1, outlining the computed metrics for each model. After analyzing all the computed metrics, it can be said that the affine transformation outperformed other registration techniques.

<br>
<p align="center">Table 1: Performance Analysis of Probabilistic ATLAS Based Brain Tissue Segmentation.</p>
<p align="center">
  <img src="https://github.com/imran-maia/IBSR_18_BraTSeg_ATLAS/assets/122020364/ed54e277-8722-4c7d-b232-e85dfd8a71ee" width="700" alt="Pre-processed Image">
</p>

