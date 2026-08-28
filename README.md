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

# Semantic Segmentation using U-Net-style CNN in PyTorch

This project implements a semantic segmentation model for synthetic images containing simple geometric objects. The goal is to classify each pixel into one of four semantic classes: background, square, triangle, or circular object.

The project uses a U-Net-style encoder-decoder convolutional neural network for pixel-wise prediction. Instead of predicting one label for the whole image, the model predicts a segmentation mask where each pixel is assigned to a semantic class.

## Project Overview

Semantic segmentation is a computer vision task where each pixel in an image is classified into a predefined category. In this project, the input is an RGB image containing multiple synthetic geometric objects, and the output is a pixel-wise segmentation mask.

The task is shape-based semantic segmentation. This means the model should learn to recognize the shape of each object and assign the correct semantic class to each pixel, rather than relying only on the visible color of the object.

The project builds on core computer vision concepts such as:

- Convolutional neural networks
- Feature extraction
- Image-based learning
- Data preprocessing
- Data augmentation
- Pixel-wise classification
- Model evaluation using segmentation metrics

## Dataset

This project uses synthetic images from the Flying Object dataset. The images contain simple geometric objects such as squares, triangles, and circular objects.

Each input image is paired with a corresponding ground-truth segmentation mask.

Expected dataset components:

- `image/`: RGB input images
- `gt_image/`: ground-truth segmentation masks

Unlike image-level classification, where each image has one label, this project performs pixel-wise classification. Each pixel in the output mask is assigned to one of four semantic classes.

## Class Mapping

The segmentation masks use fixed class labels for each object type.

| Class ID | Class Name |
|---|---|
| 0 | Background |
| 1 | Square |
| 2 | Triangle |
| 3 | Circular object |

The RGB input images may contain objects with different visual appearances, while the segmentation masks use fixed class labels to represent object categories.

## Dataset Preparation

The dataset was prepared by matching each RGB input image with its corresponding ground-truth mask. The input image is used as the model input, while the mask is used as the target label for supervised semantic segmentation.

For training, the images and masks are loaded together so that each image-mask pair remains aligned. This is especially important when applying geometric transformations such as flipping, cropping, or rotation, because the same transformation must be applied to both the image and the mask.

The dataset is split into three parts:

- Training set: used to train the model
- Validation set: used to monitor performance during training
- Test set: used for final evaluation

The dataset itself is not included in this repository. To run the notebook, place the dataset locally using the expected folder structure described in the Configuration section.

## Model Architecture

The model is based on a U-Net-style encoder-decoder CNN architecture.

The architecture includes:

- Encoder blocks for feature extraction
- Bottleneck layer for high-level representation
- Decoder blocks for upsampling
- Skip connections to preserve spatial details
- Transposed convolutions for upsampling
- 1x1 convolution for pixel-wise classification

The encoder learns spatial features from the input image, while the decoder reconstructs the segmentation mask at the original image resolution. Skip connections help preserve fine-grained spatial information, which is important for accurate pixel-level segmentation.

## Training

The model was trained using a supervised learning approach. The RGB image is passed as input, and the ground-truth segmentation mask is used as the target.

Training configuration:

- Loss function: CrossEntropyLoss
- Optimizer: Adam
- Learning rate: 1e-3
- Batch size: 4
- Epochs: 15

CrossEntropyLoss is suitable for multi-class semantic segmentation because it computes the classification loss for each pixel.

## Data Augmentation

Data augmentation was used to increase variation in the training data and improve model robustness. Augmentation helps the model become less sensitive to changes in object position, scale, orientation, and small visual variations.

The augmentation pipeline includes transformations such as:

- Random resized cropping
- Horizontal flipping
- Vertical flipping
- Rotation
- Gaussian blur

For semantic segmentation, augmentations must be applied carefully. Geometric transformations must be applied to both the input image and the segmentation mask so that the pixel-level labels remain aligned with the objects.

The project compares model performance with and without data augmentation. The non-augmented model achieved slightly higher performance on the clean test set, while the augmented model was considered more robust to visual variation.

## Evaluation

The model was evaluated using pixel-level segmentation metrics. Since semantic segmentation predicts a class for every pixel, evaluation is performed at the pixel level rather than only at the image level.

The evaluation includes:

- Pixel-wise accuracy
- Precision
- Recall
- F1-score
- Intersection over Union (IoU)
- Dice score
- Confusion matrix

Pixel accuracy alone can be misleading in segmentation tasks because the background class may dominate the image. For this reason, IoU and Dice score were also used to provide a more reliable evaluation of segmentation quality.

## Results

The model achieved strong segmentation performance on the test set.

Reported results without data augmentation:

| Metric | Value |
|---|---|
| Test Pixel Accuracy | 99.96% |
| Mean IoU | 0.9969 |
| Mean Dice | 0.9985 |
| Macro Average F1-score | 0.9985 |
| Weighted Average F1-score | 0.9996 |

Comparison with data augmentation:

| Metric | Without Augmentation | With Augmentation |
|---|---:|---:|
| Test Pixel Accuracy | 99.96% | 99.73% |
| Mean IoU | 0.9969 | 0.9833 |
| Mean Dice | 0.9985 | 0.9915 |

The results show that the model performs very well on the clean test set. The augmented version has slightly lower clean-test metrics, but the use of augmentation can support better robustness to variations in position, scale, orientation, and image quality.

## Repository Structure

```text
semantic-segmentation-unet-pytorch/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── semantic_segmentation_unet_clean.ipynb
│
└── docs/
    └── semantic_segmentation_presentation.pdf
