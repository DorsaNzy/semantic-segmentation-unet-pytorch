# semantic-segmentation-unet-pytorch
Semantic segmentation of synthetic geometric objects using a U-Net-style CNN in PyTorch.

## Configuration

This project was developed using Python and PyTorch in a Jupyter Notebook environment.

The dataset is not included in this repository. To run the notebook, place the dataset locally using the following structure:

```text
data/FlyingObjectDataset_10K/
├── training/
│   ├── image/
│   └── gt_image/
├── validation/
│   ├── image/
│   └── gt_image/
└── testing/
    ├── image/
    └── gt_image/
