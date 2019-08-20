---
layout: post
title: Matrix Completion with Variational Graph Autoencoders for Air Quality Inference 
categories: Graph
description: Graph-based matrix completion in Air Quality Inference.
keywords: Variational graph autoencoder, graph-based matrix completion, deep learning.
---

# Matrix Completion with Variational Graph Autoencoders: Application in Hyperlocal Air Quality Inference

Inferring air quality from a limited number of observations is an essential task for monitoring and controlling air pollution.



|                            Title                             |                            Author                            |             Institution             |   Journal   |      Date      |                         Index Terms                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :---------------------------------: | :---------: | :------------: | :----------------------------------------------------------: |
| [Matrix Completion with Variational Graph Autoencoders: Application in Hyperlocal Air Quality Inference](https://ieeexplore.ieee.org/abstract/document/8683787) | Tien Huu Do, Duc Minh Nguyen, Evaggelia Tsiligianni, Nikos Deligiannis | Vrije Universiteit Brussel, Belgium | ICASSP 2019 | 12-17 May 2019 | Variational graph autoencoder, graph-based matrix completion, deep learning. |

## Abstract

|                          |      Existing inference methods       |                This paper                |
| :----------------------: | :-----------------------------------: | :--------------------------------------: |
|      Data accuracy       |      low spatial resolution data      |        **<u>street-level inference</u>**        |
|        Data types        |             spatial data              |         **<u>spatio-temporal data</u>**         |
|  Data collection method  |       fixed monitoring stations       |             mobile stations              |
|        Algorithm         |             interpolation             |      graph-based matrix completion       |
|        Algorithm         | machine-learning, Deep-neural-network |      variational graph autoencoders      |
|        Algorithm         | graph-based semi-supervised approach  | **<u>end-to-end</u>** graph convolutional model |
| additional types of data |     meteorological & traffic data     |                 No need                  |

## Introduction 

The main contributions in this paper are:

1. They formulate air quality inference as a **<u>graph-based matrix completion</u>** problem and propose a **<u>variational graph autoencoder</u>** for accurate inference.
2. The proposed model effectively incorporates the **<u>temporal and spatial correlations</u>** via a **<u>temporal smoothness constraint</u>** and **<u>graph convolutional operations</u>**.
3. **<u>Real-world datasets</u>**.

## Method 

### Problem Formulation and Notation

Each vehicle makes measurements while moving on the city street network, resulting in **<u>high spatial measurement density</u>**; in contrast, the measurements at a specific location have **<u>low temporal resolution</u>**.

Our task is to predict the air pollution concentration values in the unknown entries using the **<u>measurements</u>** (known entries) and the **<u>street-network topology</u>**.

###　Variational Autoencoders

VAEs build on the assumption that the data points in a dataset can be drawn from a **<u>distribution conditioned by latent variables</u>**; furthermore, the latent variables follow a **<u>prior distribution</u>**, e.g., the Gaussian distribution. VAEs attempt to learn a deterministic function that transforms the Gaussian distribution to the **<u>distribution of the observed data</u>**.

### Variational Graph Autoencoders

Variational graph autoencoders $(VGAEs)$ adhere to the VAE concept and utilize graph convolutional layers ($GCN$) for the parameterized functions $f_µ$, $f_σ$ and $f_z$ Given a graph $\mathcal{G}=(\mathcal{V},\varepsilon)$ with an **<u>adjacency matrix</u>** $A ∈ ℝ^{N×N}$ and a **<u>degree matrix</u>** $D ∈ ℝ^{N×N}$, $N=|V|$, a **<u>graph convolutional layer</u>** is expressed as
$$
\begin{equation}
\Large
f_{\mathrm{GCN}}(\mathbf{X})=\sigma\left(\tilde{\mathbf{D}}^{-\frac{1}{2}} \tilde{\mathbf{A}} \tilde{\mathbf{D}}^{-\frac{1}{2}} \mathbf{x} \mathbf{w}\right)
\end{equation}
$$

### The Proposed AVGAE Architecture

| ![img](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/8671773/8682151/8683787/do1-p5-do-large.gif) |
| :----------------------------------------------------------: |
| ![1566291853166](C:\Users\XiaolinHu\AppData\Roaming\Typora\typora-user-images\1566291853166.png) |

Two nodes are connected if the geodesic distance between them is smaller than a **<u>predefined threshold</u> *δ* **, or if they belong to the same **<u>road segment</u>**. The weight of a connection is the **<u>inverse of the geodesic distance</u>** in meters computed by the Haversine formula.
$$
\begin{equation}
\Large
\mu=\mathrm{G} \mathrm{CN}_{\mu}\left(\mathbf{X}, \mathbf{S}, \Theta_{1}\right)
\\\Large\sigma=\mathrm{GCN}_{\sigma}\left(\mathbf{X}, \mathbf{S}, \Theta_{2}\right)
\\\Large\mathbf{Z} \sim \mathcal{N}(\mu, \sigma)   
\\\Large\tilde{\mathbf{X}} =\mathrm{GCN}_{z}(\mathbf{Z}, \Phi)

\end{equation}
$$

$GCN_µ$, $GCN_σ$ and $GCN_z$ are functions obtained by stacking $GCN$ layers and $Θ_1$, $Θ_2$ and $Φ$ are parameters that can be learned from the data. Furthermore, we summarize the **<u>locations’ geocoordinates</u>** in a matrix ***S***, By doing so, we aim at exploiting the **<u>correlation between the measurements and the geocoordinates</u>**.

The loss function of our model is defined in (3), The temporal dependency between measurements imposes an additional **<u>smoothness constraint</u>**:
$$
\begin{array}{c}\Large{\mathcal{L}\left(\mathbf{X}, \Theta_{1}, \Theta_{2}, \Phi\right)=\frac{1}{|\Omega|} \sum_{(i, j) \in \Omega}\left|\tilde{\mathbf{X}}_{i j}-\mathbf{X}_{i j}\right|+} \\ \Large{\beta \mathcal{D}[q(z | x) \| p(z)]+\gamma \sum_{(i, j)} \sum_{k \in \mathcal{T}(i, j)} e^{-|j-k|}\left(\tilde{\mathbf{X}}_{i j}-\tilde{\mathbf{X}}_{i, j}\right)^{2}}\end{array}
$$
In (3), $\beta $ and $\gamma$ are positive tuning parameters and $\mathcal{T}(i, j)$ is the **<u>neighborhood</u>** of the entry $\mathbf{X}_{i j}$ with respect to the **<u>temporal dimension</u>**. 

## Experiments

|         **<u>Table 1.</u>** Air quality inference results.          |
| :----------------------------------------------------------: |
| ![img](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/8671773/8682151/8683787/do.t1-p5-do-large.gif) |

| **<u>Table 2.</u>** The description of the $NO_2$ and $PM_{2.5}$ dataset. The units for $NO_2$ and $PM_{2.5}$ are parts per billion $(ppb)$ and $\mu G/m^3$. |
| :----------------------------------------------------------: |
| ![img](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/8671773/8682151/8683787/do.t2-p5-do-large.gif) |
