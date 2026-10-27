# RAGGauge — RAG Evaluation with LLM-as-a-Judge

**RAGGauge** is a Retrieval-Augmented Generation (RAG) evaluation project focused on measuring the quality and reliability of generated answers. It combines **LLM-as-a-Judge evaluation with LangSmith experiment tracking** to assess how well a RAG pipeline retrieves relevant information and generates context-grounded responses.

🔗 **GitHub Repository:** [Evaluation-of-Rag](https://github.com/jobayertalha/Evaluation-of-Rag)

The central idea is simple: **a RAG application should be evaluated not only by the answers it generates, but also by the quality of its retrieval and the evidence supporting those answers.**

## 🎯 Project Objectives

- Evaluate RAG outputs across multiple quality dimensions.
- Use an LLM as a judge to assess answer quality.
- Measure whether generated responses are supported by retrieved context.
- Track evaluation experiments and inspect results using LangSmith.
- Identify weaknesses in retrieval and generation instead of relying only on manual testing.

## ✨ Key Features

- **RAG Pipeline:** Retrieves information from a Markdown-based business knowledge base and generates context-aware answers.
- **Semantic Retrieval:** Uses Hugging Face embeddings and FAISS for vector-based document retrieval.
- **LLM-as-a-Judge:** Evaluates generated responses against defined quality criteria.
- **Multi-Dimensional Evaluation:** Assesses correctness, groundedness, answer relevance, and retrieval relevance.
- **LangSmith Integration:** Organizes evaluation runs, tracks experiments, and provides a place to inspect traces and evaluation results.
- **Notebook-Based Evaluation:** Provides a notebook workflow for running and understanding the evaluation process.

## 🏗️ Architecture

```text
             Business Knowledge Base
                       |
                       v
             Document Processing
                       |
                       v
           Hugging Face Embeddings
                       |
                       v
                 FAISS Index
                       |
User Question -------> Retrieval
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
     Correctness   Groundedness   Relevance
                       |
                       v
              LangSmith Evaluation
                       |
                       v
            Experiments and Results
```

*Retrieval relevance is evaluated separately to assess the quality of the retrieved evidence.*

## 🔬 Evaluation Methodology

RAGGauge evaluates the pipeline across four dimensions:

| Evaluation Metric | What It Measures |
|---|---|
| **Correctness** | Whether the generated answer is consistent with the reference answer. |
| **Groundedness** | Whether the answer is supported by the retrieved context. |
| **Answer Relevance** | Whether the response directly addresses the user's question. |
| **Retrieval Relevance** | Whether the retrieved document chunks contain information relevant to the question. |

These dimensions help distinguish different failure modes. For example, a response may be relevant to a question but still contain unsupported information, or the answer generator may perform poorly because the retriever did not find the necessary evidence.

## 📊 Experiment Tracking with LangSmith

**LangSmith is a key part of this project—not just a logging tool, but the workspace for inspecting RAG evaluation experiments.**

The integration supports running evaluations against a LangSmith dataset and examining experiment results and traces.

With LangSmith, you can investigate:

- **Experiment results:** Inspect the outcomes of evaluation runs.
- **Evaluation scores:** Review the available evaluator outputs for individual examples.
- **Execution traces:** Follow the pipeline to understand how questions, retrieved context, and generated answers flow through the system.
- **Failure analysis:** Investigate cases where an answer is incorrect, insufficiently grounded, or poorly supported by retrieved documents.
- **Experiment comparison:** Compare runs when multiple experiments have been executed and their results are available.

This makes it easier to move beyond testing a few questions manually and systematically investigate the strengths and weaknesses of a RAG pipeline.

> **Note:** LangSmith displays the results produced by the evaluation runs; it does not automatically prove that the system is reliable. Meaningful conclusions depend on the dataset, evaluator quality, and actual experimental results.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core implementation |
| LangChain | RAG pipeline components |
| Hugging Face | Text embeddings |
| FAISS | Vector indexing and semantic retrieval |
| Groq | Hosted language model for answer generation |
| LangSmith | Evaluation datasets, experiment tracking, and trace inspection |
| Jupyter Notebook | Interactive evaluation workflow |
| Pytest | Automated testing |

## 📚 Knowledge Base

The project uses Markdown documents to provide business-related information, including:

- Company overview
- Product catalog
- Frequently asked questions
- Delivery policies
- Warranty and return policies

The knowledge base provides the source material used for retrieval and grounded answer generation.

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/jobayertalha/Evaluation-of-Rag.git
cd Evaluation-of-Rag
```

### 2. Create a virtual environment

**Windows PowerShell**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux/macOS**

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=openai/gpt-oss-20b
LANGSMITH_API_KEY=your_langsmith_api_key
```

Configure any additional LangSmith tracing or project settings required by the implementation.

**Security:** Never commit your `.env` file or expose API keys in notebooks, screenshots, or public repositories.

### 5. Run the RAG application

```bash
python main.py
```

### 6. Run the evaluation workflow

Open the evaluation notebook in Jupyter or your preferred notebook environment and execute its cells in order.

The workflow is intended to prepare the RAG pipeline, create or reuse an evaluation dataset, run the evaluators, and send the results to LangSmith for inspection.

Follow the repository's current code and configuration if filenames differ from those shown here.

## 📈 Understanding the Results

When reviewing a LangSmith experiment, consider the evaluation dimensions together:

- **Low correctness:** The answer may not match the reference.
- **Low groundedness:** The answer may contain claims unsupported by the retrieved context.
- **Low answer relevance:** The response may not adequately address the question.
- **Low retrieval relevance:** The retriever may be returning irrelevant or insufficient evidence.

Use individual examples and traces to investigate why a result occurred rather than relying on a single aggregate score.

*Evaluation scores and successful experiment completion should be reported only after verifying the corresponding LangSmith runs.*

## 🔮 Potential Improvements

- Expand the evaluation dataset to cover more questions and edge cases.
- Add explicit tests for unanswerable questions and missing evidence.
- Compare retrieval configurations and chunking strategies.
- Evaluate different embedding models and generation models.
- Improve evaluator consistency and validate LLM-as-a-Judge decisions against human judgments.
- Add regression evaluations to detect quality changes between pipeline versions.

## 💡 Key Takeaway

RAG evaluation requires more than checking whether an answer sounds correct. It requires examining retrieval quality, answer correctness, and whether generated claims are supported by evidence.

**RAGGauge brings these evaluation dimensions together with LangSmith experiment tracking to make RAG behavior easier to inspect, analyze, and improve.**

## 👨‍💻 Author

**Talha Jobayer Zihan**

- GitHub: [@jobayertalha](https://github.com/jobayertalha)
- LinkedIn: [talha-jobayer](https://linkedin.com/in/talha-jobayer)

---

*Built to explore systematic evaluation of Retrieval-Augmented Generation systems using LLM-as-a-Judge and LangSmith.*
