## Methods
### Generative Adversarial Networks
Generative Adversarial Networks (GANs) are a type of generative model composed of two different networks: the generator and the discriminator. The generator's objective is to generate fake data indistinguishible from real data. On the other hand, the discriminator deals with classifying whether a random sample has been artificially generated or is a real sample data. Both networks will compete during training in which the gain or loss of one of the networks is offset by the gain or loss of the opposite, till the discriminator cannot correctly identify between the samples.

The generator, U-Net, consists of an encoder-decoder architecture in which a convolution is applied after the last layer in the decoder to map the number of output channels, 170 channels in our case.
The discriminator will decide whether the unknown image was generated from the generator or not. The architecture is also called a PatchGAN and will decide if patches from the sub-image patches are real or not.
We will focus out study around the Pix2Pix model, a state-of-the-art framework for image translation based on GANs extracting correspondence features between pairs of images. This model is a general-purpose solution for image translation tasks and is sometimes limited due to this generalization, which makes it non-specialized in capturing relationships between original and constructed images of specific constraints, characteristics and abstractions.
Transforming Multispectral data to Hyperspectral data is a clear example of a task and data that differs from the standard example solutions in which Pix2Pix is often showed performing.
Pix2Pix is based on Conditional Generative Adversarial Network (CGAN), whose generator network learns a mapping from the input image $x$ to the target image y, i.e., x->y and the discriminator network will try to classify y|x, real or fake.

### SAM-GAN: Proposed enhancement
Most of the loss functions used in architectures are based on two-dimensional feature extraction, computing the error between data extracting them channel by channel. When dealing with hyperspectral data, the most important features and relationships to aim to be extracted are not only on the spatial dimension but on the spectral dimension too. In this study, we propose a loss function that gathers both high-level features from the spatial dimension and the long spectral dimension in order to achieve a solid learning of the structure of the HS data.
The loss function is based on pix2pix's loss function but adding the Spectral Angle Mapper (SAM) metric to minimize the errors of spectral features. SAM qualifies the similarity of the original and the transformed vector reflectance across the spectra through measuring the average angle between them.


### Evaluation metrics
The resulting generated imagery will be evaluated through three different evaluation approaches:

#### Visual interpretation
The visual interpretation and comparison of the results played an important role on the training of the data since we were able to easily interpret whether we had to tune the model because of blurriness issues or offset data. However, it will not be an important decisive factor in the final comparison since the visual interpretation of 170 bands can not be easily readable, although we can have a good glance of it by visualizing three bands composing the RGB channels in a False Color Composite (FCC).

#### Statistical metrics
Quality of the data will also be statistically evaluated. This evaluation will be separated in two classes: band-wise evaluation and pixel-wise evaluation.

Band-wise evaluation will consider the next metrics:
* Pearson’s Correlation Coefficient (PCC) measures the  linear correlation between the real and fake bands
* Root-Mean-Square Error (RMSE) measures the difference between reflectances of real and fake bands
* Peak Signal-to-Noise Ratio (PSNR) measures the quality of the reconstructed image
* Structural Similarity index (SSIM) assesses the visual impact of luminance, contrast, and structure characteristics of the predicted image into a single index metric

Regarding the pixel-wise evaluation, we used more task-specific metrics which take into account the 170-long vector of reflectances of a single pixel. The next metrics will be measured for the generated data:
* Spectral Angle Mapper (SAM) evaluates the difference between two spectra (real and fake pixel) by measuring the angle.
* Spectral Information Divergence (SID) is an information theoretic spectral metric which considers each pixel as a random variable and uses its spectral histogram to define a probability distribution. The spectral similarity between two pixels is measured by the discrepancy of probabilistic behaviours between their spectra.
