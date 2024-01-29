# Retentive Network: A Successor to Transformer Architecture for Large Language Models (2023)

[The original paper](https://arxiv.org/pdf/2307.08621.pdf)

Microsoft researchers found a new way to train LLMs faster - Retentive Networks (RetNets). I've not seen anywhere actually use this yet in production but I would be very interested to see if the results hold up. 

## What are the issues with Transformers?

Their inference time is $O(N^2)$, additionally they have $O(N)$ memory complexity as well. Many papers and architectures have tried to cut these times down, however this would always come at the cost of performance. RetNets, however, are $O(1)$ in memory and $O(N)$ in inference time. The following table is taken from the paper itself.

![Architecture Performance Table](..\..\..\images\data_science\llm_diary\articles\retentive_network\retnet_table.png)

The bottleneck in the transformer is attention. Specifically, the softmax function applied after the dot product between vectors $K$ and $Q$ causes the $O(N^2)$ complexity. The linear transformer took this out and only used the dot product in the formula for attention, however its performance wasn't anywhere near as good.

## What is special about Retentive Networks?

The thing done differnetly here compared to other architectures is a "retention" formula, derived in the paper:

For input $X$ projected to space $n$, we have $v(n) = X_n \cdot \omega_{V}$. Additionally, we make our matricies $K$ and $Q$ "content aware", by setting them to $Q = XW_{Q}, K = XW_{K}$. NB: whenever we have a $W$ matrix, it usually corresponds to weights. Now, if we let $\gamma$ and $\theta$ be vecotrs in $\mathbb{R}^{n}$, we have equations 

$$ s_n = AS_{n-1} + K_{n}^{T} v_n$$
$$ o_n = Q_n s_n = \sum_{m=1}^{n} \gamma^{n-m} (Q_n e^{in\theta}) (K_{m}^{T} e^{im\theta})^{\dagger} v_{m}$$

Which form our retention mechanism and allow for recurrence (according to the paper, I'm not certain what that means).

This gives our output for each step of the sequence. If we wanted to describe a retentive layer in Matrix terms, it would look like the following:

$$ X = (QK^{T} \odot D)V$$

Where:

* $\odot$ is the Hadamard product $(A \odot B)_{ij} = (A_{ij})(B_{ij})$
* $Q = XW_{Q} \odot \Theta$
* $K = XW_{K} \odot \bar{\Theta}$
* $V = XW_{V}$
* $\Theta = e^{in\theta}$
* $D_{nm}=\begin{cases}
\gamma^{n-m}, n \geq m\\
0, n < m
\end{cases}$

After the transformation $X$, group normalization is used and we get the output of our layer. 

## How do we put this into a full Neural Network?

Similar to attention, we can mutli-head this. We can also use multi-scale retention, where we use a different $\gamma$ for each layer. 

* $\text{head}_i = \text{Retention}(X,\gamma_i)$ 
* $Y = \text{GroupNorm}(\text{Concat}(\text{head}_1, \dots, \text{head}_n))$
* $\text{MSR}(X) = (\text{swish}(XW_G) \odot Y)W_o$

We use this and a feed-forward NN. We thus have the following equations:
* $Y^{l} = \text{MSR}(\text{LN}(X^{i}))+X^{l}$
* $X^{l+1} = \text{FFN}(\text{LN}(Y^{l})) + Y^{l}$

Where LN is layer normalization and $\text{FFN}(X) = \text{gelu}(XW_1)W_2$

For training purposes, you can chunk up the sequences and train on those smaller batches.

## Questions

* What metrics are used to judge the performance of an LLM?