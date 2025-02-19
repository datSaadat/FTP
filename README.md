# Forward Target Propagation: An Efficient Method of Forward Learning in Neural Networks
This repository contains the spreadsheet("MACC_analysis_FTP.ods") of the quantitative analysis performed for the paper "Forward Target Propagation: An Efficient Method of Forward Learning in Neural Networks". 


## Brief description
This analysis focuses on evaluating the computational complexity and training time of various learning procedures applied to different models across multiple tasks and datasets. Specifically, the study examines Backpropagation (BP), Forward-Forward (FF), PEPITA (PEP), and Forward Target Propagation (FTP) to determine their efficiency and scalability. These learning procedures were tested on four distinct models: DS-CNN, MobileNet, ResNet, and AutoEncoder (AE). Each model was trained on a corresponding dataset—Speech Commands (SC), Visual Wake Words (VWW), CIFAR-10, and ToyADMOS—selected based on the nature of the tasks involved. The architectures and source code of these models are openly accessible in the [mlcommons/tiny repository](https://github.com/mlcommons/tiny), providing transparency and reproducibility. For an in-depth understanding of the methodology and results, refer to the original paper.

## Methodology
### Assumptions
The analysis is based on the following key assumptions:
- Quantization: Weights, biases, and activations are quantized to INT8. The softmax layer, which is present only in CNN models, is quantized to FLOAT32. While the multiply-accumulate operations (MACCs) in the softmax layer should technically be performed in FLOAT32, their overall computational impact within the considered neural networks is minimal. To simplify the calculations, the softmax layer computations are assumed to be INT8.
- Batch Normalization: The batch normalization layers included in the original models were excluded from the analysis.

### MACCs procedure
As a first step, a closed-form equation was derived to compute the number of multiply-accumulate operations (MACCs) required by a generic layer (e.g., fully connected, convolutional, depthwise convolutional) to perform specific operations. The operations considered include the forward pass, backward pass, weight update, and additional computations unique to forward learning procedures. These include normalization and goodness evaluation in Forward-Forward (FF), error projection in PEPITA (PEP), and target propagation in Forward Target Propagation (FTP).

By defining the characteristics of each layer (e.g., input channels, kernel size), the MACCs required for each operation can be systematically calculated. Each layer’s specifications were recorded, and the corresponding formula was applied to determine the exact MACCs needed for every operation.


The MACCs for a given operation were summed across all layers to determine the total MACCs required by the entire model for that specific operation during training. Then, the total MACCs for each learning procedure were obtained by aggregating the MACCs of all the operations involved in the training process, including forward pass, backward pass, weight update, and additional computations specific to Forward-Forward (FF), PEPITA (PEP), and Forward Target Propagation (FTP).

To describe the characteristics of a layer the following notation has been used.
- C_in: no of channels of the previous layer
- C_out: no of channels of the current layer
- H_in: height of the feature map of the previous layer 
- W_in: width of the feature map of the previous layer
- H_out: height of the feature map of the current layer 
- W_out: width of the feature map of the current layer
- H_ker: height of the kernel
- W_ker: width of the kernel
- H_stide: stride value in vertical direction
- W_stride: stride value in horizontal direction
In Fully-Connected layers, H_in and H_out were used to indicate the number of activations of, respectively, the previous and the current layer. 


### Training latencies on MCUs

To estimate the training latency on the STM32H735G-DK and NUCLEO-G474RE MCUs for a single sample, the previously computed MACCs were multiplied by the cycles per MACC and divided by the processor frequency of the respective MCU. Using the total number of samples in the dataset and the number of epochs originally used for training, the overall training time was calculated and expressed in hours.


## Citation
If "Forward Target Propagation: An Efficient Method of Forward Learning in Neural Networks" contributes to your research, please consider citing it in your work.

