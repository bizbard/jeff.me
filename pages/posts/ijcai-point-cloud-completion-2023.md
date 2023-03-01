---
title: Point Cloud Completion Guided by Prior Knowledge via Causal - IJCAI 2023
description: Point Cloud Completion Guided by Prior Knowledge via Causal - IJCAI 2023
date: 2023-01-18T00:00:00.000+00:00
lang: en
type: research
duration: 2months
---

[[toc]]

> [**IJCAI 2023**](https://www.ijcai.org/)
> 
> Authors: [Weizhi Nie](WeizhiNie@tju.edu.cn)、[Chuanqi Jiao](chuanqi_097@tju.edu.cn)、[Ruidong Chen](RuidongChen@tju.edu.cn)、[Weijie Wang](weijie.wang@unitn.it)、[Meifeng Guo](guofeng.mei@student.uts.edu.au)、[Bruno Lepri](lepri@fbk.eu)、[Nicu Sebe](niculae.sebe@unitn.it)
>
> Slides: [PDF](/images/ijcai2023.pdf)
>

---

## Abstract

**Point cloud completion** aims to recover raw point clouds captured by scanners from partial observations caused by occlusion and limited view angles. Many approaches utilize a partial-complete paradigm in which missing parts are directly predicted by a global feature learned from partial inputs. This makes it hard to recover details because the global feature is unlikely to capture the full details of all missing parts. In this paper, we propose a novel approach to point cloud completion called Point-PC, which uses a memory network to retrieve shape priors and designs an effective causal inference model to choose missing shape information as supplemental geometric information to aid point cloud completion. Specifically, we propose a memory operating mechanism where the complete shape features and the corresponding shapes are stored in the form of “key-value” pairs. To retrieve similar shapes from the partial input, we also apply a contrastive learning-based pre-training scheme to transfer features of incomplete shapes into the domain of complete shape features. Moreover, we use backdoor adjustment to get rid of the confounder, which is a part of the shape prior that has the same semantic structure as the partial input. Experimental results on the ShapeNet-55, PCN, and KITTI datasets demonstrate that Point-PC performs favorably against the state-of-the-art method.

##  Introduction

Due to occlusion, view angles, and limitations of sensor resolution, raw point clouds are usually sparse and defective. Consequently, point cloud completion becomes essential.

### Related Work

Most recent methods incorporate geometry-aware modules into a transformer-based structure. PoinTr used a KNN model to facilitate transformers, which can better leverage the inductive bias about 3D geometric structures. CompleteDT integrated useful local information into the generation operations by enhancing the correlation of neighboring points in the proposed dense augment inference transformers. These two methods formulate the point cloud completion task as a set-to-set translation task, where complex dependency is learned among the point groups. Many approaches used the same framework to handle the point cloud completion problem.

### Motivation

However, there are two **drawbacks** to the paradigm:
- An incomplete shape is hard to learn detailed structure information and build a clear relationship between the complete point cloud model;
- A global feature like this is spread out and does not keep much fine-grained information for the up-sample phase. Because of this, geometry-aware models can not learn complex structures if they know less about geometry.

## Methods

To deal with this problem, we propose a new **memory based framework** for completing point clouds (Point-PC). This framework uses a memory network to get shape priors and an effective **causal inference** model to choose missing shape information as additional geometric information to help complete point clouds. First, we construct an operating strategy to store, write, and read the memory. Specifically, we store the memory in a “key-value” pair. The key can be updated according to the similarity between the value and the corresponding ground truth. In order to achieve the best prior knowledge information, we construct a causal graph to remove the unrelated shape information of prior shapes. The obtained causal graph model removes the partial shape information and it only saves the missing structural information to help the final decoder obtain a more complete point cloud. Experimental results on the ShapeNet-55, PCN, and KITTI datasets demonstrate that Point-PC performs favorably against the state-of-the-art methods.

> The main contributions of our work are as follows:
> 1. We propose a novel memory-based 3D point cloud completion network, Point-PC, to supplement geometric information explicitly from prior knowledge
> 2. We introduce causal inference to further refine the shape prior, so as to eliminate the distraction of irrelevant information.
> 3. We apply qualitative and quantitative experiments on ShapeNet-55, PCN, and KITTI datasets, which shows that Point-PC improves the accuracy and plausibility of point cloud completion.




