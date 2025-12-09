**Self-Correcting RAG Pipeline**



This project implements a fully local, self-correcting Retrieval-Augmented Generation (RAG) pipeline designed to reduce hallucinations using automatic factual verification.



The system retrieves relevant news articles from the CC-News dataset, generates an answer using a local LLM, verifies each claim using an MNLI-based fact-checker, and rejects answers that fail validation. It also enforces answer-type correctness and year consistency between the question and the documents.



This project demonstrates a practical solution to one of the biggest real-world problems in RAG systems: hallucinated answers that appear fluent but are factually incorrect.





🔧 How It Works (Pipeline Flow)



1\. **Ingestion**

&nbsp;  - News articles are loaded from the CC-News dataset.

&nbsp;  - Articles are chunked and stored with metadata.



2\. **Indexing**

&nbsp;  - Text is converted into embeddings using SentenceTransformers.

&nbsp;  - FAISS is used for fast similarity search.



3\. **Retrieval**

&nbsp;  - For each query, the most relevant document chunks are retrieved.



4\. **Relevance + Date Filtering**

&nbsp;  - Low-relevance documents are removed.

&nbsp;  - If the question contains a year, only articles from that year are allowed.



5\. **Answer Generation**

&nbsp;  - A fully local LLM (Flan-T5) generates answers using only retrieved documents.



6\. **Fact Checking (MNLI + Extractive QA)**

&nbsp;  - The answer is split into claims.

&nbsp;  - Each claim is verified using:

&nbsp;    - Extractive QA (span detection)

&nbsp;    - MNLI entailment scoring

&nbsp;  - A support score is computed.



7\. **Hard Safety Validation**

&nbsp;  The answer is REJECTED if:

&nbsp;  - The answer type does not match the question intent.

&nbsp;  - The document year does not match the question year.

&nbsp;  - The supported fact ratio is below threshold.



8\. **Final Output**

&nbsp;  - Outputs are labeled as:

&nbsp;    - ✅ “RAG (Verified)”

&nbsp;    - ❌ “RAG-Rejected”

&nbsp;  - Full claim-level evidence is displayed.





 ✅ Key Features



\- Fully **Open-Source \& Local\** (No OpenAI or paid APIs)

\- **Self-Correcting Fact-Checking Layer\**

\- \*\*Answer-Type Enforcement\*\*

\- \*\*Strict Year Validation\*\*

\- \*\*Claim-Level Explainability\*\*

\- \*\*Built for Real News Articles\*\*

\- \*\*Designed for Hallucination Detection Research\*\*



\## ⚠️ Current Limitations



\- CC-News contains mostly 2017–2018 data (no live 2024 news)

\- Local models are slower than APIs

\- Long documents are chunked heuristically

\- Some fact-check false positives remain for short answers





