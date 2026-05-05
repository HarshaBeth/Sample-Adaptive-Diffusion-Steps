# Sample-Adaptive-Diffusion-Steps

This repository contains the code and experiment notebooks for **Complexity-Guided Adaptive Diffusion Steps for Efficient Image Restoration**.

The project investigates whether the number of diffusion sampling steps required for high-quality image restoration can be predicted from image complexity. Instead of using the same fixed number of diffusion steps for every image, the goal is to estimate a sample-specific step count based on statistical complexity features extracted from the degraded input image.

## Project Overview

Diffusion restoration methods such as DDRM can produce high-quality restoration results, but they typically use a fixed number of sampling steps for every input image. This can be inefficient because some images may be restored well with fewer steps, while others may require more iterations due to higher structural or textural complexity.

This project studies whether image complexity can be used to guide the number of diffusion steps required per image.

The overall pipeline is:

1. Apply Gaussian blur to clean images.
2. Restore degraded images using DDRM at multiple diffusion step counts.
3. Evaluate reconstruction quality using LPIPS and PSNR.
4. Estimate image complexity using a Gaussian Mixture Model (GMM) trained on local image patches.
5. Correlate GMM-based complexity statistics with the empirically observed required diffusion steps.
6. Train simple regression models to predict the required number of diffusion steps.

## Repository Structure

```text
Sample-Adaptive-Diffusion-Steps/
├── data/
│   ├── README.md
│   └── .gitkeep
├── external/
│   ├── README.md
│   └── .gitkeep
├── notebooks/
│   ├── README.md
│   ├── checkpoints/
│   ├── human_evals/
│   └── results csv files/
├── src/
│   ├── diffusion/
│   │   ├── DDIM experimenting notebooks/
│   │   └── DDRM notebooks/
│   ├── gmm/
│   │   ├── Gopro data experiment/
│   │   ├── Imagenet data experiment/
│   │   └── gmm_complexity_pilot.ipynb
│   └── regression/
├── requirements.txt
└── README.md
```

## Main Components
### Diffusion Experiments

Located in:

```src/diffusion/```

This folder contains notebooks for DDIM exploration and DDRM restoration experiments.

The main DDRM notebooks are:
```
src/diffusion/DDRM notebooks/ddrm_deblurring_gopro.ipynb
src/diffusion/DDRM notebooks/ddrm_deblurring_imagenet100.ipynb
src/diffusion/DDRM notebooks/ddrm_deblurring_ood6.ipynb
```

These notebooks evaluate image restoration quality across different diffusion step counts.

## GMM Complexity Estimation

Located in:
```
src/gmm/
```

The GMM is trained on local grayscale image patches and used as a feature extractor for image complexity. For each degraded image, patch-wise negative log-likelihood values are computed and summarized using statistics such as:

mean NLL
median NLL
90th percentile NLL
max NLL
top-10% mean NLL

These features are used to study the relationship between image complexity and required diffusion steps.

Regression Experiments

Located in:

```src/regression/```

The regression notebooks use GMM-based complexity features to predict the empirically estimated optimal diffusion step count, denoted as k*.

Main notebooks:
```
src/regression/complexity_to_kstar_regression.ipynb
src/regression/complexity_gopro_regression.ipynb
```

## Datasets

The datasets are not included in this repository because they are large.

Expected local datasets include:
```
data/BSDS300/
data/GoPro_dataset_custom/
data/imagenet_subset/
data/img_align_celeba/
```

See:
```data/README.md```
for more details.

## External Dependencies

The DDRM repository is not committed directly to this repository. It should be cloned locally into:
```external/ddrm/```

See:
```external/README.md```
for DDRM setup instructions and example commands.

## Local Artifacts

Generated artifacts are not committed to GitHub. These include:
```
notebooks/checkpoints/
notebooks/human_evals/
notebooks/results csv files/
```

These folders may contain trained GMM checkpoints, scalers, human evaluation outputs, and CSV result files.

See:
```notebooks/README.md```
for more details.

## Installation

Create and activate a Python environment:
```
python -m venv .venv
source .venv/bin/activate
```

Install dependencies:
```pip install -r requirements.txt```

DDRM may require its own separate environment. See external/README.md for details.

## Reproducibility Notes

To reproduce the experiments:

1. Download or prepare the required datasets under data/.
2. Clone and set up DDRM under external/ddrm/.
3. Run the DDRM notebooks in src/diffusion/ to generate restoration outputs at different step counts.
4. Run the GMM notebooks in src/gmm/ to compute image complexity features.
5. Run the regression notebooks in src/regression/ to analyze correlations and predict k*.

## Notes on Blur Strength

For the DDRM Gaussian deblurring experiments, the blur strength was adjusted inside:
```external/ddrm/runners/diffusion.py```

This modification was used to test stronger Gaussian blur settings and study how degradation severity affects the relationship between image complexity and required diffusion steps.

## Results Summary

The experiments show that GMM-based patch complexity features have a measurable relationship with the number of diffusion steps required for restoration.

Key observations:

ImageNet showed moderate correlations between global NLL statistics and required diffusion steps.
GoPro deblurring experiments showed stronger correlations for high-NLL patch statistics such as max NLL and top-10% mean NLL.
Local high-complexity regions appear to play an important role in determining restoration difficulty under stronger blur.

## Project Status

This repository is a research prototype developed for studying sample-adaptive diffusion step selection. The current implementation focuses on correlation analysis and simple regression-based prediction rather than a fully optimized production inference system.

## Author

Harsha Basavaraj Beth <br>
Boston University
