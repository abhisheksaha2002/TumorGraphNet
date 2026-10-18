# TumorGraphNet: Graph Neural Networks for MRI Analysis

TumorGraphNet is an exploratory research project that applies graph-based deep learning techniques to MRI images for brain tumor analysis. The project combines image segmentation, graph construction, graph neural networks, and unsupervised clustering to explore structural patterns in MRI data.

## Overview

Traditional image-processing methods analyze MRI images at the pixel level. TumorGraphNet explores a graph-based approach in which an MRI image is divided into superpixels, and each superpixel is represented as a node in a graph.

Node features and graph relationships are then processed using a dual-graph neural network architecture to learn representations of the image.

## Methodology

The current notebook implements the following pipeline:

1. **DICOM image loading:** Loads MRI images from a local DICOM dataset using `pydicom`.
2. **SLIC superpixel segmentation:** Divides each MRI image into smaller, spatially coherent regions using Simple Linear Iterative Clustering (SLIC).
3. **Node feature extraction:** Extracts mean pixel intensity and centroid coordinates for each superpixel.
4. **Graph construction:** Represents superpixels as graph nodes and creates connections based on spatial adjacency and feature similarity.
5. **Graph representation learning:** Uses Graph Attention Network (GAT) layers for spatial graph encoding and Graph Isomorphism Network (GIN) layers for feature learning.
6. **Feature reconstruction:** Trains a graph autoencoder-style model to reconstruct node features using reconstruction and graph smoothness losses.
7. **Unsupervised analysis:** Uses PCA to visualize learned embeddings and K-Means to cluster superpixels.
8. **Visualization and evaluation:** Displays superpixel maps, graph structures, cluster assignments, reconstructed features, and reconstruction errors.

## Model Architecture

The model contains two graph encoders:

- **GAT encoder:** Learns representations using graph connectivity and attention-based neighborhood aggregation.
- **GIN encoder:** Learns representations through graph-based neighborhood aggregation.

The encoder outputs are combined through element-wise addition. A neural network decoder then reconstructs the input node features.

The current notebook uses reconstruction-based learning rather than a fully supervised tumor segmentation pipeline.

## Technologies Used

- Python
- NumPy and SciPy
- pydicom
- Matplotlib
- scikit-image
- NetworkX
- PyTorch
- PyTorch Geometric
- scikit-learn
- pandas

## Dataset

The notebook uses MRI images stored in DICOM format.

The dataset is **not included in this repository**. You must obtain access to the dataset separately and configure the notebook to point to your local dataset directory.

For example, the expected directory structure can be:

```text
data/
└── MR/
    └── image_MR/
        ├── image_001.dcm
        ├── image_002.dcm
        └── ...
```

The filenames above are illustrative. Your dataset may use a different naming convention.

Configure the dataset path in the notebook to match your local setup. Do not commit medical images or identifiable patient information to a public repository.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/abhisheksaha2002/TumorGraphNet.git
cd TumorGraphNet
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv .venv
```

On Windows:

```powershell
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

PyTorch and PyTorch Geometric versions should be compatible with your Python version and hardware. Consult their official installation instructions if you encounter installation issues.

### 4. Configure the dataset path

Update the `dicom_dir` variable in the notebook to point to the directory containing your DICOM images.

For a portable setup, you can use a relative path such as:

```python
from pathlib import Path

dicom_dir = Path("data/MR/image_MR")
```

Alternatively, configure a local environment variable to point to the dataset wherever it is stored.

### 5. Run the notebook

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `Tumor_graph_net.ipynb` and execute the cells in order.

## Outputs

The notebook includes visualizations and analyses such as:

- Original MRI images and SLIC superpixel maps
- Graph visualizations overlaid on MRI images
- Graph node and edge statistics
- Learned embedding visualizations using PCA
- Original versus reconstructed node features
- Reconstruction error measurements
- K-Means clustering of superpixel embeddings

The trained model weights can be saved to `tumorgraphnet_model.pt`.

## Current Limitations

- The implementation is exploratory and has not been established as a clinically validated tumor segmentation system.
- Reconstruction error measures how well node features are reconstructed; it does not independently measure tumor segmentation accuracy.
- K-Means clustering produces unsupervised cluster assignments, which do not automatically correspond to tumor and non-tumor regions.
- Clinical segmentation performance requires appropriate ground-truth masks, quantitative evaluation metrics, and validation on suitable datasets.
- The current notebook processes selected MRI images and should be further developed and tested before drawing conclusions about generalization.

## Future Work

Potential directions for further development include:

- More robust spatial and feature-based graph construction
- Integration of ground-truth segmentation masks
- Quantitative evaluation using Dice score, Intersection over Union (IoU), and other relevant metrics
- Experiments across multiple MRI scans and patients
- Improved training, validation, and reproducibility
- Comparison with established segmentation baselines

## Disclaimer

This repository is intended for research and educational purposes only. It is not intended for clinical diagnosis or treatment decisions.

## License

This project is distributed under the MIT License. See the `LICENSE` file for details.