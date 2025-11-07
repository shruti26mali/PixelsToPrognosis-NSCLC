# CHAIMELEON - Lung cancer use case

This project implements a comprehensive medical image analysis pipeline for lung cancer patients, including tumor segmentation, mediastinal node analysis, coronary artery analysis, and survival prediction.

## Project Structure

```
├── codes/                              # Analysis notebooks
│   ├── 1_nnUNetv2_predict.ipynb       # Segmentation predictions
│   ├── 2_extract_*_HRFs.ipynb         # Feature extraction
│   ├── 2_get_CAC_scores.ipynb         # Coronary calcium scoring
│   └── 3_OS_prediction_*.ipynb        # Survival prediction models
├── models/                             # Trained models
├── nnunet_input_images/               # Input CT images
├── preds_*/                           # Segmentation results
└── raw_images/                        # Original CT scans
```

## Prerequisites

- Python 3.10
- CUDA-compatible GPU
- Required Python packages:
  - nnUNetv2
  - TotalSegmentator
  - PyTorch
  - nibabel

## Installation

1. Create a new conda environment:
```bash
conda create -n nnunet python=3.10
conda activate nnunet
```

2. Install PyTorch:
```bash
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu126
```

3. Install nnUNetv2:
```bash
pip install nnunetv2
```

4. Set up environment variables:
```bash
set nnUNet_raw=D:\path\to\project\nnUNet_raw
set nnUNet_preprocessed=D:\path\to\project\nnUNet_preprocessed
set nnUNet_results=D:\path\to\project\nnUNet_results
```

## Usage

### 1. Image Preprocessing and Segmentation

Run `1_nnUNetv2_predict.ipynb` to:
- Prepare input images for nnUNet format
- Perform lung tumor segmentation
- Segment mediastinal nodes
- Extract coronary arteries
- Segment whole lung regions

### 2. Feature Extraction

Run the following notebooks to extract features:
- `2_extract_coronary_arteries_HRFs.ipynb`: Extract coronary artery features
- `2_extract_mediastinal_nodes_HRFs.ipynb`: Extract lymph node features
- `2_extract_tumor_HRFs.ipynb`: Extract tumor features
- `2_extract_whole_lung_HRFs.ipynb`: Extract lung parenchyma features
- `2_get_CAC_scores.ipynb`: Calculate coronary artery calcium scores

### 3. Survival Analysis

The following notebooks perform survival analysis using different feature sets:
- `3_OS_prediction_CA_texture.ipynb`: Using coronary artery features
- `3_OS_prediction_CAC_scoring.ipynb`: Using calcium scores
- `3_OS_prediction_MN_texture.ipynb`: Using mediastinal node features
- `3_OS_prediction_MN_volume.ipynb`: Using node volumetric features
- `3_OS_prediction_tumor_texture.ipynb`: Using tumor texture features
- `3_OS_prediction_tumor_volume.ipynb`: Using tumor volumetric features
- `3_OS_prediction_whole_lung_texture.ipynb`: Using lung parenchyma features

## Data Files

- `test_radiomics_CA.csv`: Coronary artery radiomics features
- `test_radiomics_CAC_scores.csv`: Coronary calcium scores
- `test_radiomics_lung_tumor.csv`: Tumor radiomics features
- `test_radiomics_MN.csv`: Mediastinal node features

## Downloading Required Models

All required models are available at: [Surfdrive Model Repository](https://surfdrive.surf.nl/s/nSfx2xNR3GTNEGN)

The repository contains two main folders:

### 1. Pre-trained CoxPH Survival Models (`models/`)
Cox Proportional Hazards (CoxPH) survival prediction models for different regions of interest (ROIs):
- `coronary_arteries_texture/`: CoxPH models for coronary artery texture features
- `CAC_scoring/`: CoxPH models using coronary artery calcium scores
- `mediastinal_nodes_texture/`: CoxPH models using lymph node texture features
- `mediastinal_nodes_volume/`: CoxPH models using lymph node volumetric features
- `tumor_texture/`: CoxPH models using tumor texture features
- `tumor_volume/`: CoxPH models using tumor volumetric features

These models are used in the corresponding `3_OS_prediction_*.ipynb` notebooks to predict overall survival based on different imaging biomarkers.

### 2. nnUNet Segmentation Models (`nnUNet_results/`)
The project uses two specific nnUNet models for segmentation:

1. **Mediastinal Node Segmentation (Dataset017_LNQ2023)**
   - Used for lymph node quantification
   - Extract to: `nnUNet_results/Dataset017_LNQ2023/`
   - Used in mediastinal nodes prediction with:
     ```bash
     nnUNetv2_predict -i INPUT_DIR -o OUTPUT_DIR -d 017 -c 3d_fullres -chk checkpoint_best.pth
     ```

2. **NSCLC Tumor Segmentation (Dataset800_LUNGTUMOR)**
   - Used for lung tumor segmentation
   - Extract to: `nnUNet_results/Dataset800_LUNGTUMOR/`
   - Used in tumor prediction with:
     ```bash
     nnUNetv2_predict -i INPUT_DIR -o OUTPUT_DIR -d 800 -c 3d_fullres -chk checkpoint_best.pth
     ```

### Installation Steps:
1. Download all models from the Surfdrive link above
2. Extract the `models` folder to the root of your project directory
3. Extract the `nnUNet_results` folder to the root of your project directory
4. Verify the folder structure matches the project structure shown above

## License

For TotalSegmentator usage, a license key is required. Set it using:
```bash
totalseg_set_license -l your_license_key
```

## Notes

- Ensure sufficient GPU memory for running the segmentation models
- Input images should be in NIFTI format (.nii.gz)
- All paths in notebooks should be adjusted according to your local setup
