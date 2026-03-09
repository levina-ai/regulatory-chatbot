# 🏦 PRIIPS/KID Regulatory Comparison Chatbot

A **Retrieval-Augmented Generation (RAG)** chatbot that ingests EU PRIIPS KID and UK FCA regulatory documents and answers compliance queries across both frameworks. 
Built as a portfolio project to demonstrate applied GenAI engineering in a financial services context.

---

## 📌 Use Case

Financial institutions operating across EU and UK markets must comply with two diverging regulatory frameworks for packaged retail investment products (PRIIPs). This tool allows compliance teams and analysts to query both frameworks simultaneously and surface key differences — without manually reading hundreds of pages of regulation.

---

## 🏗️ Architecture

```
PDF Documents (EU PRIIPS + UK FCA)
        ↓
Document Loading & Chunking
(PyPDFLoader + RecursiveCharacterTextSplitter)
        ↓
Embedding
(Azure OpenAI — text-embedding-ada-002)
        ↓
Vector Indexing
(Azure AI Search)
        ↓
User Query → Similarity Search → Retrieved Chunks
        ↓
Prompt Construction + LLM Generation
(Azure OpenAI — GPT-4o-mini, temperature=0)
        ↓
Grounded Answer with Source Attribution
```

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| LLM | Azure OpenAI — GPT-4o-mini |
| Embeddings | Azure OpenAI — text-embedding-ada-002 |
| Vector Store | Azure AI Search |
| Orchestration | LangChain |
| Notebook | Google Colab |
| Language | Python 3.12 |

---

## 📄 Documents Used

| Framework | Document | Source |
|---|---|---|
| EU PRIIPS | Commission Delegated Regulation (EU) 2017/653 (consolidated Jan 2023) | [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX:02017R0653-20230101) |
| UK FCA | Policy Statement PS22/2 — UK PRIIPs post-Brexit divergence | [FCA](https://www.fca.org.uk/publication/policy/ps22-2.pdf) |

---

## ⚙️ Setup

### Prerequisites
- Azure account with Azure OpenAI and Azure AI Search resources

### Azure Resources Required
1. **Azure OpenAI** with two deployed models:
   - `gpt-4o-mini` — answer generation
   - `text-embedding-ada-002` — document and query embeddings

2. **Azure AI Search** — vector index storage and retrieval

### Credentials
Fill in Cell 2 of the notebook with your credentials:

```python
os.environ['AZURE_OPENAI_ENDPOINT']  = 'https://xxx.openai.azure.com/'
os.environ['AZURE_OPENAI_API_KEY']   = 'your_key'
os.environ['AZURE_SEARCH_ENDPOINT']  = 'https://xxx.search.windows.net'
os.environ['AZURE_SEARCH_API_KEY']   = 'your_key'
os.environ['AZURE_SEARCH_INDEX']     = 'priips-index'
```

---

## 🚀 Running the Notebook

Run cells in order:

| Cell | Purpose |
|---|---|
| 1 | Install dependencies |
| 2 | Set credentials |
| 3 | Upload EU and UK PDF documents |
| 4 | Load and chunk documents |
| 5 | Embed and index in Azure AI Search |
| 6 | Initialise LLM and RAG pipeline |
| 7 | Run preset comparison queries |
| 8 | Interactive mode — ask your own questions |

---

## 💬 Example Queries

```python
# Single framework
ask("What are the cost disclosure requirements under EU PRIIPS?")
ask("What are the cost disclosure requirements under UK FCA?")

# Comparison
ask("What are the main differences between EU PRIIPS and UK FCA KID requirements?")
ask("How do performance scenarios differ between the EU and UK regimes?")
ask("What are the differences in risk measures for structured products?")
```

### Example Output
```
============================================================
🔍  What are the main differences between EU PRIIPS and UK FCA KID requirements?
============================================================

EU PRIIPS requires mandatory past performance scenarios using
prescribed stress, unfavourable, moderate and favourable scenarios.
The UK FCA regime under PS22/2 removed mandatory future performance
scenarios, replacing them with past performance data where available...

📄  Sources: ['EU_PRIIPS', 'UK_FCA'] | Pages: [p.12, p.34, p.7, p.19]
```

---

## 🔑 Key Design Decisions

**temperature=0** — the LLM is set to fully deterministic mode. For compliance use cases, reproducibility and accuracy take priority over creativity.

**Source attribution** — every answer includes which documents and pages were used, enabling manual verification against source material.

**Filterable index** — `source_label` is registered as a filterable field in Azure AI Search at index creation, enabling framework-specific queries.

---

## 📁 Project Structure

```
priips-fca-chatbot/
│
├── priips_fca_chatbot.ipynb   # Main notebook
├── README.md                  # This file
└── .gitignore                 # Excludes credentials and checkpoints
```

---

## 🗺️ Potential Extensions

- Add evaluation metrics (retrieval precision, answer relevance)
- Swap `text-embedding-ada-002` for `text-embedding-3-large` for higher accuracy
- Deploy as an Azure Web App for team access

---

## 👩‍💻 Author

**Anastasiya Levina**
[LinkedIn](https://linkedin.com/in/anastasiya-levina) · [GitHub](https://github.com/levina-ai)
