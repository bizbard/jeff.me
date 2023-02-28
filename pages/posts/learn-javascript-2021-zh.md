---
title: Modern Javascript - 2021
description: Modern Javascript - 2021
date: 2021-10-01T00:00:00.000+00:00
lang: zh
type: project
recording: true
duration: 1month
---

> [**Vue Amsterdam 2023**](https://vuejs.amsterdam/)
> 
> Slides: [PDF](https://antfu.me/talks/2023-02-09)
>

---
Three-dimensional reconstruction is a multimedia technology widely used in computer-aided modeling and 3D
animation. Nevertheless, it is still hard for reconstruction methods to overcome the 3D geometry missing and the object occlusion in
the single-view images. In this paper, we propose a novel method (CPG3D) for reconstructing high-quality 3D shapes from a single
image under the guidance of prior knowledge. Using the single view image as the query, prior knowledge is collected from public
3D datasets, which can compensate for missing 3D geometries and assist the 3D reconstruction network to high fidelity results.
Our method consists of three parts: 1) Cross-modal 3D shape retrieval module: This part retrieves related 3D shapes based on
2D images. Here, we apply the pre-trained model to guarantee the correlation between the retrieved 3D shape and the input
image. 2) Multimodal information fusion module: We propose a multimodal attention mechanism to handle the information fusing of 2D visual and 3D structural information; 3) Three dimensional reconstruction module: We propose a novel encoder decoder network for 3D shape reconstruction. Specifically, we
employ the skip connection operation to link the target image’s visual information with the 3D model’s structural information to
enhance the prediction of 3D details. During training, we employ two carefully designed loss functions to lead the multimodal learning to obtain proper modal features. On the ShapeNet and Pix3D datasets, the final experimental results reveal that our method notably increases reconstruction quality and outperforms SOTA methods.
