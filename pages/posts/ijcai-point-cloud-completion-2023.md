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
> Authors: [Weizhi Nie](WeizhiNie@tju.edu.cn)、[Jeff](chuanqi_097@tju.edu.cn)、[Ruidong Chen](RuidongChen@tju.edu.cn)、[Weijie Wang](weijie.wang@unitn.it)、[Meifeng Guo](guofeng.mei@student.uts.edu.au)、[Bruno Lepri](lepri@fbk.eu)、[Nicu Sebe](niculae.sebe@unitn.it)
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

### Contributions

> The main contributions of our work are as follows:
> 1. We propose a novel memory-based 3D point cloud completion network, Point-PC, to supplement geometric information explicitly from prior knowledge.
> 2. We introduce causal inference to further refine the shape prior, so as to eliminate the distraction of irrelevant information.
> 3. We apply qualitative and quantitative experiments on ShapeNet-55, PCN, and KITTI datasets, which shows that Point-PC improves the accuracy and plausibility of point cloud completion.

## Methods

To deal with this problem, we propose a new **memory based framework** for completing point clouds (Point-PC). This framework uses a memory network to get shape priors and an effective **causal inference** model to choose missing shape information as additional geometric information to help complete point clouds. First, we construct an operating strategy to store, write, and read the memory. Specifically, we store the memory in a “key-value” pair. The key can be updated according to the similarity between the value and the corresponding ground truth. In order to achieve the best prior knowledge information, we construct a causal graph to remove the unrelated shape information of prior shapes. The obtained causal graph model removes the partial shape information and it only saves the missing structural information to help the final decoder obtain a more complete point cloud. Experimental results on the ShapeNet-55, PCN, and KITTI datasets demonstrate that Point-PC performs favorably against the state-of-the-art methods.

<figure>
  <img src="/images/ijcai-architecture.png" alt="The Overall Architecture of Point-PC" />
  <figcaption>The Overall Architecture of Point-PC</figcaption>
</figure>

The overall architecture of Point-PC consists of four main modules:
- pre-trained partial shape encoder
- memory network
- prior knowledge selection module
- shape decoder

The pre-trained encoder extracts feature from the partial input, which is then fed into memory. The memory network retrieves shape priors with sufficient geometric information. Moreover, the prior knowledge selection module selects useful information from the prior shapes. The shape decoder takes the concatenation of the partial shape feature and the shape prior feature to generate the complete point cloud.

## Experiments Results

<!-- | Methods | Table | Chair | Airplane | Car | Sofa | CD-S | CD-M | CD-H | AVG-CD | F1 |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| FoldingNet | 2.53 | 2.81 | 1.43 | 1.98 | 2.48 | 2.67 | 2.66(-0.01) | 4.05(+1.38) | 3.12 | 0.082 |
| PCN | 2.13 | 2.29 | 1.02 | 1.85 | 2.06 | 1.94 | 1.96(+0.02) | 4.08(+2.14) | 2.66 | 0.133 |
| TopNet | 2.21 | 2.53 | 1.14 | 2.18 | 2.36 | 2.26 | 2.16(-0.10) | 4.30(+2.26) | 2.91 | 0.126 |
| PFNet | 3.95 | 4.24 | 1.81 | 2.53 | 3.34 | 3.83 | 3.87(+0.04) | 7.97(+4.10) | 5.22 | 0.339 |
| GRNet | 1.63 | 1.88 | 1.02 | 1.64 | 1.72 | 1.35 | 1.71(+0.36) | 2.85(+1.50) | 1.97 | 0.238 |
| PoinTr | 0.81 | 0.95 | 0.44 | 0.91 | 0.79 | 0.58 | 0.88(+0.30) | 1.79(+1.21) | 1.09 | 0.464 |
| Point-PC | 1.16 | 1.26 | 0.58 | 1.05 | 1.19 | 1.16 | 1.23(+0.07) | 2.04(+0.88) | 1.48 | 0.426 | -->

<figure>
  <img src="/images/ijcai-shapenet55-table.png" alt="Quantitative Results of Our Methods and Several Baselines on ShapeNet-55" />
  <figcaption>Quantitative Results of Our Methods and Several Baselines on ShapeNet-55</figcaption>
</figure>

###  Quantitative Results

Following the evaluation setting in PoinTr, 8 fixed viewpoints are selected, and the number of points in the partial point cloud is set to 2,048, 4,096, and 6,144 (25%, 50% and 75% of the whole point cloud), which divides the testing stage into three difficulty degrees of simple, moderate, and hard (denoted as CD-S, CD-M, and CD-H). As shown in the table, Point-PC achieves an average CD-ℓ2(multiplied by 1000) of 1.48 and F-Score@1% of 0.426 on ShapeNet 55, which shows the effectiveness of Point-PC encountering diverse categories of objects. It is worth noting that the increments of CD-ℓ2 under CD-M(+0.07) and CD-H(+0.88) strategy demonstrate that Point-PC better deals with diverse incompleteness levels and diverse incomplete patterns compared to the state-of-the-art methods. Furthermore, we report the results for categories sufficient(first 5 columns) and insufficient(following 5 columns) training samples. Point-PC performs evenly despite the training sample imbalance. Quantitative results on ShapeNet-55 show that Point-PC can generate complete point clouds in a variety of situations.

<figure>
  <img src="/images/ijcai-shapenet55-figure.png" alt="Qualitative Results on ShapeNet-55 Benchmark" />
  <figcaption>Qualitative Results on ShapeNet-55 Benchmark</figcaption>
</figure>

###  Qualitative Results

The qualitative comparison results are shown in the figure. The proposed Point-PC performs better with fine details than the other methods. For example, in the bottle category, Point-PC predicts a more smooth and more regular structure of bottle edges compared with the other methods. Moreover, Point-PC retains the original details of the partial shapes. In the fifth column of the visulization, Point-PC not only generates the incomplete lamp bracket with a clear structure but also keeps the texture of the lamp shade, which makes it a more plausible completion than the other methods. Consequently, Point-PC effectively learns the geometric information based on the existing partial shape, retrieves similar shape priors based on the learned information and reconstructs complete shapes with more regular arrangements and surface smoothness.

##  Conclusion

In this paper, we propose a novel approach to point cloud completion called Point-PC, which proposes a new memory-based architecture to search prior shapes and designs an effective causal inference model to choose missing shape information as supplemental geometric information to aid point cloud completion. Specifically, the update mechanism of the memory network can optimize the retrieval distance based on the training data, thereby improving the accuracy of the prior shape. To our best knowledge, this is the first work to introduce a causal graph into the point cloud completion task, which effectively filters shape information from previous shapes and preserves missing shape information to improve the integrity and ultimate performance of the fused representation. Comprehensive experiments show the effectiveness and superiority of Point-PC compared to state-of-the-art competitors.