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

Point cloud completion aims to recover raw point clouds captured by scanners from partial observations caused by occlusion and limited view angles. Many approaches utilize a partial-complete paradigm in which missing parts are directly predicted by a global feature learned from partial inputs. This makes it hard to recover details because the global feature is unlikely to capture the full details of all missing parts. In this paper, we propose a novel approach to point cloud completion called Point-PC, which uses a memory network to retrieve shape priors and designs an effective causal inference model to choose missing shape information as supplemental geometric information to aid point cloud completion. Specifically, we propose a memory operating mechanism where the complete shape features and the corresponding shapes are stored in the form of “key-value” pairs. To retrieve similar shapes from the partial input, we also apply a contrastive learning-based pre-training scheme to transfer features of incomplete shapes into the domain of complete shape features. Moreover, we use backdoor adjustment to get rid of the confounder, which is a part of the shape prior that has the same semantic structure as the partial input. Experimental results on the ShapeNet-55, PCN, and KITTI datasets demonstrate that Point-PC performs favorably against the state-of-the-art methods.