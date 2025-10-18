1. Design Trade-Offs
The system is currently designed for maximum efficiency and low cost (high speed, low memory) at the expense of potential maximum accuracy.

Embedding Model: all-MiniLM-L6-v2 is chosen for its speed and small memory footprint (384 dimensions), which is excellent for prototyping and cost-constrained deployment. The trade-off is lower semantic accuracy compared to larger models.

LLM Generator: google/flan-t5-small is highly cost-effective and fast (can run on CPU), but its reasoning and instruction-following abilities are limited compared to larger, modern LLMs.

Vector Index: FAISS.IndexFlatIP provides guaranteed accuracy for similarity search but is an in-memory bottleneck that does not scale well beyond small datasets.









2. Retrieval Strategy
The core strategy is Simple Dense Retrieval, but it requires critical adjustment to the chunking process.

A. Chunking Strategy
Current Issue: The chunk size of 5000 words is too large, causing severe text truncation and poor embedding quality by the MiniLM model.

Correction: Reduce the size to 400-600 words with a 50-100 word overlap to ensure semantic coherence and prevent embedding model input limits from being exceeded.

B. Retrieval Method
Core Method: Pure Dense Vector Search (Cosine Similarity on normalized embeddings).

Enhancement: To improve recall for keyword-specific queries (names, codes), the system should implement Hybrid Search (combining Vector Search with BM25 or another lexical search).

Precision Improvement: Introduce a Cross-Encoder Reranker after retrieval to re-score the top-k chunks and select the most relevant subset for the final prompt.






3. Guardrails & Failure Modes
Robust measures are needed to ensure the output is safe and factually grounded.

Hallucination Prevention: The LLM prompt must strictly command source citation ([source - page]). A post-generation check should verify the presence and correct formatting of these citations.

No Answer Fallback: Implement a confidence threshold (e.g., max retrieval score <0.6). If confidence is low, the LLM must be explicitly prompted to respond with a message like: "I cannot answer based on the provided context."

Sensitive Queries: Add an Input Filtering step before retrieval using a keyword list or a dedicated text classification model to block or sanitize unsafe, toxic, or policy-violating queries.

Monitoring Metrics: Track key performance indicators (KPIs) including:

Retrieval Quality: Precision@k and Recall@k.

Generation Quality: Faithfulness Score (Are claims supported by context?) and Response Relevance.

Performance: End-to-End Latency.





4. Scalability Considerations
The system must be prepared to handle increased data volume and user load efficiently.

Scaling Data (10x Documents):

Solution: Migrate the index from in-memory FAISS.IndexFlatIP to an Approximate Nearest Neighbors (ANN) index like IndexHNSW or transition to a managed Vector Database (e.g., Pinecone, Milvus) for horizontal scaling and storage efficiency.

Scaling Users (100+ Concurrent Queries):

Solution: The LLM is the bottleneck. Deploy the LLM and Embedding models on dedicated, high-throughput cloud services. Utilize Dynamic Batching to process multiple user queries simultaneously.

Cost-Efficient Cloud Deployment:

Strategy: Maintain the small model sizes (MiniLM, Flan-T5) for CPU-based serving. For higher load, use Quantization (int8 or int4) on the models to maximize the number of concurrent users served per GPU/CPU instance, reducing total infrastructure cost.