# LLM Workshop - What could be key trends in Industry?

Recently, I attended a workshop with my team where we visited a big tech company for them to show off their GenAI tools for us. I thought it'd be worth documenting a few general thoughts here because I think there were hints there as to how the industry is moving with LLMs. 

## Open Source for this area is promising, but very young

Open Source for LLMs has blown up in the last year, however a lot of the most popular packages such as Langchain and Chroma are both still in very early versions, sporting a version 0.1.5 and a 0.4.22 at time of writing. Ragas, for evaluating, has only just launched 0.1.0.

With this, packages will have ideas and key features that get changed quite quickly. In the code demo, we did see some use of deprecated functions from these open source packages. 

LLM products are coming thick and fast, as with any hype cycle, but is this companies striking while the iron is hot, or undercooking the iron?

## What happens to data engineering?

Already, we have people saying that LLMs are changing the way we live as data professionals. This is, quite obviously, correct.

What leads to more speculation, however, is how long it may be until no one is writing any code at all. The idea of "citizen Data Scientist" and AI which writes code for us have caused some to believe we could not be coding within 5 years. However, I'm not so sure.

In my mind, one of the biggest barriers currently is the fact that we're in the same position now with text data as we were with tabular data around 10 years ago. Classically, you have the stereotype of the Data Scientist who spends all their time doing a Data Engineer's work due to the data they are working with being messy. 

Now, with natural language data becoming more and more important for companies wanted to harness the power of LLMs, companies need to clean previously messy text data instead. We've seen how much this can alter performance in pre-training with [AdaptLLMs](adaptLLMs.md) and even at [Retrieval Augmented Generation](RAG_survey_P1.md).


## Metrics and Explainability are huge issues

Traditional ML models have sound theoretical and practical ways of evaluating their performances. A lot of the methods for evaluating LLMs are either sound in theory, but very difficult to implement for a lot of companies (Reinforcement Learning with Human Feedback) or questionable in theory (LLM-as-a-Judge) or don't do well enough as of yet for production (again, LLM-as-a-Judge)

Explainability will always be a sticky one. As long as hype is around at the moment, who knows if this will be asked in board rooms quite as much.

## When will we see more LLM use cases in production?

We already have seen LLMs in production, but only really in low-stakes applications. You don't want newspaper headlines about how your chatbot was bigoted to a customer, gave them misleading advice due to hallucination or makes a key decision that mucks a lot of things up! 

OpenAI could get away with releasing Chat GPT in its initial state (1) because really their version of "production" is a rather large proof-of-concept and (2) because the tech industry is so void of regulation that it'd be impressively bad for them to do anything to upset the powers that be.

## Name Changes to rolls

"Data Engineer"? Don't be ridiculous! They're "AI Engineers" now! Anything to get the clients thinking you're the bees knees...