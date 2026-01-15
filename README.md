

# 🌋 Advanced Retrieval-Augmented Generation (RAG) Pipeline  
### `Using Volcano wikipedia page`

This repository demonstrates a production-grade **Retrieval-Augmented Generation (RAG)** pipeline designed for high-precision information extraction. While this implementation uses Wikipedia's "Volcano" article as a test case, the architecture is **domain-agnostic** and built to handle any complex unstructured text.

Unlike basic vector search demos, this system incorporates **semantic data cleaning, metadata-aware chunking, and second-stage reranking** to significantly improve answer precision and reduce hallucinations.

---

## 🧠 System Architecture

```python
Wikipedia Page
↓
Targeted HTML Parsing (BeautifulSoup)
↓
Noise Removal & Text Normalization
↓
Semantic Chunking + Metadata
↓
Dense Vector Embeddings (MiniLM)
↓
FAISS Vector Search (Top-K)
↓
Cross-Encoder Reranking (FlashRank)
↓
LLM Answer Generation (GPT-4o)
```

---

## 🎯 Problems This Project Solves

### 1. Noisy Data Ingestion
Raw HTML scraping often introduces:
- Citation markers (`[1]`, `[24]`)
- Edit tags
- Navigation and layout artifacts

These tokens pollute embedding space and degrade retrieval quality.

**Solution:**  
```python
Targeted DOM parsing + regex-based lexical cleaning before embedding.
```

---

### 2. Context Fragmentation
Naive fixed-size chunking can split ideas mid-sentence.

**Solution:**  
`RecursiveCharacterTextSplitter` with overlap preserves semantic continuity while remaining token-efficient.

---

### 3. Low-Precision Top-K Retrieval
Cosine similarity alone does not guarantee that retrieved chunks *actually answer the question*.

**Solution:**  
A **two-stage retrieval pipeline**:
- FAISS for recall
- FlashRank (cross-encoder) for precision

---

## 🛠️ Technical Stack

| Component | Technology |
|--------|-----------|
| Data Source | Wikipedia |
| HTML Parsing | BeautifulSoup + SoupStrainer |
| Chunking | LangChain RecursiveCharacterTextSplitter |
| Embeddings | `all-MiniLM-L6-v2` |
| Vector Store | FAISS |
| Reranking | FlashRank |
| LLM | OpenAI GPT-4o |
| Framework | LangChain (modern v0.3 style) |

---

## 🚀 How to Run

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/Sohan-bhatt/Reranked-Context-Pipeline.git
cd volcano-rag
````

### 2️⃣ Create Environment & Install Dependencies

```bash
pip install -r requirements.txt
```

### 3️⃣ Configure Environment Variables

Create a `.env` file:

```env
OPENAI_API_KEY=your_api_key_here
```


---

## 🔍 Example Query

```python
"What are the long-term environmental impacts of volcanic eruptions?"
```



## 📌 Key Takeaway

This project demonstrates how **careful preprocessing, retrieval design, and reranking** can dramatically outperform naïve RAG systems — even when using the same LLM.

It is designed as a **scalable foundation** for real-world research assistants.


