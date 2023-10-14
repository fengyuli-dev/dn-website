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

<table>
    <tr>
        <td></td>
        <td>\multicolumn{6}{c}{MSCOCO (5K test set)}</td>
        <td>\multicolumn{6}{c}{Flickr30K (1K test set)}</td>
    </tr>
    <tr>
        <td></td>
        <td>\multicolumn{3}{c}{Image $\rightarrow$ Text}</td>
        <td>\multicolumn{3}{c}{Text $\rightarrow$ Image}</td>
        <td>\multicolumn{3}{c}{Image $\rightarrow$ Text}</td>
        <td>\multicolumn{3}{c}{Text $\rightarrow$ Image}</td>
    </tr>
    <tr>
        <td></td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
    </tr>
    <tr>
        <td>CLIP \cite{clip}</td>
        <td>$52.4$</td>
        <td>$76.0$</td>
        <td>$84.5$</td>
        <td>$ 30.2$</td>
        <td>$55.1$</td>
        <td>$66.4$</td>
        <td>$81.3$</td>
        <td>$95.0$</td>
        <td>$98.5$</td>
        <td>$62.7$</td>
        <td>$86.0$</td>
        <td>$92.0$</td>
    </tr>
    <tr>
        <td>CLIP + TTA \cite{shanmugam2021better}</td>
        <td>$53.9$</td>
        <td>$77.5$</td>
        <td>$85.5$</td>
        <td>$32.1$</td>
        <td>$57.5$</td>
        <td>$68.3$</td>
        <td>$83.2$</td>
        <td>$96.8$</td>
        <td>$98.4$</td>
        <td>$65.2$</td>
        <td>$87.9$</td>
        <td>$92.9$</td>
    </tr>
    <tr>
        <td>CLIP + TTA + DN</td>
        <td>$53.6 \pm 0.1$</td>
        <td>$76.9 \pm 0.1$</td>
        <td>$84.8 \pm 0.1$</td>
        <td>$\textbf{34.8} \pm 0.0$</td>
        <td>$\textbf{60.4} \pm 0.0$</td>
        <td>$\textbf{70.8} \pm 0.1$</td>
        <td>$\textbf{85.8} \pm 0.2$</td>
        <td>$\textbf{97.5} \pm 0.1$</td>
        <td>$\textbf{99.1} \pm 0.0$</td>
        <td>$\textbf{68.1} \pm 0.1$</td>
        <td>$\textbf{89.4} \pm 0.1$</td>
        <td>$\textbf{94.1} \pm 0.0$</td>
    </tr>
    <tr>
        <td>CLIP + TTA + DN*</td>
        <td>$\textbf{54.7} \pm 0.1$</td>
        <td>$\textbf{77.8} \pm 0.1$</td>
        <td>$\textbf{85.6} \pm 0.1$</td>
        <td>$33.8 \pm 0.0$</td>
        <td>$59.4 \pm 0.0$</td>
        <td>$70.1 \pm 0.0$</td>
        <td>$\textbf{85.8} \pm 0.1 $</td>
        <td>$\textbf{97.5} \pm 0.1$</td>
        <td>$98.8 \pm 0.1$</td>
        <td>$67.6 \pm 0.0$</td>
        <td>$89.1 \pm 0.0$</td>
        <td>$93.9 \pm 0.1$</td>
    </tr>
    <tr>
        <td>CLIP + DN</td>
        <td>$51.7 \tiny{\pm 0.1}$</td>
        <td>$75.8 \tiny{\pm 0.1}$</td>
        <td>$84.0 \tiny{\pm 0.1}$</td>
        <td>$ 33.4 \tiny{\pm 0.0}$</td>
        <td>$58.6 \tiny{\pm 0.1}$</td>
        <td>$69.4 \tiny{\pm 0.1}$</td>
        <td>$83.3 \tiny{\pm 0.2} $</td>
        <td>$96.4 \tiny{\pm 0.1}$</td>
        <td>$98.6 \tiny{\pm 0.1}$</td>
        <td>$66.2 \tiny{\pm 0.1}$</td>
        <td>$88.2 \tiny{\pm 0.1}$</td>
        <td>$93.3 \tiny{\pm 0.1}$</td>
    </tr>
    <tr>
        <td>CLIP + DN*</td>
        <td>$52.9 \tiny{\pm 0.1}$</td>
        <td>$76.4 \tiny{\pm 0.1}$</td>
        <td>$84.9 \tiny{\pm 0.1}$</td>
        <td>$ 32.1 \tiny{\pm 0.1}$</td>
        <td>$57.4 \tiny{\pm 0.0}$</td>
        <td>${68.3} \tiny{\pm 0.1}$</td>
        <td>${83.5} \tiny{\pm 0.1} $</td>
        <td>$96.2 \tiny{\pm 0.0}$</td>
        <td>$98.5 \tiny{\pm 0.1}$</td>
        <td>$64.8 \tiny{\pm 0.2}$</td>
        <td>$87.5 \tiny{\pm 0.1}$</td>
        <td>$93.1 \tiny{\pm 0.0}$</td>
    </tr>
    <tr>
        <td>TCL \cite{tcl}</td>
        <td>$57.6$</td>
        <td>$84.3$</td>
        <td>$91.8$</td>
        <td>$41.8$</td>
        <td>$70.6$</td>
        <td>$ 80.6$</td>
        <td>$ 73.8$</td>
        <td>$93.3$</td>
        <td>$ 96.9$</td>
        <td>$59.1$</td>
        <td>$84.6$</td>
        <td>$91.1$</td>
    </tr>
    <tr>
        <td>TCL + DN</td>
        <td>$\textbf{60.6} \tiny{\pm 0.1}$</td>
        <td>$\textbf{85.8} \tiny{\pm 0.1}$</td>
        <td>$\textbf{92.4} \tiny{\pm 0.1}$</td>
        <td>$\textbf{43.2} \tiny{\pm 0.0}$</td>
        <td>$\textbf{71.8} \tiny{\pm 0.1}$</td>
        <td>$\textbf{81.6} \tiny{\pm 0.0}$</td>
        <td>$77.5 \tiny{\pm 0.5}$</td>
        <td>$94.1 \tiny{\pm 0.2}$</td>
        <td>$\textbf{96.9} \tiny{\pm 0.2}$</td>
        <td>$59.8 \tiny{\pm 0.2}$</td>
        <td>$84.9 \tiny{\pm 0.1}$</td>
        <td>$91.1 \tiny{\pm 0.1}$</td>
    </tr>
    <tr>
        <td>TCL + DN*</td>
        <td>$59.5 \tiny{\pm 0.1}$</td>
        <td>$85.2 \tiny{\pm 0.0}$</td>
        <td>$92.2 \tiny{\pm 0.1}$</td>
        <td>$42.7 \tiny{\pm 0.0}$</td>
        <td>$71.5 \tiny{\pm 0.0}$</td>
        <td>$81.3 \tiny{\pm 0.0}$</td>
        <td>$75.5 \tiny{\pm 0.0}$</td>
        <td>$\textbf{94.4} \tiny{\pm 0.1}$</td>
        <td>$ 96.9 \tiny{\pm 0.1}$</td>
        <td>$\textbf{60.0} \tiny{\pm 0.1}$</td>
        <td>$\textbf{85.1} \tiny{\pm 0.0}$</td>
        <td>$91.1 \tiny{\pm 0.0}$</td>
    </tr>
    <tr>
        <td>ALBEF \cite{albef}</td>
        <td>$62.5$</td>
        <td>$85.9$</td>
        <td>$ 92.2$</td>
        <td>$40.2 $</td>
        <td>$68.4$</td>
        <td>$78.9$</td>
        <td>$78.2$</td>
        <td>$95.5$</td>
        <td>$97.9$</td>
        <td>$59.9$</td>
        <td>$84.8$</td>
        <td>$90.6$</td>
    </tr>
    <tr>
        <td>ALBEF + DN</td>
        <td>$ \textbf{63.0} \tiny{\pm 0.2}$</td>
        <td>$85.8 \tiny{\pm 0.1}$</td>
        <td>$92.4 \tiny{\pm 0.1}$</td>
        <td>$\textbf{44.8} \tiny{\pm 0.1}$</td>
        <td>$\textbf{72.5} \tiny{\pm 0.0}$</td>
        <td>$\textbf{82.0} \tiny{\pm 0.0}$</td>
        <td>$\textbf{80.6} \tiny{\pm 0.1}$</td>
        <td>$\textbf{96.2} \tiny{\pm 0.1}$</td>
        <td>$\textbf{98.3} \tiny{\pm 0.1}$</td>
        <td>$\textbf{64.1} \tiny{\pm 0.0}$</td>
        <td>$\textbf{87.1} \tiny{\pm 0.1}$</td>
        <td>$\textbf{92.3} \tiny{\pm 0.1}$</td>
    </tr>
    <tr>
        <td>ALBEF +DN*</td>
        <td>$ \textbf{63.0} \tiny{\pm 0.1}$</td>
        <td>$\textbf{86.0} \tiny{\pm 0.0}$</td>
        <td>$\textbf{92.5} \tiny{\pm 0.1}$</td>
        <td>$42.8 \tiny{\pm 0.1}$</td>
        <td>$70.8 \tiny{\pm 0.0}$</td>
        <td>$80.7 \tiny{\pm 0.0}$</td>
        <td>$79.2 \tiny{\pm 0.1}$</td>
        <td>$\textbf{96.2} \tiny{\pm 0.0}$</td>
        <td>$98.0 \tiny{\pm 0.0}$</td>
        <td>$62.4 \tiny{\pm 0.1}$</td>
        <td>$86.1 \tiny{\pm 0.1}$</td>
        <td>$91.9 \tiny{\pm 0.1}$</td>
    </tr>
