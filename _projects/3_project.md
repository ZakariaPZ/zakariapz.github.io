---
layout: page
title: Composition Controlled Diffusion
description: Using bounding boxes to generate the desired image.
img: assets/img/diffusion_animation.gif
importance: 3
category: 
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ilgd.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Diffusion models have become the go-to solution for high-quality image generation from text prompts. However, controlling **where** objects appear in these generated images remains challenging. Traditional methods either require additional training or sacrifice image quality for layout control. Enter **Injection Loss-Guided Diffusion (iLGD)**—a **training-free** approach that delivers precise layout control without compromising image quality.

## What is iLGD?

**iLGD** combines two complementary techniques:

1. **Attention Injection (AI):**  
   Directly modifies cross-attention maps during early denoising steps to nudge the model toward a desired layout.  
   
   $$A'_t = \text{Softmax}\left(Q_tK_t^\top + \nu_t m / \sqrt{d_k}\right)$$


2. **Loss Guidance (LG):**  
   Refines the image by applying a loss on attention maps, guiding objects to stay within specified regions.  
   
   $$\hat{s}_\theta(z_t, t) = s_\theta(z_t, t) - \eta \nabla_{z_t} \ell_y(z_t)$$

Together, these methods ensure that layouts are both precise and visually coherent.

## How Does iLGD Perform?

The authors benchmarked **iLGD** against popular methods like **BoxDiff**, **Chen et al.**, and **MultiDiffusion** using prompts with bounding box constraints. The results?  

- **Higher text-to-image similarity** (T2I-Sim)  
- **Better object localization** (AP@0.5 via YOLOv4)  
- **Superior image quality** (CLIP-IQA scores)  

Notably, **iLGD** achieves these results **without additional training** and **fewer visual artifacts**.
<!-- 
| Method        | T2I-Sim ↑ | CLIP-IQA (Quality) ↑ | AP@0.5 ↑ |
|---------------|-----------:|----------------------:|---------:|
| Stable Diffusion | 0.303   | 0.928                | —        |
| BoxDiff       | 0.305     | 0.922                | 0.192    |
| **iLGD (Ours)** | **0.309** | **0.961**            | **0.202**| -->

While existing diffusion models excel in generating realistic images, they lack fine-grained spatial control. **iLGD** bridges this gap by using attention injection for coarse control and loss guidance for precise refinement—without retraining. This approach could benefit applications like content creation, design automation, and beyond.

[Paper](https://arxiv.org/abs/2405.14101) and [code](https://github.com/ZakariaPZ/loss-guided-layout-control).


