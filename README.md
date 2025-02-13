# Conditional Diffusion Models for CIFAR-10 Image Generation
This project explores Conditional Diffusion Models for generating CIFAR-10-like images. Diffusion models are a class of generative models that iteratively denoise data to generate high-quality samples. This implementation compares two architectures: a simple UNet and an Attention-Based UNet, demonstrating their effectiveness in image synthesis.

## Table of Contents
- [Model Architecture](#model-architecture)
   - [Simple UNet](#simple-unet-model)
   - [Attention-Based UNet](#attention-based-unet-model)
- [Simple UNet Model](#simple-unet-model)
   - [Config](#config)
   - [Forward Sample](#forward-sample)
   - [Backward Sample (Class: Horse)](#backward-sample-class-horse)
   - [Final Generated Samples](#final-generated-samples)
- [Attention-Based UNet Model](#attention-based-unet-model)
   - [Config](#config-1)
   - [Loss Curve](#loss-curve)
   - [Generated Samples Per Each Epoch](#generated-samples-per-each-epoch)
- [Conclusion](#conclusion)
- [Future Work](#future-work)

   
## Model Architecture

![Model Architecture](https://github.com/user-attachments/assets/a4b4402e-49e6-4322-80e9-144a972e6e1f)

The model architecture consists of two main components:
1. **DDPM Scheduler**: Adds noise to images over a series of timesteps, following a predefined noise schedule.
2. **Conditional UNet**: Predicts and removes the noise from the noisy images. The model is conditioned on both the image class and timestep embeddings, which guide the generation process.

Two variants of the UNet are implemented:
- **Simple UNet**: A baseline architecture without attention mechanisms.
- **Attention-Based UNet**: Incorporates attention layers to improve feature extraction and generation quality.

## Simple UNet Model
The Simple UNet is a baseline architecture without attention mechanisms. While straightforward and less computationally expensive, it struggled to generate high-quality images, even after 200 epochs of training. The generated samples often lacked fine details and exhibited artifacts, highlighting the limitations of a basic UNet for this task.

### Config
```python
config = {
    "beta_start": 1e-4,
    "beta_end": 0.02,
    "steps": 1000,
    "device_id": 0,
    "image_size": 32,
    "image_channel": 3,
    "epochs": 200,
    "lr": 3e-4,
    "weight_decay": 0,
    "batch_size": 200,
    "num_class": 10,
    "pos_dim": 1024,
    "patience": 10
}
```
The configuration includes the following key parameters:
- `beta_start` and `beta_end`: Define the noise schedule for the diffusion process.
- `steps`: Number of timesteps in the diffusion process.
- `image_size` and `image_channel`: Dimensions of the input images.
- `epochs`: Number of training epochs.
- `lr`: Learning rate for the optimizer.
- `batch_size`: Number of samples per batch.
- `num_class`: Number of classes in CIFAR-10.
- `pos_dim`: Dimension of the positional embeddings for timesteps.
- `patience`: Early stopping patience for training.


### Forward Sample
This image shows the progressive addition of noise to a CIFAR-10 image over the diffusion timesteps.

![Forward Sample](https://github.com/user-attachments/assets/5b331e65-6da3-4ee2-ae1d-79e563b54594)

### Backward Sample (Class: Horse)
This image demonstrates the denoising process for a specific class (horse) over the diffusion timesteps

![Backward Sample](https://github.com/user-attachments/assets/4a4fd009-3602-42a1-bd02-0048cc63e5c0)


### Final Generated Samples
These are the final images generated by the model after training.

![71306245-e575-443d-93d1-a87989b85398](https://github.com/user-attachments/assets/c2fe97fa-d01d-4659-9aa8-f2ffda66dda0)


## Attention-Based UNet Model
The Attention-Based UNet incorporates attention mechanisms into the UNet architecture, allowing the model to focus on relevant features during the denoising process. This results in faster convergence and higher-quality image generation compared to the simple UNet.

### Config
```python
config = {
    "beta_start": 1e-4,
    "beta_end": 0.02,
    "steps": 1000,
    "device_id": 0,
    "image_size": 32,
    "image_channel": 3,
    "epochs": 50,
    "lr": 3e-4,
    "weight_decay": 0,
    "batch_size": 200,
    "num_class": 10,
    "pos_dim": 1024,
    "patience": 10
}
```

### Loss Curve
![image](https://github.com/user-attachments/assets/b9895853-3fd9-4946-aad3-0c304faab7c8)


## Generated Samples Per Each Epoch
![generated_images_epoch_1](https://github.com/user-attachments/assets/dd24fbd2-ab74-4f72-8750-87743502b66a)

![generated_images_epoch_5](https://github.com/user-attachments/assets/781a5455-c6d1-49d3-b146-77ae21e59a44)

![generated_images_epoch_10](https://github.com/user-attachments/assets/8c5cb22c-4b11-4c2c-bead-c2789eb5b843)

![generated_images_epoch_15](https://github.com/user-attachments/assets/6cf39c02-53e1-461c-b189-ecfd36d16ee8)

![generated_images_epoch_20](https://github.com/user-attachments/assets/9e74e39b-b1bc-4f7b-85c1-b6a3445b5d71)

![generated_images_epoch_30](https://github.com/user-attachments/assets/1143c1ca-6da2-41ea-bbc3-3ee1bcecb845)

![generated_images_epoch_35](https://github.com/user-attachments/assets/10406207-177d-41d1-8bd9-d222fd5719ae)

![generated_images_epoch_40](https://github.com/user-attachments/assets/6c3eba48-434c-42a8-9990-521d23a66e10)

![generated_images_epoch_45](https://github.com/user-attachments/assets/6d037da4-fc67-4ffc-80c9-cff91a2fe699)

![generated_images_epoch_50](https://github.com/user-attachments/assets/180b3864-2a64-4341-9440-5a6aafdf36ff)


## Conclusion
The Attention-Based UNet outperforms the simple UNet in terms of convergence speed and image quality. With only 50 epochs of training, the Attention-Based UNet generates images that are significantly better than those produced by the simple UNet after 200 epochs. This highlights the importance of attention mechanisms in improving the performance of diffusion models.

## Future Work
Future work will focus on evaluating the quality of the generated images using the [Fréchet Inception Distance (FID) score](https://github.com/mseitzer/pytorch-fid). FID measures the similarity between the generated images and the real CIFAR-10 dataset, providing a quantitative metric for assessing the performance of the models. Additionally, more training and hyperparameter tuning are needed to further improve the model's performance. Further exploration of architectural improvements, such as incorporating advanced attention mechanisms or hybrid architectures, could also lead to even better results.