</table>

#### Zero-shot Classification

Next, we present zero-shot classification performance on ImageNet1K, Cifar100, SUN397, Stanford Cars, Caltech 101, and Flowers 102. Means for DN are estimated using 100 random unlabeled validation samples. Average accuracies and standard deviations are calculated with 5 random seeds.

<table>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="2" style="text-align:center;">{ImageNet1K}</td>
        <td colspan="2" style="text-align:center;">{Cifar100}</td>
        <td colspan="2" style="text-align:center;">{SUN397}</td>
        <td colspan="2" style="text-align:center;">{Stanford Cars}</td>
        <td colspan="2" style="text-align:center;">{Caltech 101}</td>
        <td colspan="2" style="text-align:center;">{Flowers 102}</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
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
        <td style="text-align:center;">${58.6}$</td>
        <td style="text-align:center;">${90.9}$</td>
        <td style="text-align:center;">$82.3$</td>
        <td style="text-align:center;">$95.0$</td>
        <td style="text-align:center;">$62.1$</td>
        <td style="text-align:center;">$83.8$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.14169" rel="noreferrer nofollow" target="_blank">CALIP</a></td>
        <td style="text-align:center;">$61.2\pm 0.2$</td>
        <td style="text-align:center;">$87.5\pm 0.1$</td>
        <td style="text-align:center;">$64.2\pm 0.1$</td>
        <td style="text-align:center;">$88.9 \pm 0.0$</td>
        <td style="text-align:center;">$56.1\pm 0.1$</td>
        <td style="text-align:center;">$89.3 \pm 0.1$</td>
        <td style="text-align:center;">$58.7\pm 0.0$</td>
        <td style="text-align:center;">$90.1 \pm 0.0$</td>
        <td style="text-align:center;">$82.5 \pm 0.1$</td>
        <td style="text-align:center;">$95.1\pm 0.0$</td>
        <td style="text-align:center;">$62.2 \pm 0.1$</td>
        <td style="text-align:center;">$83.4 \pm 0.0$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.07511" rel="noreferrer nofollow" target="_blank">TPT</a></td>
        <td style="text-align:center;">$\textbf{63.5}$</td>
        <td style="text-align:center;">$87.1$</td>
        <td style="text-align:center;">$65.2$</td>
        <td style="text-align:center;">$88.1$</td>
        <td style="text-align:center;">$\textbf{59.4}$</td>
        <td style="text-align:center;">$88.8$</td>
        <td style="text-align:center;">$\textbf{61.5}$</td>
        <td style="text-align:center;">$90.2$</td>
        <td style="text-align:center;">$83.2$</td>
        <td style="text-align:center;">$96.0$</td>
        <td style="text-align:center;">$\textbf{64.5}$</td>
        <td style="text-align:center;">$81.3$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA</td>
        <td style="text-align:center;">$62.4$</td>
        <td style="text-align:center;">$88.5$</td>
        <td style="text-align:center;">$66.0$</td>
        <td style="text-align:center;">$90.5$</td>
        <td style="text-align:center;">$56.9$</td>
        <td style="text-align:center;">$90.0$</td>
        <td style="text-align:center;">$60.8$</td>
        <td style="text-align:center;">$\textbf{92.3}$</td>
        <td style="text-align:center;">$82.5$</td>
        <td style="text-align:center;">$95.4$</td>
        <td style="text-align:center;">$62.5$</td>
        <td style="text-align:center;">$84.0$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN</td>
        <td style="text-align:center;">$63.0 \pm 0.0$</td>
        <td style="text-align:center;">$88.8 \pm 0.0$</td>
        <td style="text-align:center;">$\textbf{67.5} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{90.7} \pm 0.1$</td>
        <td style="text-align:center;">$58.8 \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{91.0} \pm 0.1$</td>
        <td style="text-align:center;">$60.3 \pm 0.2$</td>
        <td style="text-align:center;">$91.5 \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{83.3} \pm 0.1$</td>
        <td style="text-align:center;">$95.4 \pm 0.0$</td>
        <td style="text-align:center;">$63.1 \pm 0.1$</td>
        <td style="text-align:center;">$84.3 \pm 0.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN *</td>
        <td style="text-align:center;">$63.2 \pm 0.0$</td>
        <td style="text-align:center;">$\textbf{88.9} \pm 0.0$</td>
        <td style="text-align:center;">$67.1 \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{90.7} \pm 0.0$</td>
        <td style="text-align:center;">$58.1 \pm 0.1$</td>
        <td style="text-align:center;">$90.7 \pm 0.0$</td>
        <td style="text-align:center;">$\textbf{61.5} \pm 0.1$</td>
        <td style="text-align:center;">$92.2 \pm 0.0$</td>
        <td style="text-align:center;">$83.1 \pm 0.0$</td>
        <td style="text-align:center;">$\textbf{95.5} \pm 0.0$</td>
        <td style="text-align:center;">$63.5 \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{84.5} \pm 0.0$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN</td>
        <td style="text-align:center;">$61.6 \pm 0.1$</td>
        <td style="text-align:center;">$87.7 \pm 0.1$</td>
        <td style="text-align:center;">$65.5 \pm 0.2$</td>
        <td style="text-align:center;">$89.2 \pm 0.2$</td>
        <td style="text-align:center;">$57.9 \pm 0.1$</td>
        <td style="text-align:center;">$90.5 \pm 0.1$</td>
        <td style="text-align:center;">$57.5 \pm 0.1$</td>
        <td style="text-align:center;">$90.5 \pm 0.1$</td>
        <td style="text-align:center;">$82.9 \pm 0.1$</td>
        <td style="text-align:center;">$94.9 \pm 0.1$</td>
        <td style="text-align:center;">$62.7 \pm 0.1$</td>
        <td style="text-align:center;">$84.1 \pm 0.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">$61.7 \pm 0.1$</td>
        <td style="text-align:center;">$87.8 \pm 0.0$</td>
        <td style="text-align:center;">$65.1 \pm 0.1$</td>
        <td style="text-align:center;">$89.4 \pm 0.0$</td>
        <td style="text-align:center;">$57.3 \pm 0.0$</td>
        <td style="text-align:center;">$90.2 \pm 0.1$</td>
        <td style="text-align:center;">$58.6 \pm 0.1$</td>
        <td style="text-align:center;">$90.7 \pm 0.0$</td>
        <td style="text-align:center;">$82.8 \pm 0.0$</td>
        <td style="text-align:center;">$95.1 \pm 0.0$</td>
        <td style="text-align:center;">$62.9 \pm 0.1$</td>
        <td style="text-align:center;">$84.3 \pm 0.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2202.10401" rel="noreferrer nofollow" target="_blank">TCL</a></td>
        <td style="text-align:center;">$20.0 $</td>
        <td style="text-align:center;">$42.9$</td>
        <td style="text-align:center;">$39.1$</td>
        <td style="text-align:center;">$68.2$</td>
        <td style="text-align:center;">$28.6$</td>
        <td style="text-align:center;">$63.4$</td>
        <td style="text-align:center;">$ 2.0$</td>
        <td style="text-align:center;">$ 8.7 $</td>
        <td style="text-align:center;">$58.8$</td>
        <td style="text-align:center;">$80.1$</td>
        <td style="text-align:center;">$24.4$</td>
        <td style="text-align:center;">$42.6$</td>
    </tr>
    <tr>
        <td style="text-align:center;">TCL + DN</td>
        <td style="text-align:center;">$\textbf{30.5} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{54.3} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{45.4} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{73.1} \pm  0.1$</td>
        <td style="text-align:center;">$\textbf{36.2} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{70.8} \pm 0.3$</td>
        <td style="text-align:center;">$\textbf{2.6} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{10.7} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{66.7} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{81.8} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{28.5} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{48.4} \pm 0.2$</td>
    </tr>
    <tr>
        <td style="text-align:center;">TCL + DN*</td>
        <td style="text-align:center;">$ 25.1 \pm 0.1$</td>
        <td style="text-align:center;">$48.9\pm 0.2$</td>
        <td style="text-align:center;">$42.2 \pm 0.0$</td>
        <td style="text-align:center;">$71.1 \pm  0.1$</td>
        <td style="text-align:center;">$31.8 \pm 0.1$</td>
        <td style="text-align:center;">$66.8 \pm 0.2$</td>
        <td style="text-align:center;">$2.4 \pm 0.0$</td>
        <td style="text-align:center;">$9.5 \pm 0.1$</td>
        <td style="text-align:center;">$63.9 \pm 0.1$</td>
        <td style="text-align:center;">$81.1 \pm 0.0$</td>
        <td style="text-align:center;">$27.2 \pm 0.1$</td>
        <td style="text-align:center;">$45.7 \pm 0.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2107.07651" rel="noreferrer nofollow" target="_blank">ALBEF</a></td>
        <td style="text-align:center;">$37.3$</td>
        <td style="text-align:center;">$65.8$</td>
        <td style="text-align:center;">$38.5$</td>
        <td style="text-align:center;">$65.7 $</td>
        <td style="text-align:center;">$ 45.6$</td>
        <td style="text-align:center;">$81.5$</td>
        <td style="text-align:center;">$25.0 $</td>
        <td style="text-align:center;">$61.0$</td>
        <td style="text-align:center;">$66.0$</td>
        <td style="text-align:center;">$86.3$</td>
        <td style="text-align:center;">$26.9$</td>
        <td style="text-align:center;">$49.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALBEF + DN</td>
        <td style="text-align:center;">$\textbf{39.9} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{68.5} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{50.2} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{77.0} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{46.7} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{82.4} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{26.0} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{62.0} \pm 0.3$</td>
        <td style="text-align:center;">$\textbf{71.5} \pm 0.3$</td>
        <td style="text-align:center;">$\textbf{88.9} \pm 0.1$</td>
        <td style="text-align:center;">$\textbf{33.1} \pm 0.2$</td>
        <td style="text-align:center;">$\textbf{54.4} \pm 0.1$</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALBEF + DN*</td>
        <td style="text-align:center;">$38.7 \pm 0.1$</td>
        <td style="text-align:center;">$67.3 \pm 0.0$</td>
        <td style="text-align:center;">$44.8 \pm 0.2$</td>
        <td style="text-align:center;">$72.0 \pm 0.2$</td>
        <td style="text-align:center;">$46.3 \pm 0.1$</td>
        <td style="text-align:center;">$ 82.0 \pm 0.0$</td>
        <td style="text-align:center;">$\textbf{26.0} \pm 0.1$</td>
        <td style="text-align:center;">$61.7 \pm 0.2$</td>
        <td style="text-align:center;">$68.9 \pm 0.4$</td>
        <td style="text-align:center;">$87.6 \pm 0.2$</td>
        <td style="text-align:center;">$29.9 \pm 0.1$</td>
        <td style="text-align:center;">$51.9 \pm 0.1$</td>
    </tr>
