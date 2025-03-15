# CS229-Final-Project

This repository contains the code and experiments for our CS229 Final Project on detecting and mitigating image recapture (rebroadcast) attacks. The project explores both defense and attack strategies using deep learning and physics-based methods.

## Overview

Our work is split into two main components:

- **Defense:**  
  We implement three detection models:
  - An EXIF-based classifier using a Random Forest.
  - A ResNet-based classifier trained on original images.
  - A physics-based ResNet model trained on wavelet-transformed images that capture Moiré patterns.
  
- **Attack:**  
  We develop a camera simulator using a UNet architecture to mimic the recapture process. In addition, we build a reverse, symmetric UNet to generate adversarial examples that fool our detection models in a black-box setting.

## Defense

Our defense is three-fold:

- **EXIF-based Detection:**  
  Utilizes hand-picked camera metadata (e.g., shutter speed, ISO, color temperature) to build a Random Forest classifier. This method is robust against adversarial modifications since EXIF data is embedded in the C2PA signature.

- **ResNet on Original Images:**  
  A pre-trained ResNet-50 model is fine-tuned on the ICL Dataset ([ICL Dataset](https://www.commsp.ee.ic.ac.uk/~pld/research/Rewind/Recapture/)). This model achieves high accuracy by learning discriminative visual features.

- **ResNet on Wavelet Transformed Images:**  
  We preprocess images using the Wavelet Transform to isolate Moiré patterns typical of recaptured images. A separate ResNet-50 model is trained on these transformed images, offering a physics-based perspective.

## Attack

The attack implementation is contained in the `camera_sim_colab` notebook. We train:

- A **camera simulator** using a UNet backbone on the Chimera dataset to mimic the recapture process.
- An **adversarial generator** (reverse UNet) to transform recaptured images back to their original form, effectively bypassing our detection models.

Our experiments show that the adversarial generator can successfully fool our detectors, achieving 100% misclassification on the test set.
