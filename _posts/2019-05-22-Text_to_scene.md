---
title: 'Text2Scene: Generating Compositional Scenes from Textual Descriptions'
date: 2019-05-22
modified: 2019-05-22
permalink: /posts/2019/05/text_to_scene/
tags:
  - paper
  - computer vision
  - deep learning
excerpt_separator: <!--more-->
---

The note of [*Text2Scene: Generating Compositional Scenes from Textual Descriptions*](https://arxiv.org/pdf/1809.01110)

<!--more-->

## Overview

Unlike the previous work, this paper doesn't adopt Generative adversarial network, but a combination of encoder-decoder architecture with a semi-parametric retrieval-based approach **TOREAD**. Under minor modification, this model performs decent generation of different forms of scene representation, including clip-art generation on *Abstract Scenes*, semantic layout on *COCO* and compositional image generation on *COCO*.

## Preliminaries

### Difficulties encountered

Generating rich textual representation has two main challenges, one is indirect hint at the presense of attributes from input textual description (e.g. "Mike is surprised" should change facial attributes on the generated object "Mike"), and another is relative spatial configurations within the text (e.g. "Mike is runing towards to his girlfriend" confines the orientation of "Mike" dependent on "his girlfriend")

### Related work

Relative work is as follows:

- [*Semi-parametric Image Synthesis*](https://arxiv.org/abs/1804.10992) proposed a retrieval-based semi-parametric method for image synthesis given an input by a human. But different from the previous work using ground-truth semantic layout as input, this work learns to **predict** the location and layout of the object indirectly from the text.

- [*Image generation from scene graphs*](), it proposed a graph-convolution model to generate from structured scene graph where objects and their relationship are provided as inputs, while this work the presence of objects is inferred from text.

- [*Inferring semantic layout for hierarchical text-to-image synthesis*]() generates the layout as the intermediate representations in separably trained modules, but this work generate pixel-wise outputs with semi-parametric retrieval module without advesarial training.

- [*Visual dialog for collaborative drawing*]() performs pictorial generation from chat logs, compared to out works that need text much considerably more underspecified. 

## Network structure

Using a sequence-to-sequence approach, this model arranges the generated object sequentially along with their attributes (locations, sizes, aspect ratios, pose, appearance and more) on an initially empty canvas.

Generally, Text2Scene model consists of
- a text encoder that maps input sentence to a set of embedding representations
- an object decoder that predicts the next generated object conditioned on the current scene state
- an attribute decoder that determines the attribute of the next object

![network_arch](/assets/images/2019/05/text_to_scene/network_arch.png)

### Text encoder

To compute for each word $i$ given input text

$$
h_{i}^{E}=\operatorname{BiGRU}\left(x_{i}, h_{i-1}^{E}, h_{i+1}^{E}\right)
$$

here BIGRU is a bidirectional GRU cell, $ x_{i} $ is a word embedding and $h_{i}^{E}$ is a vector encoding the current word and its context.

### Decoders

To use a convolutional network $\Omega$ to encode current canvas ${B}\_{t}$ into a $\mathcal{C} \times H \times W$ feature map representing the current scene state, and to model the history of the scene state $ \left\\{ h_{t}^{D} \right\\} $ by a convolutional GRU. $ h_{t}^{D}$ is an important representation of both temporal and spatial dynamical information.

$$
h_{t}^{D}=\operatorname{ConvGRU}\left(\Omega\left(B_{t}\right), h_{t-1}^{D}\right)
$$

In case that $h_{t}^{D}$ fails to capture small objects, previous step $ {o}_{t-1} $ is provided as input to the downstream decoders.

### Attention-based object decoder

The object decoder based on attention mechanism outputs the likelihhod score on all objects in object vocabulary library $\mathcal{V}$, and takes as input the 

$$
u_{t}^{o}=\text { AvgPooling }\left(\Psi^{o}\left(h_{t}^{D}\right)\right)
$$

$$
c_{t}^{o}=\Phi^{o}\left(\left[u_{t}^{o} ; o_{t-1}\right],\left\{\left(h_{i}^{E}, x_{i}\right)\right\}\right)
$$

$$
p\left(o_{t}\right) \propto \Theta^{o}\left(\left[u_{t}^{o} ; o_{t-1} ; c_{t}^{o}\right]\right)
$$


## Conclusion

Text2Scene model demonstrates the capacity on both abstract and real images, which opens the possibility for future work on transfer learning across domains.

