---
author: Rahul Kumar
pubDatetime: 2026-05-08T09:00:00Z
title: Writing posts — a quick reference
slug: writing-posts-quick-reference
featured: false
draft: false
tags:
  - meta
  - reference
description:
  How to add new posts, embed images and graphs, format code, and render math. A note to self.
---

A reference for myself on how to write posts on this blog. Delete or replace this once it stops being useful.

## Table of contents

## Adding a new post

Create a new markdown file in `src/data/blog/` — the filename becomes the URL slug unless you set one explicitly. Every post needs frontmatter at the top:

```yaml
---
author: Rahul Kumar
pubDatetime: 2026-05-08T10:00:00Z
title: Your post title
slug: your-post-slug
featured: false
draft: false
tags:
  - tag1
  - tag2
description: A one-line summary that shows up in previews and SEO.
---
```

Set `draft: true` while you're still working on it — drafts won't appear on the site.

## Embedding images and screenshots

Drop the image into `public/` (or a subfolder like `public/posts/`) and reference it:

```markdown
![Alt text describing the image](/posts/my-screenshot.png)
```

For a captioned figure, use HTML:

```html
<figure>
  <img src="/posts/loss-curve.png" alt="Training loss curve" />
  <figcaption class="text-center">
    Training loss across 50 epochs.
  </figcaption>
</figure>
```

Charts and plots from notebooks work the same way — export them as PNG or SVG and drop them in `public/`.

## Code blocks

Triple-backtick fenced blocks with a language hint give you syntax highlighting:

```python
import torch
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, d_in, d_hidden, d_out):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(d_in, d_hidden),
            nn.GELU(),
            nn.Linear(d_hidden, d_out),
        )

    def forward(self, x):
        return self.net(x)
```

You can also highlight specific lines:

```python {2,5-6}
def train_step(model, batch):
    x, y = batch                          # highlighted
    pred = model(x)
    loss = F.cross_entropy(pred, y)
    loss.backward()                       # highlighted
    optimizer.step()                      # highlighted
    return loss.item()
```

## Math

Inline math like $f(x) = \sigma(Wx + b)$ and display math both work:

$$
\mathcal{L} = -\sum_{i=1}^{N} y_i \log \hat{y}_i
$$

> Note: math rendering needs a small config tweak in `astro.config.ts` to enable KaTeX. Add it when you write your first math-heavy post.

## Callouts and quotes

> Blockquotes work for callouts, asides, and pull quotes.

## Links and references

Standard markdown — `[link text](https://example.com)`. For paper references, I'll use footnotes: like this[^attn].

[^attn]: Vaswani et al., *Attention Is All You Need*, NeurIPS 2017.

## Publishing

Once a post is ready (`draft: false`), commit and push to GitHub. The site rebuilds and deploys automatically via GitHub Actions — usually live within a minute.
