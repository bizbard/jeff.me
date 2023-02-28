---
title: Deep Conditional Clustering for Point Cloud Registration - ICCV 2023
description: Deep Conditional Clustering for Point Cloud Registration - ICCV 2023
date: 2023-03-08T00:00:00.000+00:00
lang: en
type: research
duration: 4months
---

> [**Vue Amsterdam 2023**](https://vuejs.amsterdam/)
> 
> Slides: [PDF](https://antfu.me/talks/2023-02-09)
>

---

This paper proposes a clustering-based point cloud registration approach under the constraint of overlap scores learning by a network. Specifically, our CluReg first introduces a geometric transformer-based network to extract pointwise features with their associated overlap scores.
Then, the clustering is implemented in the coordinate space under the constraints of overlap scores to generate cluster centers with associated cluster probabilities, which can be translated into solving a weighted Wasserstein K-Means problem. After that, the probabilities are used to calculate
feature centers in feature space. Finally, the transformation are estimated using both coordinate and feature centers.