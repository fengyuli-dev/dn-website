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

```{=latex}
\begin{table*}[h]
\begin{center}
\resizebox{\columnwidth}{!}{
\begin{tabular}{ l | c c c c c c | c c c c c c }
\toprule[1pt]
&\multicolumn{6}{c}{MSCOCO (5K test set)} & \multicolumn{6}{c}{Flickr30K (1K test set)} \\
&\multicolumn{3}{c}{Image $\rightarrow$ Text} & \multicolumn{3}{c}{Text $\rightarrow$ Image} & \multicolumn{3}{c}{Image $\rightarrow$ Text} & \multicolumn{3}{c}{Text $\rightarrow$ Image}\\
\midrule
 & R@1 & R@5 & R@10 & R@1 & R@5 & R@10 & R@1 & R@5 & R@10 & R@1 & R@5 & R@10\\
CLIP \cite{clip} & $52.4$ & $76.0$ & $84.5$ &$ 30.2$ & $55.1$ & $66.4$ & $81.3$ & $95.0$ & $98.5$ & $62.7$ & $86.0$ & $92.0$\\
CLIP + TTA \cite{shanmugam2021better} & $53.9$ & $77.5$ & $85.5$ & $32.1$ & $57.5$ & $68.3$ & $83.2$ & $96.8$ & $98.4$ & $65.2$ & $87.9$ & $92.9$\\
CLIP + TTA + DN  & $53.6 \pm 0.1$ & $76.9 \pm 0.1$ & $84.8 \pm 0.1$ & $\textbf{34.8} \pm 0.0$ & $\textbf{60.4} \pm 0.0$ & $\textbf{70.8} \pm 0.1$ & $\textbf{85.8} \pm 0.2$ & $\textbf{97.5} \pm 0.1$ & $\textbf{99.1} \pm 0.0$ & $\textbf{68.1} \pm 0.1$ & $\textbf{89.4} \pm 0.1$ & $\textbf{94.1} \pm 0.0$\\
CLIP + TTA + DN* & $\textbf{54.7} \pm 0.1$ & $\textbf{77.8} \pm 0.1$ & $\textbf{85.6} \pm 0.1$ & $33.8 \pm 0.0$ & $59.4 \pm 0.0$ & $70.1 \pm 0.0$ & $\textbf{85.8} \pm 0.1 $ & $\textbf{97.5} \pm 0.1$ & $98.8 \pm 0.1$ & $67.6 \pm 0.0$ & $89.1 \pm 0.0$ & $93.9 \pm 0.1$\\
CLIP + DN & $51.7 \tiny{\pm 0.1}$ & $75.8 \tiny{\pm 0.1}$& $84.0 \tiny{\pm 0.1}$ &$ 33.4 \tiny{\pm 0.0}$& $58.6 \tiny{\pm 0.1}$ & $69.4 \tiny{\pm 0.1}$ & $83.3 \tiny{\pm 0.2} $ & $96.4 \tiny{\pm 0.1}$ & $98.6 \tiny{\pm 0.1}$ & $66.2 \tiny{\pm 0.1}$ & $88.2 \tiny{\pm 0.1}$& $93.3 \tiny{\pm 0.1}$\\
CLIP + DN* & $52.9 \tiny{\pm 0.1}$ & $76.4 \tiny{\pm 0.1}$& $84.9 \tiny{\pm 0.1}$ &$ 32.1 \tiny{\pm 0.1}$& $57.4 \tiny{\pm 0.0}$ & ${68.3} \tiny{\pm 0.1}$ & ${83.5} \tiny{\pm 0.1} $ & $96.2 \tiny{\pm 0.0}$ & $98.5 \tiny{\pm 0.1}$ & $64.8 \tiny{\pm 0.2}$ & $87.5 \tiny{\pm 0.1}$& $93.1 \tiny{\pm 0.0}$\\
\midrule
TCL \cite{tcl} & $57.6$ & $84.3$ & $91.8$ & $41.8$ & $70.6$ &$ 80.6$ &$ 73.8$ & $93.3$ &$ 96.9$ & $59.1$ & $84.6$ & $91.1$\\
TCL + DN  & $\textbf{60.6} \tiny{\pm 0.1}$ & $\textbf{85.8} \tiny{\pm 0.1}$ & $\textbf{92.4} \tiny{\pm 0.1}$ & $\textbf{43.2} \tiny{\pm 0.0}$ & $\textbf{71.8} \tiny{\pm 0.1}$ & $\textbf{81.6} \tiny{\pm 0.0}$ & $77.5 \tiny{\pm 0.5}$ & $94.1 \tiny{\pm 0.2}$ & $\textbf{96.9} \tiny{\pm 0.2}$ & $59.8 \tiny{\pm 0.2}$ & $84.9 \tiny{\pm 0.1}$ & $91.1 \tiny{\pm 0.1}$\\
TCL + DN* & $59.5 \tiny{\pm 0.1}$ & $85.2 \tiny{\pm 0.0}$ & $92.2 \tiny{\pm 0.1}$ & $42.7 \tiny{\pm 0.0}$ & $71.5 \tiny{\pm 0.0}$ & $81.3 \tiny{\pm 0.0}$ & $75.5 \tiny{\pm 0.0}$ & $\textbf{94.4} \tiny{\pm 0.1}$ & $ 96.9 \tiny{\pm 0.1}$ & $\textbf{60.0} \tiny{\pm 0.1}$ & $\textbf{85.1} \tiny{\pm 0.0}$ & $91.1 \tiny{\pm 0.0}$\\
\midrule
ALBEF \cite{albef} & $62.5$ & $85.9$ &$ 92.2$ & $40.2 $& $68.4$ & $78.9$ & $78.2$ & $95.5$ & $97.9$ & $59.9$ & $84.8$ & $90.6$ \\
ALBEF + DN  &$ \textbf{63.0} \tiny{\pm 0.2}$ & $85.8 \tiny{\pm 0.1}$ & $92.4 \tiny{\pm 0.1}$ & $\textbf{44.8} \tiny{\pm 0.1}$ & $\textbf{72.5} \tiny{\pm 0.0}$ & $\textbf{82.0} \tiny{\pm 0.0}$ & $\textbf{80.6} \tiny{\pm 0.1}$ & $\textbf{96.2} \tiny{\pm 0.1}$ & $\textbf{98.3} \tiny{\pm 0.1}$ & $\textbf{64.1} \tiny{\pm 0.0}$ & $\textbf{87.1} \tiny{\pm 0.1}$ & $\textbf{92.3} \tiny{\pm 0.1}$\\
ALBEF +DN* &$ \textbf{63.0} \tiny{\pm 0.1}$ & $\textbf{86.0} \tiny{\pm 0.0}$ & $\textbf{92.5} \tiny{\pm 0.1}$ & $42.8 \tiny{\pm 0.1}$ & $70.8 \tiny{\pm 0.0}$ & $80.7 \tiny{\pm 0.0}$ & $79.2 \tiny{\pm 0.1}$ & $\textbf{96.2} \tiny{\pm 0.0}$ & $98.0 \tiny{\pm 0.0}$ & $62.4 \tiny{\pm 0.1}$ & $86.1 \tiny{\pm 0.1}$ & $91.9 \tiny{\pm 0.1}$\\
\bottomrule[1pt]
\end{tabular}}
\caption{Cross-modal retrieval performance on MSCOCO and Flickr30K in the zero-shot setting. Means for DN are estimated using 100 random unlabeled validation samples. Average recalls and standard deviations are calculated with 5 random seeds.}
\label{table:zeroshot_retrieval}
\end{center}
\end{table*}
```

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
</div></div>
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

<!-- Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
``` -->
