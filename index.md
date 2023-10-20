---
title: Test-Time Distribution Normalization For Contrastively Learned Vision-language Models
layout: default
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js" type="text/javascript"></script>

<style>
    td {
        text-align: center;
        font-size: 11pt;
    }

    a:hover {
        text-decoration: underline;
    }
</style>

### TL;DR

We present **Distribution Normalization**, a test-time plug-in module that enhances CLIP's performance on a wide range of zero-shot tasks with less than 5 lines of additional code.

<p align="center">
    <img src="images/dn.gif" alt="Image" />
    <figcaption>An illustration of how Distribution Normalization better aligns with infoNCE pre-training objective. $\mu_{img}$ and $\mu_{txt}$ are mean encoded representations for images and text respectively.</figcaption>
</p>

### Abstract

Advances in the field of vision-language contrastive learning have made it possible for many downstream applications to be carried out efficiently and accurately by simply taking the dot product between image and text representations.

One of the most representative approaches proposed recently known as [CLIP](https://arxiv.org/abs/2103.00020) has garnered widespread adoption due to its effectiveness. CLIP is trained with an InfoNCE loss that takes into account both positive and negative samples to help learn a much more robust representation space. This paper reveals that the common downstream practice of taking a dot product is only a zeroth-order approximation of the optimization goal, resulting in a loss of information during test-time. Intuitively, since the model has been optimized based on the InfoNCE loss, test-time procedures should also be in alignment. The question lies in how one can retrieve any semblance of negative samples information during inference in a computationally efficient way.

To this end, we propose Distribution Normalization (DN), where we approximate the mean representation of a batch of test samples and use such a mean to represent what would be analogous to negative samples in the InfoNCE loss. DN requires no retraining or fine-tuning and can be effortlessly applied during inference. Extensive experiments on a wide variety of downstream tasks exhibit a clear advantage of DN over the dot product on top of other existing test-time augmentation methods.

### Motivation

The InfoNCE loss considers negative samples from the data distribution per positive sample and has been shown to be very effective for cross-modal representation learning. In contrast, during test-time, the similarity is often measured using the dot product between the image and the text representations, which does not utilize any information with regard to the data distribution.

### Methodology

Our paper aims to rectify such misalignment, and we show that this boosts performance consistently across a variety of downstream tasks. However, naively applying InfoNCE loss for downstream tasks requires iterating over all negative samples for every test sample, which is not a tractable operation. Instead, we found that a first-order approximation of the InfoNCE loss, consisting of simply subtracting the distribution mean in the representation space before taking the dot product, is able to achieve a similar effect. We call this proposed approach Distribution Normalization (DN) &mdash; DN is very easy to implement and does not require any retraining, fine-tuning or labeled data.

While a more thorough proof is provided in our paper, we will briefly touch on the core idea here. In our analysis, we let $\mathcal{D}_S$ represent the training distribution and $\mathcal{D}_T$ represent the test distribution, $x_0, y_0$ represent an image and text pair, $\phi$ and $\psi$ represent image and text encoders respectively, and $\tau$ represent a temperature hyperparameter used in InfoNCE loss while training. Then, we observe a zero-order approximation of the InfoNCE loss can be expressed as

$$
    \mathcal{L}_{NCE}^{(0)}(\mathcal{D}_T) = 2n \cdot \mathbb{E}_{x_0, y_0 \sim \mathcal{D}_T} \left[e^{-\phi(x_0)^\intercal \psi(y_0)/\tau} \right],
$$

such that the downstream similarity score is computed as

$$
S_{(0)}(x_0, y_0) = \phi(x_0)^\top \psi(y_0).
$$

However, if we can estimate the mean of the unlabeled distribution $\mu_x$ and $\mu_y$ for $\phi(x_1)$ and $\psi(y_1)$, we can perform a first-order approximation of the distribution with $\widehat{P}(\phi(x_1)) = \mathbb{I}\{\phi(x_1) = \mu_x\}$ and $\widehat{P}(\psi(y_1)) = \mathbb{I}\{\psi(y_1) = \mu_y\}$, so that $\widehat{P}(\phi(x_1)), \widehat{P}(\psi(y_1))$ matches the true distribution in terms of the first-order moment. We are thus left with

$$
    \mathcal{L}_{NCE}^{(1)}(\mathcal{D}_T) = 2n \cdot \mathbb{E}_{x_0, y_0 \sim \mathcal{D}_T} \left[e^{\phi(x_0)^\intercal (\mu_y - \psi(y_0))/\tau} + e^{(\mu_x - \phi(x_0))^\intercal \psi(y_0)/\tau} \right].
$$

We show that this can be nicely represented as geometric mean, such that using monotonicity of exponentiation, we get a more accurate similarity score

$$
S_{(1)}(x_0, y_0) = (\phi(x_0) - \frac{1}{2}\mu_x)^\intercal (\psi(y_0) - \frac{1}{2}\mu_y).
$$

This translates directly to out implementation of DN, in which subtract off the mean of a test-time batch of image and text embeddings from each image and text embedding of the test set.

### Results

We present a representative subsection of our results. For the full list, please see our paper!

#### Cross-Modal Retrieval

We first present results on cross-modal retrieval performance on MSCOCO in the zero-shot setting for more CLIP variants. As can be seen from the bolded entries, adding DN improves retrieval accuracy across the board for CLIP and CLIP+TTA. Means for DN\* are estimated using 100 random unlabeled validation samples. Average recalls are calculated with 5 random seeds.

<table>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="6" style="text-align:center;">MSCOCO (5K test set)</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="3" style="text-align:center;">Image $\rightarrow$ Text</td>
        <td colspan="3" style="text-align:center;">Text $\rightarrow$ Image</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td style="text-align:center;">R@1</td>
        <td style="text-align:center;">R@5</td>
        <td style="text-align:center;">R@10</td>
        <td style="text-align:center;">R@1</td>
        <td style="text-align:center;">R@5</td>
        <td style="text-align:center;">R@10</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2103.00020" rel="noreferrer nofollow" target="_blank">CLIP</a></td>
        <td style="text-align:center;">$52.4$</td>
        <td style="text-align:center;">$76.0$</td>
        <td style="text-align:center;">$84.5$</td>
        <td style="text-align:center;">$30.2$</td>
        <td style="text-align:center;">$55.1$</td>
        <td style="text-align:center;">$66.4$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">$52.9$</td>
        <td style="text-align:center;">$76.4$</td>
        <td style="text-align:center;">$84.9$</td>
        <td style="text-align:center;">$32.1$</td>
        <td style="text-align:center;">$57.4$</td>
        <td style="text-align:center;">$68.3$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + <a href="https://arxiv.org/abs/2303.16730" rel="noreferrer nofollow" target="_blank">TTA</a> + DN*</td>
        <td style="text-align:center;">$\textbf{54.7}$</td>
        <td style="text-align:center;">$\textbf{77.8}$</td>
        <td style="text-align:center;">$\textbf{85.6}$</td>
        <td style="text-align:center;">$\textbf{33.8}$</td>
        <td style="text-align:center;">$\textbf{59.4}$</td>
        <td style="text-align:center;">$\textbf{70.1}$</td>
    </tr>

</table>

#### Zero-shot Classification

Next, we present zero-shot classification performance on ImageNet1K, Cifar100, and SUN397. Means for DN are estimated using 100 random unlabeled validation samples. Average accuracies and standard deviations are calculated with 5 random seeds.

<table>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="2" style="text-align:center;">ImageNet1K</td>
        <td colspan="2" style="text-align:center;">Cifar100</td>
        <td colspan="2" style="text-align:center;">SUN397</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2103.00020" rel="noreferrer nofollow" target="_blank">CLIP</a></td>
        <td style="text-align:center;">$61.0$</td>
        <td style="text-align:center;">$87.4$</td>
        <td style="text-align:center;">$63.9$</td>
        <td style="text-align:center;">$88.7$</td>
        <td style="text-align:center;">$56.1$</td>
        <td style="text-align:center;">$89.4$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">$61.7$</td>
        <td style="text-align:center;">$87.8$</td>
        <td style="text-align:center;">$65.1$</td>
        <td style="text-align:center;">$89.4$</td>
        <td style="text-align:center;">$57.3$</td>
        <td style="text-align:center;">$90.2$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.14169" rel="noreferrer nofollow" target="_blank">CALIP</a></td>
        <td style="text-align:center;">$61.2$</td>
        <td style="text-align:center;">$87.5$</td>
        <td style="text-align:center;">$64.2$</td>
        <td style="text-align:center;">$88.9$</td>
        <td style="text-align:center;">$56.1$</td>
        <td style="text-align:center;">$89.3$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.07511" rel="noreferrer nofollow" target="_blank">TPT</a> (Inefficient)</td>
        <td style="text-align:center;">$\textbf{63.5}$</td>
        <td style="text-align:center;">$87.1$</td>
        <td style="text-align:center;">$65.2$</td>
        <td style="text-align:center;">$88.1$</td>
        <td style="text-align:center;">$\textbf{59.4}$</td>
        <td style="text-align:center;">$88.8$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA</td>
        <td style="text-align:center;">$62.4$</td>
        <td style="text-align:center;">$88.5$</td>
        <td style="text-align:center;">$66.0$</td>
        <td style="text-align:center;">$90.5$</td>
        <td style="text-align:center;">$56.9$</td>
        <td style="text-align:center;">$90.0$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN *</td>
        <td style="text-align:center;">$63.2$</td>
        <td style="text-align:center;">$\textbf{88.9}$</td>
        <td style="text-align:center;">$\textbf{67.1}$</td>
        <td style="text-align:center;">$\textbf{90.7}$</td>
        <td style="text-align:center;">$58.1$</td>
        <td style="text-align:center;">$\textbf{90.7}$</td>
    </tr>
</table>

#### Image Captioning Metrics

We see that on image captioning, adding DN improves upon existing baselines on Flicker8k-Expert, Flickr8k-CF, and THumb datasets.

<table>
    <tr>
        <td colspan="2"></td>
        <td style="text-align:center;">Flickr8k-expert</td>
        <td style="text-align:center;">Flickr8k-cf</td>
        <td style="text-align:center;">THumb</td>
    </tr>
    <tr>
        <td colspan="2"></td>
        <td style="text-align:center;">$\tau_c$</td>
        <td style="text-align:center;">$\tau_b$</td>
        <td style="text-align:center;">$\tau_c$</td>
    </tr>
    <tr>
        <td rowspan="6">Ref-free</td>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td style="text-align:center;">51.4</td>
        <td style="text-align:center;">34.3</td>
        <td style="text-align:center;">19.9</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA</td>
        <td style="text-align:center;">51.9</td>
        <td style="text-align:center;">34.7</td>
        <td style="text-align:center;">20.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN</td>
        <td style="text-align:center;">53.6</td>
        <td style="text-align:center;">$\textbf{35.7}$</td>
        <td style="text-align:center;">23.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN*</td>
        <td style="text-align:center;">53.5</td>
        <td style="text-align:center;">35.5</td>
        <td style="text-align:center;">22.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN</td>
        <td style="text-align:center;">$\textbf{54.3}$</td>
        <td style="text-align:center;">35.4</td>
        <td style="text-align:center;">$\textbf{23.5}$</td>
    </tr>
    <tr  style="border-bottom:2px solid black">
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">53.2</td>
        <td style="text-align:center;">35.1</td>
        <td style="text-align:center;">22.2</td>
    </tr>
    <tr>
        <td rowspan="9">Ref-based</td>
        <td style="text-align:center;">BLEU-1</td>
        <td style="text-align:center;">32.3</td>
        <td style="text-align:center;">17.9</td>
        <td style="text-align:center;">11.1</td>
    </tr>
    <tr>
        <td style="text-align:center;">BLEU-4</td>
        <td style="text-align:center;">30.8</td>
        <td style="text-align:center;">16.9</td>
        <td style="text-align:center;">6.9</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/1411.5726" rel="noreferrer nofollow" target="_blank">CIDEr</a></td>
        <td style="text-align:center;">43.9</td>
        <td style="text-align:center;">24.6</td>
        <td style="text-align:center;">13.8</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/1908.02265" rel="noreferrer nofollow" target="_blank">ViLBERTScore-F</a></td>
        <td style="text-align:center;">50.1</td>
        <td style="text-align:center;">-</td>
        <td style="text-align:center;">-</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td style="text-align:center;">53.0</td>
        <td style="text-align:center;">36.4</td>
        <td style="text-align:center;">24.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN-ref</td>
        <td style="text-align:center;">$\textbf{55.3}$</td>
        <td style="text-align:center;">$\textbf{37.0}$</td>
        <td style="text-align:center;">25.8</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*-ref</td>
        <td style="text-align:center;">54.3</td>
        <td style="text-align:center;">36.9</td>
        <td style="text-align:center;">$\textbf{26.2}$</td>
    </tr>
</table>

### Authors

<div>
  <div style="text-align: left;">
  {% for person in site.data.authors %}
  <div class="person">
    <img src="{{ person.image }}" width=140 /><br>
    <a href="{{ person.url | relative_url }}">{{ person.name }}</a><br>
    <span>{{ person.title | replace: '&', '<br>' }}</span>
  </div>
  {% endfor %}
  </div>
</div>
\* indicates equal contribution.

### Citation

Need change after we update paper version on arXiv.

```

@misc{zhou2023testtime,
      title={Test-Time Distribution Normalization for Contrastively Learned Vision-language Models},
      author={Yifei Zhou and Juntao Ren and Fengyu Li and Ramin Zabih and Ser-Nam Lim},
      year={2023},
      eprint={2302.11084},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}

```
