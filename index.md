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
````

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

- This is an unordered list following a header.
- This is an unordered list following a header.
- This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
| :----------- | :---------------- | :---- |
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

---

### Here is an unordered list:

- Item foo
- Item bar
- Item baz
- Item zip

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
```
$$
