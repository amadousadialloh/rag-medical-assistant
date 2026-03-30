# 🏥 Medical RAG Assistant

A **Retrieval-Augmented Generation (RAG)** system that answers clinical questions grounded in verified medical documentation — the Merck Manual. Built to demonstrate the measurable accuracy gains of RAG over standard LLM responses.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://drive.google.com/file/d/1Hy0vdsI-Mfz89Q_Ytc4qynA8GdnXbNFl/view?usp=sharing)

---

## The Problem

General-purpose LLMs answer medical questions with confident but unverifiable responses — a serious risk in clinical contexts. This project tests three approaches to the same questions and measures the difference.

---

## Three-Stage Approach

| Stage | Method | Grounded in Source? | Hallucination Risk |
|---|---|---|---|
| 1 | Baseline LLM | ❌ Training knowledge only | High |
| 2 | Prompt Engineering | ❌ Training knowledge, better format | Medium |
| 3 | RAG Pipeline | ✅ Merck Manual PDFs | Low |

**Example — Question:** *"What is the protocol for managing sepsis in a critical care unit?"*

- **Baseline LLM:** General answer, no specific dosages, cannot verify source
- **Prompt Engineering:** Better structured, medically formatted, still training-based
- **RAG:** Returns specific, verifiable answers — e.g., *"Gentamicin or tobramycin 5.1 mg/kg IV once daily"* — pulled directly from the Merck Manual

---

## How It Works

```
PDF Documents (Merck Manual)
        ↓
  PyMuPDF Loader
        ↓
  Text Chunking (RecursiveCharacterTextSplitter)
        ↓
  OpenAI Embeddings
        ↓
  ChromaDB Vector Store
        ↓
  Similarity Search (top-k retrieval)
        ↓
  LangChain QA Chain + Prompt Engineering
        ↓
  Grounded Clinical Response
```

---

## Tech Stack

- **LLM:** OpenAI GPT (via LangChain)
- **Embeddings:** OpenAI Embeddings
- **Vector Store:** ChromaDB
- **Document Loader:** PyMuPDF (via LangChain)
- **Text Splitting:** RecursiveCharacterTextSplitter
- **Evaluation:** LLM-as-judge framework
- **Environment:** Google Colab

---

## Evaluation

Responses across all three stages were evaluated using an **LLM-as-judge** framework scoring on:

- **Groundedness** — Is the answer traceable to a verified source?
- **Relevance** — Does it directly address the clinical question?
- **Completeness** — Does it cover the key clinical details?

> **Note on evaluation:** The automated LLM-as-judge metric produced similar fluency scores across all three approaches — a known limitation of text-quality metrics, since fluency ≠ factual grounding. A manual review of specificity (exact drug dosages, treatment thresholds) present only in RAG outputs better demonstrates the real-world value of retrieval augmentation.

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/your-username/rag-medical-assistant.git
cd rag-medical-assistant
```

### 2. Create your config file
Create a `config.json` file in the root directory:
```json
{
  "OPENAI_API_KEY": "your-openai-api-key-here",
  "OPENAI_API_BASE": "https://api.openai.com/v1"
}
```

> ⚠️ **Never commit this file.** It is listed in `.gitignore`.

### 3. Install dependencies
```bash
pip install langchain langchain-openai langchain-community chromadb pymupdf tiktoken
```

### 4. Add the Merck Manual PDF
Place your PDF in the root directory and update the file path in the notebook's data loading cell.

### 5. Run the notebook
Open `medical-rag-assistant.ipynb` in Colab or Jupyter and run all cells in order.

---

## Project Structure

```
rag-medical-assistant/
├── medical-rag-assistant.ipynb   # Main notebook
├── config.json                   # API keys (not committed)
├── .gitignore
└── README.md
```

---

## Key Takeaways

- RAG significantly reduces hallucination risk for domain-specific queries by grounding answers in verified documents
- Prompt engineering alone improves structure and format but does not solve the source verification problem
- Automated evaluation metrics like LLM-as-judge measure fluency well but require manual review to capture factual precision gains

---

## Author

**Diallo** — Full-Stack Engineer & AI Application Developer  
Currently completing a Post-Graduate Program in Generative AI at UT Austin  
[LinkedIn](https://linkedin.com/in/your-profile) · [Portfolio](https://your-portfolio.com)
