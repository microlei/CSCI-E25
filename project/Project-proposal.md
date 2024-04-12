# Project Proposal - Image classification of plankton

## Introduction

Plankton are microorganisms that form the base of the aquatic food chain. As fast-growing organisms, they are particularly sensitive to perturbations in the environment, and have been proposed as a biosensor for changing environmental conditions. In recent years, near-realtime automatic systems for capturing in-situ plankton images has generated large of plankton image data that are not feasible for humans to manually analyze. However, this large quantity of images has also created an opportunity to develop machine learning approaches to classify plankton species [1]. There are several publically available datasets of annotated plankton images, such as the WHOI, ZOOSCAN, and Kaggle (National Data Science Bowl) datasets. The challenges in classifying these plankton images involve class imbalance, class morphological variability, and bias in experts' annotations, among others. 

## Problem Statement

The combination of imbalanced class sizes and morphological variability poses the challenge of augmenting the data in such a way that mimics real variability while not introducing bias. Traditional data augmentation techniques such as rotation, flipping, and scaling may not be sufficient to capture the morphological variability of plankton and may introduce biased morphologies. For example, some diatoms are chain-forming while others are solitary and can vary in size. Chain length and major axis length are important variables to capture for these species as they vary over the length of the season[2]. For this project, I will compare the performance of traiditional data augmentation techniques with a more advanced technique using generative adversarial network (GAN). 

## Methodology

I will use the WHOI22 dataset [3], which contains 22 classes of plankton image and is a balanced subset of the WHOI-plankton set [4]. I will use a convolutional neural network (CNN) to classify the full balanced set. I will then generate two new datasets, one where images have been augmented with trational techniques such as rotation and flipping, and one where images are augmented using a GAN. For these datasets, images within the same class will be substituted for the augmented images, keeping the total balance of the classes constant, as well as the total number of training images. I will then train the CNN on these augmented datasets and compare the performance to the original balanced dataset. 

### Creating the GAN

The GAN will be trained on WHOI-plankton dataset (larger imbalanced model) subsampled to the 22 species present in WHOI22. The size of this dataset may also be subsampled due to storage constraints. My GAN will consist of two models, a Discriminator (D) and a Generator (G). The Generator will create fake images of plankton, and the Discriminator will try to distinguish between real and fake images. The D & G then compete against each other until the D can no longer tell the difference. The resulting generator will be used to augment the WHOI22 dataset. 

To optimize the GAN, I will experiment with different measures of similarity upon which the discriminator is trained, including the traditional Kullback–Leibler and Jensen–Shannon Divergence, as well as the Wasserstein distance. Additionally, I will assess an alternative to the binary discriminator and use a classifier as the discriminator. This discriminator will output real class labels, with an additional 'K' label for fake images. 

## Evaluation & Hypotheses

I will evaluate the performance of the CNN on the original balanced dataset, the dataset augmented with traditional techniques, and the dataset augmented with the various GANs. I will use the F1 score as the primary metric, as it is a good measure of the balance between precision and recall. I will also use the confusion matrix to assess the performance of the model on each class. Initially, I will only augment a single class, then multiple classes, to see how classification changes with the number of fake images within and between classes. I expect that the a good GAN will create better fake images than traditional augmentation techniques and that the resulting CNN will perform more closely to the original balanced dataset. I will also expect that over-augmentation using the GAN will lead to overfitting and loss of generalization, and will experiment with the number of fake images to find the optimal balance. A good GAN will create a variety of fake images, and thus will maintain similar performance to the original dataset even as the ratio of real to fake images is reduced.

## References

1. Ciranni, Massimiliano, Vittorio Murino, Francesca Odone, and Vito Paolo Pastore. “Computer Vision and Deep Learning Meet Plankton: Milestones and Future Directions.” Image and Vision Computing 143 (March 1, 2024): 104934. https://doi.org/10.1016/j.imavis.2024.104934.
2. Sonnet, Virginie, Lionel Guidi, Colleen B. Mouw, Gavino Puggioni, and Sakina-Dorothée Ayata. “Length, Width, Shape Regularity, and Chain Structure: Time Series Analysis of Phytoplankton Morphology from Imagery.” Limnology and Oceanography 67, no. 8 (2022): 1850–64. https://doi.org/10.1002/lno.12171.
3. Olson, Robert J., and Heidi M. Sosik. “A Submersible Imaging-in-Flow Instrument to Analyze Nano-and Microplankton: Imaging FlowCytobot.” Limnology and Oceanography: Methods 5, no. 6 (2007): 195–203. https://doi.org/10.4319/lom.2007.5.195.
4. Sosik, Heidi M., Emily E. Peacock, and Emily F. Brownlee. "Annotated plankton images-data set for developing and evaluating classification methods." URL http://darchive. mblwhoilibrary. org/handle/1912/7341 (2015).
