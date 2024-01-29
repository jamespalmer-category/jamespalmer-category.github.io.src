# Adapting Large Language Model via Reading Comprehension (2023)

[The original paper](https://arxiv.org/pdf/2309.09530.pdf)

Something I've recently been interested in is how you can get an LLM to read a document and answer questions about it as if it is taking a reading comprehension test. Obviously, this is a good benchmark to test the understanding of an LLM; testing it against things that come naturally to humans is a good way of understanding its "cognitive" ability to some extent. 

Additionally, it is a use case in and of itself for LLMs. If I give it a document that it learns from, then I can have people query the bot and use it as an assistant in understanding the documents. Step in AdaptLLM, a way of adapting the training data of LLMs to improve their ability at domain-specific reading comprehension.

## What makes AdaptLLMs special?

Well, ugh, nothing to do with the architecture itself. Instead, what it does differently is it changes the raw data corpora to put it into the structure of questions you would see in a reading comprehension with expected answers as well, just using Regular Expressions for this initial bit of parsing.

The tasks types given were:

* Summarisation of the text
* Word-to-text (describe what a domain keyword means)
* Natural Language Inference (given two sentences, does one entail the other?)
* Commonsense Reasoning (What if the effects of {input sentence 1}?) {input sentence 2}
* Paragraph Detection (Compose a sentence to support/contradict "{input sentence 1}") {input sentence 2}
* Text completion (How would you complete this article?)

It is worth noting where we have the 2 input sentences, it's because we're giving it a few-shot reasoning capability by giving it the questions and the answers in the training text.

## How Was this experimented on in the Paper?

The three domains it was tested on were Biomedicine, Finance and Law. A LLaMA-7B model was trained using different ratios of reading comprehension texts with general instructions. "General Instructions" is not defined in the paper, the idea is that it covers a wider range of "input-output types" which I take to mean questions that aren't a type of the above six categories. Perhaps exploring the codebase in the GitHub link the paper provides has the answers, but I've not found them there yet.

These models were pitted against bigger models which had been trained solely on raw domain-specific text, including the MedAlpaca-13B (for Biomedicine), BloombergGPT-50B (for finance) and LexGPT-6B to name a few. The table results show it can be competitive with all three, which is certainly very impressive against Bloomberg. Here is the table found in the paper.

![AdaptLLMs Performance Table](..\..\..\images\data_science\llm_diary\articles\Adapt_LLMs\AdaptLLMs_performance.png)

## A Limitation worth considering

This adaptation is fine, there's no mention of what the time complexity of their function is (though as it is using RegEx, its time and memory complexity won't be any better than linear time on a potentially huge body of text) and the use of general insturctions as well has the capacity to make a long training process even longer if using this for a Large Language Model. 

Additionally, unless your company specializes in developing/ is excessively rich then these models then you don't want to be in the business of training these models from scratch. Hence why RAG (which I don't fully understand but will will try my best to very shortly...) is a fruitful area of research being explored at the moment.

While interesting to read about, I would be somewhat surprised if these become a mainstream thing unless we get to a point where it is a lot cheaper to train your own LLMs, potentionally this is something people will do to design their own domain-specific chatbots.

**NB:** Retentive Networks could bring the price down by virtue of being faster to train than current architecture - but they lowered training time to be competitive with Transformers with Flash Attention (idk what this means). Maybe by using kernel fusion (as the RetNet paper says, again idk what this is) then the training time will be lowered significantly enough that the costs are feasible for more to develop their own. Given it can cost about $150,000 worth of cloud credits to fully train a model though, the decrease in cost would have to be huge.