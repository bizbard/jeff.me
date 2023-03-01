---
title: Deep Conditional Clustering for Point Cloud Registration - ICCV 2023
description: Deep Conditional Clustering for Point Cloud Registration - ICCV 2023
date: 2023-03-08T00:00:00.000+00:00
lang: en
type: research
duration: 4months
---

[[toc]]

> [**ICCV 2023**](https://iccv2023.thecvf.com/)
> 
> Authors: [Meifeng Guo](guofeng.mei@student.uts.edu.au)、[Jeff](chuanqi_097@tju.edu.cn)、[Weijie Wang](weijie.wang@unitn.it)
>
> Slides: [PDF](/images/clureg.pdf)
>

---

## Abstract

This paper proposes a **clustering-based** point cloud registration approach under the constraint of overlap scores learning by a network. Specifically, our CluReg first introduces a geometric transformer-based network to extract pointwise features with their associated overlap scores. Then, the clustering is implemented in the coordinate space under the constraints of overlap scores to generate cluster centers with associated cluster probabilities, which can be translated into solving a weighted Wasserstein K-Means problem. After that, the probabilities are used to calculate feature centers in feature space. Finally, the transformation are estimated using both coordinate and feature centers.

##  Introduction

Point cloud registration aims to seek a relative spatial transformation that aligns two partially observed point clouds, which is a crucial aspect in various applications, including but not limited to 3D printing, robotics, and autonomous driving. In recent registration advancements, deep learning based techniques have outperformed conventional methods both in accuracy and efficiency.

### Related Work

Existing deep learning-based point cloud registration methods can be categorized as **correspondence-free** or **correspondence-based**. The former aims to minimize the difference between global features extracted from two input point clouds. These global features are typically computed based on all the points of a point cloud, making correspondence-free approaches inadequate to handle real scenes with partial overlap. The latter typically involves performing keypoint detection and then
computing local descriptors. These descriptors are then matched to determine a set of possible point-level or distribution-level correspondences, which are subsequently
used to estimate the sought transformation. However, point-level registration may not be effective in situations where there are variations in the density of points or
the presence of repetitive patterns. This issue is especially prominent in indoor environments, where low-texture regions or repetitive patterns sometimes dominate the field of view.

Keypoint-free methods attempt to mitigate these limitations by first downsampling the input point clouds into superpoints and then matching them based on their respective local neighborhoods(patches) to determine any overlaps.Lastly, overlapped patches are used to establish dense point correspondences by propagating the matched patches to the individual points. The precision of dense point correspondences is largely influenced by the accuracy of superpoint matches.

### Motivation

- Aligning point clouds with low overlap remains difficult because the overlapping regions need to be explicitly identified in current methods. 
- Most available approaches lack flexibility and are incapable of handling point clouds with an only partial overlap in real-world scenarios. 
- Many correspondence-based methods often necessitate the detection of keypoints, which can be challenging to detect repeatedly across two point clouds, particularly if they have a small area of overlap. 
- The vanilla transformer neglects the geometric information of the point clouds, resulting in inaccurate forecasts.


### Contributions

> The main contributions of our work are as follows:
> 1. We propose a clustering-based point cloud registration approach under the constraint of overlap scores learning by a network.
> 2. We provides a geometric cross-attention which learns transformation-invariant geometric representation and highlights the overlap regions of both point clouds.
> 3. We introduces a conditional clustering method in the geometric space to generate candidate matching centroids in the overlap regions, which can be solved by translating it into an optimal transport problem.

## Methods

We design a cross-attention transformer that injects transformation-invariant geometric structure into learned features to highlight the overlap regions of both point clouds. Our registration approach follows a coarse-to-fine strategy to predict the correspondences. Unlike prior techniques, we employ a clustering algorithm in the fine phase to group the points in each patch while adhering to the overlap scores constraint. This conditional clustering approach can handle various densities and partial overlaps.