</table>

#### Image Captioning Metrics

We see that on image captioning, adding DN improves upon existing baselines on Flicker8k-Expert, Flickr8k-CF, and THumb datasets.

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

#### Cross-Modal Retrieval

Below is cross-modal retrieval performance on MSCOCO and Flickr30k in the zero-shot setting against different CLIP variants.

<table>
    <tr>
        <td></td>
        <td>\multicolumn{6}{c}{MSCOCO (5K test set)}</td>
        <td>\multicolumn{6}{c}{Flickr30K (1K test set)}</td>
    </tr>
    <tr>
        <td></td>
        <td>\multicolumn{3}{c}{Image $\rightarrow$ Text}</td>
        <td>\multicolumn{3}{c}{Text $\rightarrow$ Image}</td>
        <td>\multicolumn{3}{c}{Image $\rightarrow$ Text}</td>
        <td>\multicolumn{3}{c}{Text $\rightarrow$ Image}</td>
    </tr>
    <tr>
        <td></td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
        <td>R@1</td>
        <td>R@5</td>
        <td>R@10</td>
    </tr>
    <tr>
        <td>CLIP(B16) + TTA</td>
        <td>$53.6$</td>
        <td>$77.5$</td>
        <td>$85.1$</td>
        <td>$33.8$</td>
        <td>$58.7$</td>
        <td>$69.1$</td>
        <td>$85.4$</td>
        <td>$97.9$</td>
        <td>$99.1$</td>
        <td>$66.6$</td>
        <td>$89.0$</td>
        <td>$93.7$</td>
    </tr>
    <tr>
        <td>CLIP(B16) + TTA + DN*</td>
        <td>$\textbf{54.6}$</td>
        <td>$\textbf{78.5}$</td>
        <td>$\textbf{86.1}$</td>
        <td>$\textbf{35.7}$</td>
        <td>$\textbf{60.7}$</td>
        <td>$\textbf{70.8}$</td>
        <td>$\textbf{87.3}$</td>
        <td>$\textbf{98.0}$</td>
        <td>$\textbf{99.6}$</td>
        <td>$\textbf{69.3}$</td>
        <td>$\textbf{90.2}$</td>
        <td>$\textbf{94.6}$</td>
    </tr>
    <tr>
        <td>CLIP(L14) + TTA</td>
        <td>$57.7$</td>
        <td>$80.1$</td>
        <td>$87.8$</td>
        <td>$36.8$</td>
        <td>$61.3$</td>
        <td>$71.2$</td>
        <td>$88.4$</td>
        <td>$\textbf{98.9}$</td>
        <td>$\textbf{99.9}$</td>
        <td>$69.9$</td>
        <td>$90.6$</td>
        <td>$94.8$</td>
    </tr>
    <tr>
        <td>CLIP(L14) + TTA + DN*</td>
        <td>$\textbf{58.8}$</td>
        <td>$\textbf{81.3}$</td>
        <td>$\textbf{88.4}$</td>
        <td>$\textbf{38.6}$</td>
        <td>$\textbf{63.1}$</td>
        <td>$\textbf{72.9}$</td>
        <td>$\textbf{89.3}$</td>
        <td>$98.8$</td>
        <td>$99.8$</td>
        <td>$\textbf{72.1}$</td>
        <td>$\textbf{91.7}$</td>
        <td>$\textbf{95.5}$</td>
    </tr>
    <tr>
        <td>CLIP(B32-Laion) + TTA</td>
        <td>$58.5$</td>
        <td>$80.9$</td>
        <td>$88.1$</td>
        <td>$40.0$</td>
        <td>$65.8$</td>
        <td>$76.0$</td>
        <td>$85.8$</td>
        <td>$96.7$</td>
        <td>$98.9$</td>
        <td>$71.1$</td>
        <td>$91.4$</td>
        <td>$\textbf{94.8}$</td>
    </tr>
    <tr>
        <td>CLIP(B32-Laion) + TTA + DN*</td>
        <td>$\textbf{60.7}$</td>
        <td>$\textbf{82.3}$</td>
        <td>$\textbf{88.9}$</td>
        <td>$\textbf{40.8}$</td>
        <td>$\textbf{66.5}$</td>
        <td>$\textbf{76.4}$</td>
        <td>$\textbf{86.6}$</td>
        <td>$\textbf{97.1}$</td>
        <td>$98.9$</td>
        <td>$\textbf{71.7}$</td>
        <td>$\textbf{91.6}$</td>
        <td>$94.7$</td>
    </tr>
