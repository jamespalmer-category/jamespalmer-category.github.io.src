# The Era of 1-bit LLMs: All Large Language Models are in 1.58 Bits

[The original paper](https://arxiv.org/pdf/2402.17764.pdf)

Something interesting dropped very recently, the idea that not only could we use ternery values of 1,0,-1 for every weight in an LLM to save memory, but also the fact that this can still preserve a very good level of performance as well. 

The paper pulls no punches when it comes to blowing its own trumpet, titling its first section “The era of 1-bit LLMs” and introducing BitNet b1.58. 

## The idea and why we need small-memory LLMs

The idea of 1 bit LLMs had already been floated around, where each value would be 1 or -1. The issue with this idea was that the LLMs sucked after doing such a thing.

As well, ideas in the past for LLMOps solutions to save memory had involved changing the weight from float16 (float values that take up 16 bytes of memory) to 8/4 bytes in inference time. 

There is an issue, however, which is that a 7 billion parameter model is still taking up 4 bytes per parameter. Therefore, we'll still end up with a model whose size is 28 GB. 

Of course, for researchers this doesn't necessarily matter. When pushing for state-of-the-art and given all the cloud computing resources in the world, you can basically store what you like no matter how big. 

For someone trying to deploy an LLM on an app however, you've got to be considerate of a user's phone space, your server space and the fact that if you want inference to take place user-side then you'll most likely be dealing with a CPU. Hence why performance optimization is researched on many different platforms.

## Addition is all you need

What manages to speed up the training and inferences of the ‘1.58 bit’ LLMs is the fact that now we're only dealing with 0,1,-1, we don't have to worry about matrix multiplication due to the lack of numbers involved. We're only doing addition and subtraction instead.

To reflect this in code, instead of using nn.Linear from PyTorch, a bitLinear layer is used instead. Additionally, the paper suggests instead of using GPUs which are made specifically for quick matrix calculations, a new hardware specialising in these equations could be made instead.

It is also worth noting that activation functions are now limited to 8 bit instead, with a rounding equation used (I don't fully understand how it works - unsure why it doesn't end with 1 every time) to determine the new weights.

## Initial results

Using LLaMA as a benchmark, the results look incredible. One that popped out to me was the 71x decrease in energy consumption of BitNet b1.58. 

Additionally, compared to StableLM-3B, BitNet b1.58 3B is highly competitive when it comes to QA when both models are trained with 2K tokens. 

Other metrics, like latency and throughput, show very promising results as well.