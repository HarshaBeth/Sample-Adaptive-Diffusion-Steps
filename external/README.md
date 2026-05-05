# External Dependencies

This directory is used for external repositories and third-party model code.

The main external dependency used in this project is DDRM:

- Repository: Denoising Diffusion Restoration Models (DDRM)
- Used for Gaussian deblurring experiments and diffusion-step evaluation.

## Cloning DDRM

From the project root, run:
```
cd external
git clone https://github.com/bahjat-kawar/ddrm.git
cd ddrm
```

## Environment Setup

DDRM provides its own environment file:

```
conda env create -f environment.yml
conda activate ddrm
```

Depending on the machine or cluster setup, additional package/version adjustments may be required.

## Running DDRM

The DDRM experiments are run from inside the cloned DDRM directory using its main.py script and configuration files under configs/.

A typical command structure is:

```
python main.py \
  --ni \
  --config imagenet_256.yml \
  --doc <experiment_name> \
  --timesteps <num_steps> \
  --eta 0.85 \
  --etaB 1 \
  --deg deblur_gauss \
  --sigma_0 <noise_value> \
  -i <run_name>
```

The value passed to --timesteps controls the number of diffusion restoration steps. In this project, multiple values were tested to estimate the per-image required step count.

## Changing Blur Strength

The Gaussian blur strength used by DDRM was adjusted inside:

```external/ddrm/runners/diffusion.py```

Specifically, the blur/degradation setting in the DDRM runner was modified to test stronger Gaussian blur conditions. This was necessary for evaluating how restoration difficulty changes under different degradation severities.

Because DDRM is an external dependency, the cloned repository itself is not committed here. Only this README and .gitkeep are tracked.
