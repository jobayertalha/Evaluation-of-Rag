# RAGGauge — RAG Evaluation with LLM-as-a-Judge

**A systematic approach to evaluating Retrieval-Augmented Generation beyond answer generation.**

RAGGauge is a Retrieval-Augmented Generation (RAG) evaluation project designed to assess the quality and reliability of a business knowledge-base question-answering system. It combines semantic retrieval, context-grounded answer generation, LLM-as-a-Judge evaluation, and experiment tracking in a single workflow.

Rather than evaluating a RAG system solely by its generated responses, RAGGauge examines both the retrieved evidence and the quality of the final answer.

## 🎯 Project Objectives

- Build a RAG pipeline for business knowledge-base question answering.
- Retrieve relevant information using semantic search.
- Generate context-grounded responses with a large language model.
- Evaluate retrieval and generation using LLM-as-a-Judge.
- Track experiments and inspect evaluation results using LangSmith.

## ✨ Key Features

- **Semantic Retrieval:** Uses Hugging Face embeddings and FAISS for similarity-based document retrieval.
- **Context-Grounded Generation:** Uses a Groq-hosted LLM to answer questions using retrieved business information.
- **LLM-as-a-Judge:** Evaluates responses across four complementary quality dimensions.
- **Evaluation Dataset:** Supports reusable datasets for repeatable evaluation.
- **Experiment Tracking:** Integrates LangSmith to run evaluations and inspect traces and results.
- **Knowledge-Base Question Answering:** Works with structured collections of business documents in Markdown format.

## 🏗️ System Architecture

```text
             Business Knowledge Base
                       |
                       v
              Document Chunking
                       |
                       v
             Hugging Face Embeddings
                       |
                       v
                  FAISS Index
                       |
                       v
                User Question
                       |
                       v
                Semantic Search
                       |
                       v
               Retrieved Context
                       |
                       v
               Groq-hosted LLM
                       |
                       v
                Generated Answer
                       |
                       v
               LLM-as-a-Judge
                       |
          +------------+------------+
          |            |            |
          v            v            v
      Correctness  Groundedness  Relevance
                       |
                       v
               LangSmith Tracking
```

*Note: The evaluation pipeline also includes a separate retrieval-relevance evaluator.*

## 🔄 How It Works

### 1. Knowledge-Base Preparation

The system loads Markdown documents containing business information, including company details, product catalogs, orders, payments, refunds, and service policies.

### 2. Document Chunking and Embedding

The documents are divided into smaller chunks. Hugging Face embeddings convert these text chunks into vector representations that capture semantic information.

### 3. Vector Indexing and Retrieval

FAISS indexes the document vectors and retrieves the most relevant chunks for a user's question using similarity search.

### 4. Context-Grounded Answer Generation

The retrieved context and user question are passed to a Groq-hosted LLM. The model generates an answer based on the available evidence, with instructions to avoid inventing unsupported business information.

### 5. LLM-as-a-Judge Evaluation

The generated response and retrieved context are evaluated using four criteria. This helps identify whether an issue originates from the retrieval stage or the generation stage.

### 6. Experiment Tracking

LangSmith is used to organize evaluation datasets, execute evaluation runs, and inspect traces and results.

## 🧪 Evaluation Methodology

RAGGauge uses four LLM-as-a-Judge evaluators.

| Evaluation Metric | Purpose |
|---|---|
| **Correctness** | Measures whether the generated answer is consistent with the reference answer. |
| **Groundedness** | Checks whether the answer's claims are supported by the retrieved context. |
| **Answer Relevance** | Evaluates whether the response directly addresses the user's question. |
| **Retrieval Relevance** | Evaluates whether the retrieved documents provide useful information for answering the question. |

These metrics assess different aspects of RAG performance. A response can be relevant to a question but still contain unsupported claims. Similarly, a fluent answer may be generated from irrelevant retrieved documents.

Using complementary evaluation criteria provides a more informative view of system quality than inspecting generated answers alone.

> **Important:** LLM-as-a-Judge scores are automated assessments, not definitive proof of factual correctness. Their reliability depends on the evaluation criteria, reference answers, context, and judge model.

## 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| LLM Provider | Groq |
| LLM Framework | LangChain |
| Embedding Model | Hugging Face Sentence Transformers |
| Vector Store | FAISS |
| Evaluation Framework | LangSmith |
| Development Environment | Jupyter Notebook |

## 📁 Knowledge-Base Coverage

The business knowledge base contains Markdown documents covering areas such as:

- Company overview
- Product catalog
- Orders and payments
- Refunds and cancellations
- Service delivery and support
- Subscription and business policies

These documents provide the reference context used for retrieval and answer generation.

## 🚀 Getting Started

### Prerequisites

- Python 3.12 or a compatible Python version
- A Groq API key
- A LangSmith API key for hosted tracing and evaluation tracking
- The project's knowledge-base files

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd rag-gauge
```

Replace `<your-repository-url>` with your actual repository URL.

### 2. Create a Virtual Environment

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Ensure the requirements file includes the packages needed by the notebook, including LangChain, Hugging Face embeddings, FAISS, Groq integration, LangSmith, and environment-variable management.

### 4. Configure Environment Variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=openai/gpt-oss-20b
LANGSMITH_API_KEY=your_langsmith_api_key
```

Use valid credentials and a model available to your Groq account. The model shown is an example configuration.

**Security:** Never commit `.env` or expose API keys in source code, screenshots, or public repositories. Add `.env` to `.gitignore`.

### 5. Run the Evaluation Notebook

Open `BusinessMind_RAG_Evaluation_Simple.ipynb` in Jupyter or a compatible editor.

1. Select the project's Python environment.
2. Run the notebook cells sequentially.
3. Confirm that the knowledge base loads and the RAG pipeline generates a response.
4. Verify that the evaluation dataset is available.
5. Run the LLM-as-a-Judge evaluators.
6. Inspect the resulting evaluation run in LangSmith.

The notebook filename can be changed to `RAGGauge_Evaluation.ipynb` to match the project name.

## 📊 Understanding the Results

Evaluation results should be interpreted across all four metrics rather than relying on a single score.

- **Low correctness:** Review the generated answer against the reference answer.
- **Low groundedness:** Investigate unsupported claims or insufficient retrieved context.
- **Low answer relevance:** Examine whether the response addresses the user's actual question.
- **Low retrieval relevance:** Inspect the retrieved chunks, embedding model, and search configuration.

These observations can guide targeted improvements to retrieval, prompting, and generation.

Actual performance should be reported only after the evaluation run has completed successfully and the results have been verified.

## 🔮 Future Improvements

- Expand the evaluation dataset with diverse business questions and reference answers.
- Add retrieval metrics such as Recall@K, Mean Reciprocal Rank (MRR), and Precision@K.
- Compare embedding models and retrieval configurations.
- Evaluate different LLMs and prompting strategies.
- Analyze failure cases and improve the pipeline iteratively.
- Add regression testing to detect quality changes between versions.

## 💡 Key Takeaway

**A RAG system should be evaluated not only on what it answers, but also on the evidence it retrieves and the reliability of the answer it generates.**

RAGGauge explores this principle through a practical workflow that combines semantic retrieval, grounded generation, LLM-as-a-Judge, and experiment tracking.

## 👨‍💻 Author

**Talha Jobayer Zihan**  
CSE Graduate | AI/ML Engineer

- LinkedIn: [talha-jobayer](https://www.linkedin.com/in/talha-jobayer)
- GitHub: [jobayertalha](https://github.com/jobayertalha)

---

*Built to explore systematic evaluation of Retrieval-Augmented Generation systems.*
