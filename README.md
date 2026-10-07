# CS5720 Neural Network and Deep Learning - Home Assignment 2

## Student Information

* **Name:** Ruyi Gai
* **Student ID:** 700778329
* **Course:** CS5720 Neural Network and Deep Learning
* **Semester:** Fall 2026
* **University:** University of Central Missouri


## Files

```text
Home Assignment 3.docx
Assignment 3.ipynb
README.md
```


### `Home Assignment 3.docx`

This document contains: Answers to the short-answer questions, code and results screenshots

### `Assignment 3.ipynb`

The Jupyter Notebook contains:

* The implementation of 2-D convolution from scratch using NumPy
* The pretrained ResNet18 transfer learning experiments
* Frozen feature extraction and fine-tuning
* Training time and validation accuracy results
* Training loss visualization


## Assignment Overview

This assignment focuses on Convolutional Neural Networks (CNNs), representation learning, and transfer learning.

The assignment covers convolution, padding, stride, CNN architectures including AlexNet, VGGNet, GoogLeNet, and ResNet, as well as representation learning and transfer learning.

The programming portion includes implementing 2-D convolution from scratch and comparing frozen feature extraction with fine-tuning using a pretrained ResNet18 model.


## Part I: Short-Answer Questions

### Question 1: Convolution, Padding, and Stride

A convolutional layer with an input size of `32 × 32 × 3`, 16 filters of size `5 × 5`, stride `1`, and padding `0` was analyzed.

The parameters are:

```text
N = 32
F = 5
S = 1
P = 0
```

The output spatial dimensions are:

```text
28 × 28
```

Since 16 filters are used, the complete output volume is:

```text
28 × 28 × 16
```

To preserve the original spatial dimensions of `32 × 32`, the required padding is:

```text
P = 2
```


## Question 2: CNN Architectures

This question discusses several important CNN architectures.

### AlexNet

AlexNet demonstrated that deep CNNs could achieve strong image recognition performance using GPUs and large-scale datasets such as ImageNet.

### VGGNet

VGGNet uses multiple small `3 × 3` convolutional filters instead of very large filters.

Three `3 × 3` convolutional layers provide approximately the same receptive field as one `7 × 7` layer while using fewer parameters and introducing more nonlinearities.

### GoogLeNet

GoogLeNet introduced the Inception module.

The Inception module performs several operations in parallel, including different convolution sizes and pooling, and then concatenates their outputs.

### ResNet

ResNet introduced residual learning and skip connections.

Residual connections help address the degradation problem and make very deep neural networks easier to train by improving gradient flow.


## Question 3: Representation Learning and Transfer Learning

Representation learning allows neural networks to automatically learn useful features from data.

Lower CNN layers usually learn simple features such as edges and textures, while higher layers learn more complex features such as object parts and complete objects.

Transfer learning uses a pretrained model as the starting point for a new dataset or task.

Fine-tuning allows some or all pretrained parameters to be updated for the new task.

Freezing a layer keeps its pretrained parameters fixed, while fine-tuning allows those parameters to change during training.


## Part II: Programming Questions

## Question 1: Implement Convolution from Scratch

A 2-D convolution operation was implemented using NumPy without using a built-in convolution function.

The input matrix was:

```text
1 1 1 0 0
0 1 1 1 0
0 0 1 1 1
0 0 1 1 0
0 1 1 0 0
```

The filter was:

```text
1 0 1
0 1 0
1 0 1
```

The convolution used:

```text
Stride = 1
Padding = 0
```

The filter was manually moved across every valid location of the input matrix. At each location, the corresponding elements were multiplied and summed.

The resulting output feature map was:

```text
[[4 3 4]
 [2 4 3]
 [2 3 4]]
```

The output shape was:

```text
(3, 3)
```

If the stride changes from `1` to `2`, the filter moves two positions at a time instead of one. Therefore, fewer locations are evaluated and the output feature map becomes smaller. In this example, the output shape changes from `3 × 3` to `2 × 2`.


## Question 2: Transfer Learning - Freeze vs. Fine-Tune

A pretrained **ResNet18** model was used to compare frozen feature extraction and fine-tuning.

The **Hymenoptera dataset** was used for image classification and contains two classes:

* Ants
* Bees

The dataset used in the experiment contains:

```text
Training images:   244
Validation images: 153
```

### Dataset Download

The dataset can be downloaded from the official PyTorch website:

https://download.pytorch.org/tutorial/hymenoptera_data.zip

After extraction, the dataset structure is:

```text
hymenoptera_data/
├── train/
│   ├── ants/
│   └── bees/
└── val/
    ├── ants/
    └── bees/
```


### Training Settings

The following settings were used for both experiments:

```text
Model:         ResNet18
Classes:       2
Batch Size:    16
Epochs:        5
Learning Rate: 0.0001
Loss Function: CrossEntropyLoss
Optimizer:     Adam
Image Size:    224 × 224
```


### Experiment A: Frozen Feature Extractor

A pretrained ResNet18 model was loaded and all pretrained layers were frozen.

The original final classification layer was replaced with a new classifier for the two classes:

```text
Pretrained ResNet18 Layers
           ↓
        Frozen
           ↓
New Classification Layer
           ↓
       Trainable
```

Only the new final classification layer was trained.

The number of trainable parameters was:

```text
1,026
```


### Experiment B: Fine-Tuning

A new pretrained ResNet18 model was loaded.

The early layers were frozen, but the final convolutional block (`layer4`) was unfrozen.

The final classification layer was also replaced and trained.

```text
Early ResNet18 Layers
           ↓
        Frozen
           ↓
        layer4
           ↓
       Trainable
           ↓
New Classification Layer
           ↓
       Trainable
```

The number of trainable parameters was:

```text
8,394,754
```


## Experimental Results

| Method | Trainable Parameters | Training Time | Validation Accuracy |
|---|---:|---:|---:|
| Frozen Feature Extractor | 1,026 | 52.77 seconds | 80.39% |
| Fine-Tuned Network | 8,394,754 | 79.32 seconds | 92.81% |


## Training Loss

The training loss was recorded for both approaches over 5 epochs.

The frozen feature extractor showed a gradual decrease in training loss.

The fine-tuned network showed a much faster decrease in training loss and achieved a lower final training loss.

The loss curves were plotted using Matplotlib to compare the two approaches.


## Results Discussion

The frozen feature extractor had fewer trainable parameters because only the final classification layer was trained. Therefore, it required less training time. The fine-tuned network had many more trainable parameters because the last convolutional block and classifier were both updated. Fine-tuning usually takes longer because more parameters require gradient calculations and updates. However, fine-tuning allows the pretrained features to adapt to the new ants-and-bees dataset. This improved the validation accuracy compared with using completely frozen features. Overall, feature extraction was faster, while fine-tuning achieved better performance.


## Technologies Used

* **Python**
* **NumPy**
* **PyTorch**
* **Torchvision**
* **Matplotlib**
* **Jupyter Notebook**
* **ResNet18**


## Conclusion

This assignment provided practical experience with convolutional neural networks and transfer learning.

The short-answer questions reviewed convolution, padding, stride, major CNN architectures, representation learning, and transfer learning.

The first programming task demonstrated how a convolution operation can be implemented manually by sliding a filter across an input matrix and calculating the output feature map.

The second programming task demonstrated two transfer learning approaches using a pretrained ResNet18 model. The frozen feature extractor required less training time, while the fine-tuned model achieved higher validation accuracy.

Overall, this assignment demonstrated how pretrained CNN models can be adapted to new image classification tasks using feature extraction and fine-tuning.
