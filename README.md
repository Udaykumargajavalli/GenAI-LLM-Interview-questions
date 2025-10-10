# Can we trust one LLM to evaluate another LLM’s output?
Only with caution — and never blindly.

LLMs can simulate human judgment surprisingly well — they’re fast, cheap, and scalable. But here’s the catch: 1.They lack grounded understanding 2.They often reward fluency over factuality 3.They inherit the biases and blind spots of the model family they’re built from

If GPT-4 evaluates GPT-3.5, they might reinforce the same mistakes. Even worse: an eloquent hallucination can sometimes score higher than a boring truth.

That’s why top labs (OpenAI, Anthropic, DeepMind) combine model-based evaluation with human review, especially for tasks involving truthfulness, safety, or creativity. LLM-as-a-judge is useful — but it’s a co-pilot, not a referee.

---
# Let’s say you’re working on optimizing a large language model (like a decoder-only transformer). How would you redesign the attention mechanism to make inference faster and more memory-efficient — without hurting performance too much?
One practical way to improve efficiency is by using Grouped Query Attention (GQA).

In standard multi-head attention, each head has its own query, key, and value projections. That works well, but when you’re scaling to really large models, it gets expensive — especially at inference time.

With GQA, the idea is pretty simple: You keep each head’s query separate (so each can still look at the input differently), but you share keys and values across a smaller number of groups. For example, instead of 16 heads with 16 K/Vs, you might group them into 4 — and all heads in each group share the same K/V.

This cuts down on memory and speeds up inference, especially during decoding — where key/value caching is a bottleneck. And the best part? Models like Mistral and Gemma show that it works really well in practice. You get most of the performance benefits of multi-head attention, but with fewer computations.

---
# You inherit a RAG system. Users complain it's slow but accurate. How would you diagnose and improve it?

Systematic approach, measurement before optimization, understanding trade-offs

---
# Your model works great Monday-Friday but performs poor on weekends. How would you investigate?

Data distribution thinking, monitoring strategy, root cause analysis process

---

# You have $10K monthly budget for AI infrastructure. Design a recommendation system that scales.
 Cost awareness, build vs buy decisions, incremental deployment strategy

 ---
# A production model suddenly drops from 95% to 60% accuracy. Walk me through your investigation.
Check data pipeline first, not model → Look for upstream changes → Verify monitoring wasn't broken → Compare distributions, not just accuracy → Have rollback ready before investigating

---
# How would you build a system that summarizes customer support tickets in real-time?
Explain on these points → How do you handle different ticket formats? → What's your approach to quality control? → How do you measure if summaries are helpful? → What happens when the LLM service is down? → How would you gather feedback and improve?

---
# You can have fast, cheap, or accurate models. Pick two and explain why.

---
# Here's a Jupyter notebook that works. How would you productionize it?

Apply: → Error handling and logging → Configuration management → Testing strategy → Deployment approach → Monitoring plan → Documentation needs

---
# In RAG, after retrival how do you know whether those retrieved documents or paragraph of data is correct?
# When do you use Agents instead of LLM workflow?
# Difference between Traditional Rule based + NLU, Gen AI enhanced , Agentic Chatbots?
# Your RAG chatbot answers general questions well but fails on company-specific FAQs. What might be wrong with your retrieval setup?
# In RAG, if the retriever returns too many irrelevant chunks, how does it affect the final LLM answer? How would you fix it?
# You fine-tuned an LLM on domain data, but a simple RAG pipeline performs better. Why might RAG outperform fine-tuning in this case?
# What trade-off do you face between using smaller context chunks vs larger chunks in RAG?
# Your RAG system answers correctly in dev but fails in prod when documents are updated frequently. How would you maintain freshness of retrieval?
# What does tokenization really mean, beyond just “splitting text”?
# Why are decoder-only models dominant today? When would encoder-decoder be a smarter choice?
# Explain attention in plain English — where exactly do queries, keys, and values come into play?
# Compare sampling methods: top-K, top-P, and temperature. When would you choose each one?
# Share an example where a zero-shot prompt failed. How did you fix it?
# How do you track versions of prompts and test them for consistency?
# What is “context window waste” and how would you reduce it?
# LoRA vs QLoRA vs PEFT — how do you balance memory, speed, and accuracy?
# What are the biggest limitations of RLHF? Give one real failure case.
# When would you pick an open-source model over a proprietary one for fine-tuning?
# Walk me through how embeddings and similarity search actually work.
# Pinecone vs Chroma vs Weaviate — which one would you choose and why?
# When does hybrid retrieval (vector + keyword) outperform pure vector search?
# Define an AI agent without using buzzwords. 
# How do you handle tool-use errors inside an agent loop?
# What’s the toughest part about coordinating multiple agents?
# How would you detect and monitor hallucinations in production? 
# Which metrics matter most in an evaluation pipeline?
# Walk me through deploying a GenAI service with FastAPI + Docker + K8s — how do you ensure rollback safety?
# If your system blows the budget because of model size, how do you decide on trade-offs — speed, accuracy, or cost?
# What is MCP, and why does it matter for interoperability across GenAI tools?
# How would you design a safe fallback if an MCP-enabled tool fails mid-workflow?
# How do encoders and decoders really work?
# Can you implement byte pair encoding to tokenize text data?
# Can you build an ML pipeline with just NumPy and Pandas, no pre-built libraries?
# Your retriever returns high-similarity junk—how do you improve contextual precision? What do you do?
In Retrieval-Augmented Generation (RAG), one of the most common failure modes is this: The retriever confidently returns passages that look similar to the query but are contextually irrelevant.

Example: 🔹 Query → “How do I enable two-factor authentication in Gmail?” 🔹 Retriever → returns docs about “multi-factor auth in Microsoft Teams.” High overlap, wrong context.

This is a precision problem: lots of near-matches, few truly useful hits.

Here’s how strong teams improve contextual precision:

- Better chunking strategies → Ensure documents are split semantically, not arbitrarily.
- Query rewriting / expansion (HyDE, query routing) → Make queries more precise before retrieval.
- Use rerankers → Add a lightweight ranking step that scores passages by contextual relevance, not just embedding similarity.
- Task-specific embeddings → Fine-tune embeddings on your domain instead of relying on generic off-the-shelf vectors.

---
# Our team needs to build RAG over 10 million documents. Which vector database would you recommend and, crucially, why? and provide the trade offs.
