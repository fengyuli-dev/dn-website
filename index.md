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

### Background

The InfoNCE loss considers negative samples from the data distribution per positive sample and has been shown to be very effective for cross-modal representation learning. In contrast, during test-time, the similarity is often measured using the dot product between the image and the text representations, which does not utilize any information with regard to the data distribution.

### Key Insight

Our paper aims to rectify such misalignment, and we show that this boosts performance consistently across a variety of downstream tasks. However, naively applying InfoNCE loss for downstream tasks requires iterating over all negative samples for every test sample, which is not a tractable operation. Instead, we found that a first-order approximation of the InfoNCE loss, consisting of simply subtracting the distribution mean in the representation space before taking the dot product, is able to achieve a similar effect. We call this proposed approach Distribution Normalization (DN) &mdash; DN is very easy to implement and does not require any retraining, fine-tuning or labeled data.

### Results

#### Cross-Modal Retrieval

We first present results on cross-modal retrieval performance on MSCOCO and Flickr30K in the zero-shot setting for more CLIP variants. As can be seen from the bolded entries, adding DN improves retrieval accuracy across the board for CLIP, ALBEF, and TCL. Means for DN\* are estimated using 100 random unlabeled validation samples. Average recalls are calculated with 5 random seeds.

#### Image Captioning Metrics

<table>
    <tr>
        <td colspan="2"></td>
        <td>Flickr8k-expert</td>
        <td>Flickr8k-cf</td>
        <td>THumb</td>
    </tr>
    <tr>
        <td colspan="2"></td>
        <td>$\tau_c$</td>
        <td>$\tau_b$</td>
        <td>$\tau_c$</td>
    </tr>
    <tr>
        <td rowspan="12">Ref-free</td>
        <td><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td>51.4</td>
        <td>34.3</td>
        <td>19.9</td>
    </tr>
    <tr>
        <td>CLIP + TTA</td>
        <td>51.9</td>
        <td>34.7</td>
        <td>20.7</td>
    </tr>
    <tr>
        <td>CLIP + TTA + DN</td>
        <td>53.6</td>
        <td>\textbf{35.7}</td>
        <td>23.7</td>
    </tr>
    <tr>
        <td>CLIP + TTA + DN*</td>
        <td>53.5</td>
        <td>35.5</td>
        <td>22.7</td>
    </tr>
    <tr>
        <td>CLIP + DN</td>
        <td>\textbf{54.3}</td>
        <td>35.4</td>
        <td>\textbf{23.5}</td>
    </tr>
    <tr>
        <td>CLIP + DN*</td>
        <td>53.2</td>
        <td>35.1</td>
        <td>22.2</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2202.10401" rel="noreferrer nofollow" target="_blank">TCL</a></td>
        <td>31.0</td>
        <td>20.6</td>
        <td>8.1</td>
    </tr>
    <tr>
        <td>TCL + DN</td>
        <td>\textbf{42.0}</td>
        <td>\textbf{26.4}</td>
        <td>\textbf{14.4}</td>
    </tr>
    <tr>
        <td>TCL + DN*</td>
        <td>36.0</td>
        <td>23.3</td>
        <td>11.1</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2107.07651" rel="noreferrer nofollow" target="_blank">ALBEF</a></td>
        <td>24.9</td>
        <td>15.4</td>
        <td>0.9</td>
    </tr>
    <tr>
        <td>ALBEF + DN</td>
        <td>\textbf{34.8}</td>
        <td>\textbf{21.8}</td>
        <td>\textbf{5.5}</td>
    </tr>
    <tr>
        <td>ALBEF + DN*</td>
        <td>29.2</td>
        <td>18.1</td>
        <td>2.5</td>
    </tr>
    <tr>
        <td rowspan="9">Ref-based</td>
        <td>BLEU-1</td>
        <td>32.3</td>
        <td>17.9</td>
        <td>11.1</td>
    </tr>
    <tr>
        <td>BLEU-4</td>
        <td>30.8</td>
        <td>16.9</td>
        <td>6.9</td>
    </tr>
    <tr>
        <td><a href="https://arxiv.org/abs/1411.5726" rel="noreferrer nofollow" target="_blank">CIDEr</a></td>
        <td>43.9</td>
        <td>24.6</td>
        <td>13.8</td>
    </tr>
    <tr>
        <td><a href="https://arxiv.org/abs/1908.02265" rel="noreferrer nofollow" target="_blank">ViLBERTScore-F</a></td>
        <td>50.1</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td>53.0</td>
        <td>36.4</td>
        <td>24.7</td>
    </tr>
    <tr>
        <td>CLIP + DN-ref</td>
        <td>\textbf{55.3}</td>
        <td>\textbf{37.0}</td>
        <td>25.8</td>
    </tr>
    <tr>
        <td>CLIP + DN*-ref</td>
        <td>54.3</td>
        <td>36.9</td>
        <td>\textbf{26.2}</td>
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

@misc{zhou2023distribution,
title={Distribution Normalization: An "Effortless" Test-Time Augmentation for Contrastively Learned Visual-language Models},
author={Yifei Zhou and Juntao Ren and Fengyu Li and Ramin Zabih and Ser-Nam Lim},
year={2023},
eprint={2302.11084},
archivePrefix={arXiv},
primaryClass={cs.LG}
}

```
