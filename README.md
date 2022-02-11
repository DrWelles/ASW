# Training strategies for automatic song writing: a unified framework perspective (ICASSP 2022)

Training strategies for automatic song writing: a unified framework perspective

## More Details of Paper

Automatic song writing (ASW) aims to allow machines to gener-ate associated melodies and lyrics. 

In previous literature, four taskshave  been  investigated  for  ASW: 

* lyrics-to-lyrics  generation  (L2L,generating lyrics from initial lyrics) 
* melody-to-melody gener-ation (M2M, generating melody from initial melody)
* lyric-to-melody generation (L2M, generating melody from correspondinglyrics)
* melody-to-lyric generation (M2L, generating lyricsfrom corresponding melody)

We aim to address these challenges with a unifiedframework for ASW under the low resource condition.  Based  on SongMASS, we proposed a more general pre-training strategy that exploits unpaired data and a new dual transformation loss that better utilzes paired data to further alleviate the data scarcity issue of paired data.

![image-20220211174427854](https://github.com/DrWelles/ASW/blob/main/pic/image-20220211174427854.png)

### Pre-training With Unpaired data

We adapt GPT-like self-supervised learning task as our pre-training strategy rather than MASS.

GPT-like self-supervised learning task (encoder only pre-training task) can be formulated as follow:

![1](http://latex.codecogs.com/svg.latex? L(coppus) = \Sigma_{n=K}^N p(token_n|token_{n-K},token_{n-K+1},...,token_{n-1};\Theta) )

where $coppus$ is a token sequence with $N$ tokens ($ coppus = (token_1,token_2,...,token_{N})$) and $\Theta$ is the parameters of model. In our paper $\Theta$ consists of Transformer encoders $\theta_x$ and decoders $\theta_y^*$ (the  part of decoder paramters is skipped on pre-training stages).

MASS task is a kind of encoder-decoder pre-training pre-training task which results huge inductive bias for model and hurts the performance for single domian generation tasks (L2L, M2M).

![image-20220211173228031](https://github.com/DrWelles/ASW/blob/main/pic/image-20220211173228031.png)

The table above is the perplexity results with different pre-training setting. S0 is the MASS pre-training and S1 and S2 stand for the two pre-training stages in Section 2.1 of our paper.  The best performance of MASS strategy is $+S0+S1$ , whose avergae perplexity  is 22.82, far from baseline performance. We did not report the SongMASS results in all  experiments for a fair comparision consideration due to the large gap in objective  indicators.



### Fine-tuning With Paired Data

We adapt Machine Translation framework for corss domain generation tasks (L2M,M2L), which means the melody and lyric can be treated as two kinds of languages and can be formated as follow:

$ L(tar|src;\Theta) = \Sigma_{n=1}^{|tar|} p(tar_n|src;tar_{<n};\Theta) $

where $tar$ is a token sequence with $|tar|$ tokens  and $\Theta$ is the parameters of model. In our paper $\Theta$ consists of Transformer encoders $\theta_x$ and decoders $\theta_y$ (all decoder paramters will be trained on fine-tuning stage).  

The paired data is much less than unpaired data and we add dual  transformation  loss that better utilzes paired data.

### Dual  Transformation  Loss

![image-20220211174635962](https://github.com/DrWelles/ASW/blob/main/pic/image-20220211174635962.png)



We use paired data $(X,Y)$ for cross domain generation tasks (M2L and L2M). Dual transformation  loss aims to reconstruct the $Y$ from $\hat{X}$ (predicted $X$) or reconstruct the $X$ from $\hat{Y}$ (predicted $Y$). This cycle procedure may enforce the weak correlation between melody and lyrics.



## We will release the code after formatting.





