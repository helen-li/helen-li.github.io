---
layout: page
title: Genderation
description: platform for exploring gender bias in generative T2I models.
img: assets/img/genderation.png
importance: 1
category: ai/data
---

<div class = "projheader">
    <div class="links"><a href="https://genderation.github.io/" class="btn z-depth-0" role="button"> Website </a></div>
    <div class="links"><a href="https://github.com/qylisayu/GENDERation" class="btn z-depth-0" role="button"> <i class="fab fa-github gh-icon"></i> Github</a></div>
</div>

Determined to improve my understanding of gender bias in algorithms, I leveraged resources at the AI4Good Lab to dig deeper via proposing my own project to quanitfy and mitigate gender bias in text-to-image (T2I) models.
Gender bias in AI research has largely resided within the realm of computational linguistics and natural language processing. 
Famous past studies have worked on detecting misogyny in speech, analyzing the role of gender identity in decision-making systems (e.g., resume screening), and identifying explicit biases in machine translation and word embeddings. 

With the rapid rise of generative AI, however, I grew concerned at the lack of gender bias research in text-to-image models like Stable Difussion and DALL-E.
A simple preliminary Google search supported my hypothesis—this article from the <a href="https://www.technologyreview.com/2022/12/13/1064810/how-it-feels-to-be-sexually-objectified-by-an-ai/">MIT Technology Review</a> discussed how Lensa AI generated many overtly sexualized avatar images to represent an Asian woman user. 
<blockquote>
    Funnily enough, I found more realistic portrayals of myself when I told the app I was male. This probably applied a different set of prompts to images. The differences are stark. In the images generated using male filters, I have clothes on, I look assertive, and—most important—I can recognize myself in the pictures.  
</blockquote>

Together with a team of 6 students and 4 mentors, I embarked on a three-week-long journey in search of ways to contribute to this space. 
Due to limitations in compute and open-sourced models, we centralized our analysis to two models: <span style="color: var(--global-theme-color);">Stable Difussion v1.4</span> and <span style="color: var(--global-theme-color);">VQGAN+CLIP</span>.
We gathered a dataset of 2,200 images using these models, implemented various embedding-based bias quantfiers, proposed a more gender-fluid labeling scale for gender expression, and integrated these into an interactive platform as a final product. 
We narrowed the project scope to answer the following 3 focus questions, and we presented our results, code, and product to academic researchers and industry professionals at the <a href="https://mila.quebec/en/">Mila</a> research institute in Montréal, Québec. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-demo-2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-demo-3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Showcasing our findings in both booth (left) and presentation (right) formats. 
</div>

#### Question 1: Do T2I models reflect and/or amplify societal gender biases?

T2I models not only reflect but also seem to amplify gender biases in the occupational context. 
<span style="color: var(--global-theme-color);">Figure 1</span> compares the proportion of female-presenting individuals for 6 different jobs between real-world labor statistics and our generated image dataset, and across almost all the industries, Stable Diffusion model outputs tend to underrepresent females in areas where they are already the minority gender group and vice versa for overrepresentation. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-q2-1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1. Percentage (%) of employed persons who identify as female in the real world versus SD-generated profession images that are feminine-presenting.
</div>

More interestingly, there seems bias against androgynous presentations of individuals.
For instance, representation of androgynous individuals jumps from 0% to 18% when Stable Diffusion was prompted to generate images of rich versus poor individuals, as shown below in <span style="color: var(--global-theme-color);">Figure 2</span>. 
We see similar jumps in female proportions when comparing the generated outputs of assertive versus emotional individuals. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0 d-flex justify-content-center">
        {% include figure.html path="assets/img/genderation/genderation-q2-2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2. Count of gender presentation labels for the SD-generated adjective images. Use the dashboard to visualize the exact distributions over the gender spectrum scale.
</div>

#### Question 2: How can T2I models better represent gender?

For the purpose of further analysis, we needed to label images generated by neutral prompts like “photo portrait of a doctor” with gender which comes with its own ethical complications. 
After much deliberation and conversations with Responsible AI experts, we decided to move forward with a CLIP-based embedding approach that would label gender expression on a spectrum of 10 classes from most feminine presenting towards most masculine presenting with androgynous in between. 
First, we compute two embeddings for the words “female” and “male” as the benchmark to compare against using CLIP. 
Then, we calculated an image-text similarity score for each generated image, fed these scores into the softmax function, and interpreted the outputs as the probability of the image belonging to one of the 10 different classes on the spectrum.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-q3-1.png" title="example image" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Figure 3. Distribution of gender spectrum labels for output images from the prompt <span style="color: var(--global-theme-color);">"photo portrait of a nonbinary doctor"</span>.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-q3-2.png" title="example image" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Figure 4. Distribution of gender spectrum labels for output images from the prompt <span style="color: var(--global-theme-color);">"photo portrait of a nonbinary programmer"</span>.
        </div>
    </div>
</div>

I wanted to further leverage the fact that our T2I models and labelling mechanism both rely on CLIP embeddings. 
Based on this realization, I hypothesized that the labelled distribution for images generated by Stable Diffusion should somewhat match the distribution assumed by the given prompt. 
In other words, the labeled outputs of the prompt "photo of a female doctor" should overlap with a distribution that's skewed toward the feminine presenting side. 
However, I observed on several occasions how the distribution of the generated images for nonbinary identity prompts don’t live up to this expectation.
As visualized in <span style="color: var(--global-theme-color);">Figures 3 and 4</span>, the distribution is still quite skewed to either end of the spectrum, reflecting the large space for improvement within T2I models to become more gender-inclusive. 

#### Question 3: How to increase awareness of current biases in T2I model?

Check out this <a href="https://youtu.be/i6OsaanS2Hw">video demo</a> of the GENDERation dashboard, an interactive platform that empowers users to reshape their understanding of gender biases in generative AI. 
Built mostly with Python and the dash package, users can interact with the two T2I models and our CLIP-based labeling method. 
Together with another teammate, I also implemented and integrated bias quantifiers into this platform including the EATS (Embedding Association Test Score) and MCAS (Multimodal Composite Association Score).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/genderation/genderation-demo-1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Capturing the aftermath of a successful demo day in a big group photo!
</div>

{% raw %}```#python```{% endraw %}
{% raw %}```#dash```{% endraw %}
{% raw %}```#google-colab```{% endraw %}
{% raw %}```#clip```{% endraw %}
{% raw %}```#gan```{% endraw %}
{% raw %}```#numpy```{% endraw %}
{% raw %}```#pandas```{% endraw %}