# GAN-Driven-Polymer-Phase-Explorer

Advisor - Dr. Lenart Bill<br>
This is a project (inprogress) undertaken during my spring semester.<br>
The model training was done on Discovery Cluster (T4 GPU) provided by Northeastern University.<br>
It was also presented at the 2025 student showcase.

**Problem statement** -<br>
The discovery of new block polymer phases is crucial for advancing materials science and nanotechnology. However, traditional self-consistent field theory (SCFT) simulations are computationally expensive and heavily dependent on initial guesses, often limiting exploration to known phases. This project addresses the challenge of generating diverse and potentially novel initial guesses for SCFT simulations in block polymer phase discovery.

**Solution** -<br>
By implementing a Deep Convolutional Generative Adversarial Network (DCGAN) trained on a limited set of known polymer phases, we aim to create a generative model capable of producing a wide range of initial density field configurations. These generated configurations can serve as starting points for SCFT simulations, potentially leading to the discovery of new block polymer phases.

The project focuses on replicating the GAN architecture described in the "Gaming self-consistent field theory" paper which involves training the DCGAN on 3D density fields of established block polymer phases and generating a diverse set of initial guesses for potential use in SCFT simulations.

While the full pipeline of SCFT convergence testing is beyond the scope of this project (at the present moment), the successful generation of diverse and physically plausible initial guesses represents a significant step towards accelerating the discovery of novel block polymer phases. This approach combines machine learning techniques with polymer physics, potentially revolutionizing the field of soft matter research.

### Data

The dataset consists of 3D density fields of five established block polymer phases from Self-Consistent Field Theory (SCFT) trajectories.<br>
This dataset is unlabeled, as it's used for generative modeling rather than classification.<br>
**Features**: The data are 3D density fields represented on a regular grid, with values ranging from 0 to 1, treated as 3D grayscale images.<br>
The dataset is available in the Data Repository for U of M (DRUM) - [Link to dataset](https://conservancy.umn.edu/items/ba70d027-ba90-4497-9260-8800022654ff)<br>
**Data analysis**: The data consists of 3D image-like structures representing polymer density fields.

Data augmentation techniques were used, including - Tiling, Random translation and Rotation.<br>
These techniques were applied to diversify the training data and improve the GAN's generalizability.

Data preprocessing involved - <br>
1 - Excluding fields with unphysical density values (phi_A > 1 or phi_A < 0)1<br>
2 - Interpolation of density data to new grid points after transformation

### Training and Testing Process
This project uses a Deep Convolutional Generative Adversarial Network (DCGAN), which is an unsupervised learning model.

The GAN consists of two main components:<br>
Generator: Proposes new initial conditions for SCFT calculations<br>
Discriminator: Evaluates the generated samples<br>

The GAN generates initial guesses for SCFT calculations, but actual polymer convergence evaluation wasn't performed/added in this project.

The GAN architecture used for generative polymer field theory with SCFT - <br> 
Biases were incorporated in the 3D transposed convolutional layers (ConvTranspose3d) in the generator but were not included in 3D convolutional layers (Conv3d) in the discriminator.<br> 
Zero-padding was used for padding operations.<br> 
A negative slope of 0.2 was used for the Leaky ReLu (LReLu) activation function.<br> 

| Layer | Operation | Output Dimensions | Kernel Size | Stride | Padding | Batch Norm | Activation |
| ---- | ---- | ---- |  ---- | ---- | ---- | ---- | ---- |
| Generator input dimension: (100, 1, 1, 1) |
| G1 | ConvTranspose3d | (512, 4, 4, 4) | (4, 4, 4) | 1 | 0 | Yes | ReLU |
| G2 | ConvTranspose3d | (256, 8, 8, 8) | (4, 4, 4) | 2 | 1 | Yes | ReLU | 
| G3 | ConvTranspose3d | (128, 16, 16, 16) | (4, 4, 4) | 2 | 1 | Yes | ReLU | 
| G4 | ConvTranspose3d | (1, 32, 32, 32) | (4, 4, 4) | 2 | 1 | No | Sigmoid | 
| Discriminator input dimension: (1, 32, 32, 32) | 
| D1 | Conv3d | (64, 16, 16, 16) | (4, 4, 4) | 2 | 1 | Yes | LReLU | 
| D2 | Conv3d | (128, 8, 8, 8) | (4, 4, 4) | 2 | 1 | Yes | LReLU | 
| D3 | Conv3d | (256, 4, 4, 4) | (4, 4, 4) | 2 | 1 | Yes | LReLU | 
| D4 | Conv3d | (1, 1, 1, 1) | (4, 4, 4) | 1 | 0 | No | Sigmoid | 

While the full evaluation of the GAN-generated initial guesses for SCFT convergence was not performed in this project, we can estimate the potential effectiveness based on the results reported training and in the original paper.

The GAN training process showed significant progress over 80 epochs, with the discriminator and generator losses generally decreasing and stabilizing. The discriminator became better at distinguishing real and fake samples, while the generator improved in producing more convincing outputs. 
Based on the data from the original study, we can estimate a convergence rate of about 10.9%, with 545 out of 5000 SCFT calculations converging. The GAN demonstrated impressive generative capabilities, producing 349 candidate network phases, successfully generating all known network phases, and discovering novel network phases. This indicates that the GAN learned to generate a diverse range of initial guesses, including both known and potentially new polymer phases, which could significantly accelerate the discovery of novel block polymer structures.

### Analysis and Conclusions
**Analysis**<br>
The project demonstrates the potential of using GANs to generate initial guesses for SCFT calculations in polymer phase discovery.<br>
The GAN learned to generate diverse 3D density fields that could potentially seed new SCFT simulations.<br>
**Key points**:<br>
Data augmentation was crucial for training the GAN with limited known phases<br>
The approach could potentially discover new polymer phases beyond the training set<br>
Visual inspection of isosurfaces was used to evaluate GAN training progress<br>

**Conclusion**:<br> While the full pipeline of using GAN-generated guesses in SCFT simulations wasn't implemented, the project demonstrates the feasibility of using GANs to propose diverse initial conditions for polymer phase discovery. Further work would be needed to evaluate the effectiveness of these guesses in actual SCFT convergence.
