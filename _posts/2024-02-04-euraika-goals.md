---
layout: post
title:  "eurAIka goals"
date:   2024-02-07
categories: eurAIka update
permalink: /2024-02-04-euraika-goals/
---

One of the main goals of eurAIka is to help with generating code. We face two problems with scientific coding. The first is that we're not software engineers and we tend to hack  something together that works even if it isn't well written or efficient. 

The second problem is that not every software tool is applicable to every problem. Julia might be your preferred language for most problems, and you might be very good at coding in Julia, but there will be times when Mathematica or some specialized tool is more appropriate.

Rather than spending time on the research you want to do, you are forced to divert attention to developing an understanding of the software needed to solve your problem. Is it possible with the recent developments in LLMs and related systems that much of the science-to-software issues could be automated? 

Scientists won't want to cede complete control of code generation to an automated tool, but just as we can now query ChatGPT, Bard or Claude, the same methods might be achievable for code development. A natural language prompt from the user could initiate chain-of-reasoning follow-up questions to converge on an overall architecture of the desired code. 

A database of key words and general syntax structures could be developed from the manual, examples and  online user forums. A disadvantage of large language models is that it's difficult to verify correctness of the response. With code, if it doesn't run we know there's a problem. A desirable feature would be to have The Coder access the API, run the code and understand and respond to error messages. Borrowing from control theory, we want a closed loop system.

How might this work? Interacting with a large language model seems like a logical place to start given the LLMs ability to converse in natural languages. But we'll need more than simply an LLM to build a universal communicator with other computer languages. Some possibilities are:

- RAG/Fusion - [Retrieval Augmented Generation](https://www.pinecone.io/blog/rag-study/?utm_medium=email&_hsmi=291465816&utm_content=291465816&utm_source=hs_email) (RAG) creates a specialized database of knowledge for the LLM to access with potential significant improvements in accuracy. [RAG/Fusion](RAG/Fusion) extends this concept by generating multiple results and combining the output through quality ranking.
- [Neuro-symbolic AI](https://www.turing.ac.uk/research/interest-groups/neuro-symbolic-ai) - links low-level perception to high-level reasoning with goals of transparency and explainability.
- [Neural databases](https://www.marktechpost.com/2021/08/26/facebook-ai-introduces-neural-databases-a-new-approach-which-enables-machines-to-search-unstructured-data-and-connect-the-fields-of-databases-and-nlp/) - uses [machine learning](https://medium.com/thirdai-blog/neural-database-next-generation-context-retrieval-system-for-building-specialized-ai-agents-with-861ffa0516e7) to reduce the cost of maintaining large databases.
- [Large Action Models](https://medium.com/version-1/the-rise-of-large-action-models-lams-how-ai-can-understand-and-execute-human-intentions-f59c8e78bc09) - combine multiple agents to achieve goals. An example is [LangGraph](https://python.langchain.com/docs/langgraph) for building multi-actor systems on top of large language models.
- [RL/LLM]([RL/LLM](https://huggingface.co/learn/deep-rl-course/en/unitbonus3/language-models)) - Reinforcement Learning (RL) is a machine learning method that is goal-directed. The system tries to solve the given problem and then gets a score based on how closely the solution comes to the desired outcome. Combining RL with LLMs gives the method gives this technique a head start by eliminating most of the clearly impractical solutions.
- [Graph-based prompting and reasoning](https://cameronrwolfe.substack.com/p/graph-based-prompting-and-reasoning) - combines multiple chains of thought and uses non-linear reasoning similar to human thought processes.
- A method to [connect LLMs](https://gorilla.cs.berkeley.edu/) to an array of tools via APIs

LLMs are capable of generating code as Alessio Buscemi has shown in his paper, [*A Comparative Study of Code Generation using ChatGPT 3.5 across 10 Programming Languages*](https://arxiv.org/abs/2308.04477) where he looked at the quality of code generated for 10 commonly used languages. With a large corpus of available training data, the generated code was reasonably good for Julia and Python, but quality suffered for some other languages. LLMs may have difficulty with special purpose languages due to the sparsity of online examples.

Our aim with The Coder in eurAIka is to create a system that will ingest manuals, training information and examples to create a searchable database for each selected language. The user will prompt the system with the output goal of the code to be written, and the language to use, initiating a procedure for The Coder to follow. Reinforcement learning with feedback from the user ([RLHF](https://huggingface.co/blog/rlhf)) could help converge to the desired result.

How to go about this, or if it's even possible, isn't clear at this point, but I'm sure ChatGPT, Bard and Claude can help me figure it out!