</table>

#### Zero-shot Classification

Finally, we present zero-shot classification performance on ImageNet1K, Cifar100, SUN397, Stanford Cars, Caltech 101, and Flowers 102.

<table>
    <tr>
        <td></td>
        <td>\multicolumn{2}{c}{ImageNet1K}</td>
        <td>\multicolumn{2}{c}{Cifar100}</td>
        <td>\multicolumn{2}{c}{SUN397}</td>
        <td>\multicolumn{2}{c}{Stanford Cars}</td>
        <td>\multicolumn{2}{c}{Caltech 101}</td>
        <td>\multicolumn{2}{c}{Flowers 102}</td>
    </tr>
    <tr>
        <td></td>
        <td>Acc@1</td>
        <td>Acc@5</td>
        <td>Acc@1</td>
        <td>Acc@5</td>
        <td>Acc@1</td>
        <td>Acc@5</td>
        <td>Acc@1</td>
        <td>Acc@5</td>
        <td>Acc@1</td>
        <td>Acc@5</td>
        <td>Acc@1</td>
        <td>Acc@5</td>
    </tr>
    <tr>
        <td>CLIP(B16) + TTA</td>
        <td>$67.1$</td>
        <td>$91.5$</td>
        <td>$67.7$</td>
        <td>$90.1$</td>
        <td>$60.0$</td>
        <td>$91.4$</td>
        <td>$63.3$</td>
        <td>$93.1$</td>
        <td>$84.5$</td>
        <td>$\textbf{96.7}$</td>
        <td>$69.8$</td>
        <td>$84.8$</td>
    </tr>
    <tr>
        <td>% CLIP(B16) + TTA + DN *</td>
        <td>$\textbf{67.8}$</td>
        <td>$\textbf{92.0}$</td>
        <td>$\textbf{71.1}$</td>
        <td>$\textbf{92.2}$</td>
        <td>$\textbf{61.9}$</td>
        <td>$\textbf{92.4}$</td>
        <td>$\textbf{64.6}$</td>
        <td>$ \textbf{93.9}$</td>
        <td>$\textbf{84.9}$</td>
        <td>$\textbf{96.7}$</td>
        <td>$69.8$</td>
        <td>$84.8$</td>
    </tr>
    <tr>
        <td>CLIP(B16) + TTA + DN *</td>
        <td>$\textbf{67.8}$</td>
        <td>$\textbf{92.0}$</td>
        <td>$\textbf{71.1}$</td>
        <td>$\textbf{92.2}$</td>
        <td>$\textbf{61.9}$</td>
        <td>$\textbf{92.4}$</td>
        <td>$\textbf{64.6}$</td>
        <td>$\textbf{93.9}$</td>
        <td>$\textbf{84.9}$</td>
        <td>$96.6$</td>
        <td>$\textbf{70.0}$</td>
        <td>$\textbf{85.0}$</td>
    </tr>
    <tr>
        <td>CLIP(L14) + TTA</td>
        <td>$73.1$</td>
        <td>$93.4$</td>
        <td>$77.6$</td>
        <td>$94.0$</td>
        <td>$62.1$</td>
        <td>$92.5$</td>
        <td>$76.3$</td>
        <td>$97.7$</td>
        <td>$86.0$</td>
        <td>$97.3$</td>
        <td>$72.8$</td>
        <td>$89.1$</td>
    </tr>
    <tr>
        <td>CLIP(L14) + TTA + DN *</td>
        <td>$\textbf{74.2}$</td>
        <td>$\textbf{94.1}$</td>
        <td>$\textbf{80.4}$</td>
        <td>$\textbf{95.4}$</td>
        <td>$\textbf{63.8}$</td>
        <td>$\textbf{93.3}$</td>
        <td>$\textbf{77.2}$</td>
        <td>$\textbf{97.8}$</td>
        <td>$\textbf{86.2}$</td>
        <td>$97.1$</td>
        <td>$\textbf{74.0}$</td>
        <td>$89.1$</td>
    </tr>
    <tr>
        <td>CLIP(B32-Laion) + TTA</td>
        <td>$66.9$</td>
        <td>$89.4$</td>
        <td>$75.7$</td>
        <td>$94.0$</td>
        <td>$63.9$</td>
        <td>$93.5$</td>
        <td>$87.1$</td>
        <td>$\textbf{99.2}$</td>
        <td>$\textbf{87.3}$</td>
        <td>$\textbf{97.7}$</td>
        <td>$70.3$</td>
        <td>$\textbf{86.4}$</td>
    </tr>
    <tr>
        <td>CLIP(B32-Laion) + TTA + DN *</td>
        <td>$\textbf{67.2}$</td>
        <td>$\textbf{90.3}$</td>
        <td>$\textbf{76.2}$</td>
        <td>$\textbf{94.3}$</td>
        <td>$\textbf{64.3}$</td>
        <td>$\textbf{93.7}$</td>
        <td>$87.1$</td>
        <td>$99.1$</td>
        <td>$86.8$</td>
        <td>$97.2$</td>
        <td>$\textbf{71.1}$</td>
        <td>$85.8$</td>
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
