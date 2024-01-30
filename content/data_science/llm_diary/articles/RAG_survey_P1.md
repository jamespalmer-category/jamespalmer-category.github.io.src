# Retrieval-Augmented Generation for Large Language Models: A Survey

[The original paper](https://arxiv.org/pdf/2312.10997.pdf)

In 2020, while completing his PhD for UCL (who wouldn't give me funding...) and working for meta, Patrick Lewis [wrote a paper](https://arxiv.org/pdf/2005.11401.pdf) introducing the idea of Retrieval-Augmented Generation. Lewis hit the jackpot by writing a paper that, 4 years later, formed the backbone of what is now a must-know technique for anyone who works with LLMs.

Of course, LLMs weren't known about much to people outside of the NLP research world at the time, however now that more and more companies are seeing use cases for LLMs and Generative AI crop up, RAG is a very popular method of querying an LLM. Let me illustrate why before we get onto the survey given by the paper.

## Why RAG is Important

Detailed at the end of [yesterday's](adaptLLMs.md) entry, training/fine-tuning LLMs is something that takes a long time. For companies that are solely tech companies, you need to have a lot of money in your pocket ($150K+!) to train anything state of the art (this is talking the very highest end of LLMs, 7B parameter models are cheaper). Additionally, bigger companies might not have the infrastructure in place to train their own language model on their own specialised domain.

Instead of training your own LLM, what if instead you did the following:

* Built an external knowledge base of texts related to your company's specialist field
* Have a pre-trained LLM of your choice (e.g. GPT, LLaMA, PaLM)
* When your chatbot is queried, have it retrieve relevant documents from your knowledgebase to give it additional context to answering the query (a bit of prompt engineering is used here)
* Send the output to the user based on their query plus the added context from the knowledgebase!

![Example of What a RAG pipeline looks like, from the linked paper](..\..\..\images\data_science\llm_diary\articles\RAG_survey_P1\RAG_example.png)

On top of being a more cost-efficient method of improving how a domain-specific LLM performs, it also comes with the advantage that you can engineer your pipeline so that the LLM only gets interacted with by your customer during the RAG process, while you let the rest of the customer-facing side be handled by a manual chatbot. Something [DPD may've appreciated](https://www.bbc.co.uk/news/technology-68025677).

## So that's the high-level, what is going on under the hood?

The corpus within RAG is split into chunks. Depending on what your knowledge base is and how it is split, this can happen quite naturally. E.g. an essay can be split into paragraphs and if needs be you can split that further. Work has been done on preprocessing PDFs, HTML, Word and Markdown files into plain text so that we can then encode these chunks with a RAG model into a vector space (e.g. encode them and then store them in a VectorDB like [Chroma DB](https://www.trychroma.com/) provides). 

At retrieval time, your RAG model will then find the top K chunks closest to the query itself and put those into a prompt template which will mix with the original prompt to make a new one, with both query and context. The model finally generates a response based on this framework. Additionally, the chat history could also be added to the prompt.

It is worth noting that this is what the paper describes as the "naive RAG" paradigm. It is a great starting point, but it has its issues. These include a lot of the usual issues of hallucinations, as well as issues associated with the precision/recall of the RAG model itself. A rather nuanced flaw is that you could end up with the LLM providing an over-reliance on the encoded database, meaning we gain no value from the LLM itself.

## Advanced RAG

The second paradigm is Advanced RAG, where we iron out the issues listed above. Part of what the paper lists as entailing this seems obvious, though nonetheless important, stuff. This includes making sure the database is well-maintained and well pre-processed. Additionally, you could incorperate a graph sturcture to capture context or encode metadata such as chapter and date.

When retrieving, you could have dynamic embeddings (the embedding for one word changes on context) or you could fine-tune your embeddings model, even by going as far as to make your own custom embedding model.

Post-retrieval, you can re-rank the chunks based on document diversity, re-calculated semantic similarity, or use "prompt compression" whereby you compress the prompt based on perplexity/ mutual information to reduce noise being fed to the model.

## Modular RAG

The idea here is to "modularise" the RAG process, this is currently the most popular paradigm as it puts the developer in control and lets them tailor the RAG approach to their task/ requirements. The paper suggests that Advanced RAG is even a form of modular RAG. There are far too many modules to list them all here (this entry is already too long), so I really would checking out the original paper to read these (section 3.3). There are also a few in there I don't fully understand, such as the idea of the "predict" module. With that in mind, I wanted to pick out some interesting ones:

* **Search Module**: A tailored search to specific scenarios, it could even be using SQL generated by the LLM
* **Routing**: Have a number of actions, with search being one, based on a user's input

Additionally, the modular paradigm has inspired more paradigms (specifically trying different module architectures as well as exploring different ways to make the steps interact with each other). Additionally, considerations for optimizing the rag pipeline round out section 3 of the paper.

![Different RAG architectures](..\..\..\images\data_science\llm_diary\articles\RAG_survey_P1\different_RAG_architectures.png)