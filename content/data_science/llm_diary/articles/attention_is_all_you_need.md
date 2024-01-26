# Attention is all you need (2017)

The reason I came to this paper first is because it's the one that introduced the transformer, the key to understanding how LLM architecture works. The number one picture you'll always see for the architecture is the one below:

![Transformer Architecture](..\..\..\images\transformer_architecture.png)

This comes straight from the paper itself and is very clear, hence why it is the go-to image for describing transformer architecture. However, there are a key questions to ask from this picture: **What is "Multi-head attention?"**

## What is attention?

Attention can be put rather nicely into a small mathematical equation:

$$ \text{attention}(Q,K,V) = \text{softmax}(\frac{QK^{T}}{\sqrt{d_k}})V$$

I say "rather nicely" mainly because the equation is concise. This doesn't really say anything about what it is doing.
$Q,K,V$ are all vectors in $\mathbb{R}^{n}$ which are effectively an encoded "query", "key" and "value". These are never really explained much in the paper and I mainly assume they were ubiquitous during the time of its writing. From reading "the illustrated transformer" - they are described as "abstractions" of these concepts - which I take to mean vector encodings. In an LLM context, I imagine this to be prompt, context and output. I could be wrong though.

Worth noting the $\sqrt{d_k}$ is a scale factor where $d_k$ is the dimension of the vectors, as we want to make sure we don't get any issues with vanishing/exploding gradients.

## What makes it multi-head?

To make it multi-headed, we project the vectors into multiple different (sub-?)spaces and then apply attention on these projections and concatenate the resulting vectors together.

## What is self-attention? The type used here?

This is where the query, key and value are provided from the same place. "The illustrated transformer" describes self-attention as the ability to take a sentence and figure out what words are being referred to by one particular word in the sentence. 

We also have "encoder-decoder attention" which corresponds to the arrow coming out of the left cell and into the right cell. This is where the keys and queries come from the previous encoder-decoder.

## Word on Group Normalization

Here, the normalisation used is called "Group Normalization", which was first written about by meta researchers in 2020. The idea behind it is when normalising over a batch to get a mean and standard deviation over it, they wanted to find a way that wasn't dependent on batch dimension - as this lead to batch normalization increasing in error. Instead, you take your samples and split them into different "groups" of channels and average over them - this mitigates the issue but I'm not 100% clear how.

## Questions that I need to follow up on

* How does parrallelistion work in practise?
* What does encoder-decoder stuff actually do?
* What do Q,K and V actually look like in practise?
* What is byte-pair encoding and how is it done?
* What makes "Retentive Networks" so special?
* What is "perplexity" and why is it used as a metric?