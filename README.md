# PDF-Estimation-using-GAN

# Overview

This assignment focuses on learning an unknown probability density function (PDF) purely from data samples, without assuming any analytical or parametric form. A Generative Adversarial Network (GAN) is used to implicitly model the distribution of a transformed random variable derived from real-world air quality data(Indian Air Quality Data).
The dataset used contains NO₂ concentration values, which are transformed using a nonlinear function. The transformed data is then used to train a GAN to learn the underlying probability distribution.


# Dataset Description

Dataset used:Indian Air Quality Dataset
You can find the dataset here:https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data
We use the NO2 concentration feature for our assignment because NO₂ data is continuous, real-valued, and exhibits a non-Gaussian distribution, making it suitable for density estimation using generative models.It has 16233 missing values which are removed before further processing.


# Methodology

Step 1: Data Transformation
Each NO₂ concentration value x is transformed into a new variable z using the following nonlinear transformation:
z=x+ar​⋅sin(br​⋅x)

Where:
->ar=0.5*(r%7),Calculated value:2.0
->br=0.3*(r%5+1)Calculated value:0.3
->r=My University roll number


# Step 2:GAN-Based PDF Learning

# GAN Architecture:
The Generative Adversarial Network (GAN) consists of two neural networks: a Generator and a Discriminator.

The Generator takes random noise sampled from a normal distribution and converts it into synthetic samples of the transformed variable z. It is implemented using a fully connected neural network with ReLU activations and outputs a single scalar value. Its goal is to generate samples that resemble real data.

The Discriminator is also a fully connected neural network that takes a single value as input and outputs a probability indicating whether the sample is real or generated. It uses a sigmoid activation in the final layer to perform binary classification.

Both networks are trained simultaneously in an adversarial manner: the discriminator learns to distinguish real and fake samples, while the generator learns to fool the discriminator. Through this competition, the generator implicitly learns the underlying probability distribution of z without assuming any parametric form.

# Training GAN:

Loss Function: Binary Cross-Entropy Loss
Optimizer: Adam

Training Strategy:
Discriminator is trained using real samples and generated samples
Generator is trained to maximize the discriminator’s belief that generated samples are real

Training continues until both networks reach a stable equilibrium.

# PDF Estimation from Generated Samples

After training:

A large number of samples are generated from the trained generator.
The probability density function is estimated using:
->Histogram-based density estimation
This estimated PDF represents the learned distribution of the transformed variable z.

# Result:
A Histogram comparing the Real PDF and GAN generated PDF
<img width="718" height="506" alt="image" src="https://github.com/user-attachments/assets/a8a7ee76-29be-4351-91c4-b3e68b94f078" />

# Observations:
# 1)Mode Coverage:
The generator successfully captures the main regions where the real data is concentrated. The generated samples span the same major ranges as the real samples, indicating good mode coverage.
# 2)Training Stability:
<img width="500" height="154" alt="image" src="https://github.com/user-attachments/assets/abe19122-4e69-4905-babf-a161adc53789" />

# Discriminator Loss (D Loss)
->The discriminator loss stays roughly around 1.3–1.5
->It does not collapse to 0 (which would mean discriminator is too strong)
->It does not explode to very large values
Therefore,The discriminator is neither overpowering nor failing.

# Generator Loss (G Loss)
->Generator loss fluctuates between ~0.6 and ~1.0
->These fluctuations are expected in adversarial training
->No continuous increase or collapse is observed.

Thus,The generator is learning gradually and adjusting to the discriminator’s feedback.

The discriminator and generator losses remain bounded and oscillate smoothly, indicating stable and balanced GAN training.

# 3)Quality of Generated Distribution:

The quality of the generated distribution is evaluated by comparing the histogram of GAN-generated samples with the real data distribution. From the plot, it can be observed that both distributions share a similar shape and are concentrated in the same range of values. The generator successfully captures the main structure of the data, although slight differences in peak height and tail behavior are present. Overall, the GAN produces realistic samples and effectively learns the underlying probability distribution.
