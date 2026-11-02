# Vector Databases and QA: Retrieval Augmented Generation (RAG)

**Session Time:** 120 minutes

---

## Table of Contents

1. The Hallucination Problem and RAG Solution
2. Deep Dive: The Indexing Pipeline (Loading & Splitting)
3. Deep Dive: Embeddings and Vector Stores
4. Lab: "Ask the Knowledge Base" (End-to-End RAG)
5. Wrap-Up and Reflection
6. Resources

---

## Learning Objectives

By the end of this module, participants will be able to:

1. **Define** Retrieval Augmented Generation (RAG) and explain how it mitigates LLM hallucinations by grounding responses in external data.
2. **Apply** advanced text splitting strategies (like recursive character splitting with overlap) to prepare documents for semantic indexing.
3. **Construct** a Vector Database using standard embedding models to store and retrieve information based on cosine similarity.
4. **Build** a functional Question-Answer (QA) pipeline that retrieves relevant context before generating an answer.

---

## Session Breakdown

| Segment | Topic | Duration (minutes) |
| :--- | :--- | :--- |
| 1 | The Hallucination Problem & RAG Overview | 15 |
| 2 | Deep Dive: The Indexing Pipeline (Loading & Splitting) | 25 |
| 3 | Deep Dive: Vector Stores & Cosine Similarity | 25 |
| 4 | Lab: "Ask the Knowledge Base" | 45 |
| 5 | Wrap-Up and Reflection | 10 |

---

## 1. The Hallucination Problem & RAG Overview (15 min)

* **The Limitation of Pre-training:**
    * LLMs are frozen in time. They don't know about events after their training cutoff, and they definitely don't know your company's private internal documents.
    * When asked about these topics, they often *hallucinate* confidently incorrect answers.
* **RAG (Retrieval Augmented Generation):**
    * *Concept:* Instead of relying solely on the LLM's internal memory (weights), we first search an external database for relevant facts, then paste those facts into the prompt as context.
    * *Analogy:* Taking an open-book exam vs. a closed-book exam.
* **The RAG Architecture:**
    * **Indexing Phase:** Load Documents -> Split into Chunks -> Embed -> Store in Vector DB.
    * **Query Phase:** User Question -> Embed Question -> Semantic Search (Vector DB) -> Retrieve Top-K Chunks -> Pass Chunks + Question to LLM -> Generate Answer.

## 2. Deep Dive: The Indexing Pipeline (25 min)

Garbage in, garbage out. How we prepare data determines if the RAG system fails or succeeds.

* **Document Loading:**
    * Using standard loaders (e.g., LangChain's `PyPDFLoader`, `WebBaseLoader`, `TextLoader`) to unify diverse formats into standard text.
* **The Critical Importance of Splitting (Chunking):**
    * LLMs have finite context windows (cannot feed an entire 500-page manual at once).
    * **Semantic Relevance:** Smaller chunks often contain more concentrated semantic meaning than whole chapters.
* **Splitting Strategies:**
    * *Naive Splitting:* Breaking every 500 characters. (Bad: might split a sentence in half, destroying meaning).
    * *Recursive Character Splitting:* Tries to split by paragraph first, then by sentence, then by word. Preserves semantic structure.
    * *Chunk Overlap:* Crucial for maintaining context at the boundaries. If a key concept is split between chunk A and chunk B, overlap ensures it exists fully in at least one.

## 3. Deep Dive: Vector Stores & Cosine Similarity (25 min)

* **The "Database" of Meanings:**
    * Traditional DBs use keyword matches (finding exact words). Vector DBs use *semantic* matches (finding similar meanings).
* **Embeddings (Recap in Context):**
    * The embedding model (e.g., OpenAI `text-embedding-3-small`) converts a text chunk into a fixed-size vector of floating-point numbers (e.g., 1536 dimensions).
* **Cosine Similarity:**
    * How do we know if vector A (User Question) is relevant to vector B (Document Chunk)?
    * We measure the cosine of the angle between them.
    * *Score of 1.0:* Identical meaning.
    * *Score of 0.0:* Completely unrelated (orthogonal).
* **Vector Databases (The Engine):**
    * Specialized systems designed to perform this mathematical comparison across millions of vectors in milliseconds.
    * *Examples:* Pinecone (managed), Chroma (local/open-source), FAISS (Meta's library for efficient similarity search).

## 4. Lab: "Ask the Knowledge Base" (45 min)

**Objective:** Build a RAG system capable of answering questions about a fictional, newly released product manual that the base LLM knows nothing about.

**Refer: [Ask the Knowledge Base](https://github.com/ga-curriculum/lab-4-2-ask-the-knowledge-base)**


## 5. Wrap-Up and Reflection (10 min)

**Reflection Questions:**
1.  If the RAG system gives a wrong answer, where could the failure be? (It could be the retrieval failed to find the right doc, OR the LLM failed to read the retrieved doc correctly).

---

## 6. Resources

* [LangChain RAG Tutorial](https://python.langchain.com/docs/use_cases/question_answering/)
* [ChromaDB Documentation](https://docs.trychroma.com/) - *Easy to use local vector database.*
* [Pinecone Learning Center](https://www.pinecone.io/learn/) - *Excellent high-level concepts on vector search.*