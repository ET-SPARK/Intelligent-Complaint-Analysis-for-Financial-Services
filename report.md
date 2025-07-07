
# CrediTrust Financial Complaint Analysis: Project Report

## 1. Business Objective

The primary objective of this project is to develop a RAG-powered chatbot for CrediTrust Financial. This tool is designed to transform raw CFPB complaint narratives into actionable insights, focusing on five key product categories: Credit card, Personal loan, Buy Now, Pay Later (BNPL), Savings account, and Money transfer, virtual currency. By automating the analysis of customer complaints, the chatbot aims to significantly reduce the time required for manual review, empower non-technical teams with direct access to customer feedback, and enable the company to shift from a reactive to a proactive approach in addressing customer issues.

## 2. Project Execution and Progress

### Task 1: Exploratory Data Analysis and Data Preprocessing

The initial phase of the project focused on understanding and preparing the CFPB complaint dataset for the RAG pipeline.

**Summary of Findings:**

The Exploratory Data Analysis (EDA) revealed that the full dataset contained over 9.6 million records, with a substantial portion (over 6.6 million) lacking consumer narratives. The length of the narratives varied significantly, from single-word entries to extensive descriptions exceeding 6,000 words. This highlighted the necessity of robust preprocessing to handle the diverse nature of the data. The analysis also identified the most frequently complained-about financial products, which informed the filtering process.

**Preprocessing and Cleaning:**

To create a high-quality dataset for the RAG model, the following preprocessing steps were performed:

*   **Product Filtering:** The dataset was filtered to include only complaints related to the five target products.
*   **Narrative Cleaning:** Records without a `Consumer complaint narrative` were removed. The narratives were then cleaned by converting them to lowercase, removing special characters and boilerplate text, and normalizing whitespace.

The final, cleaned dataset contains 80,667 records, providing a solid foundation for the subsequent stages of the project.

**Deliverables:**

*   **Jupyter Notebook:** The complete code for the EDA and preprocessing is available in `notebooks/eda_and_preprocessing.ipynb`.
*   **Cleaned Dataset:** The cleaned dataset is saved as `data/filtered_complaints.csv`.

### Task 2: Text Chunking, Embedding, and Vector Store Indexing

The second task focused on converting the cleaned text narratives into a format suitable for efficient semantic search.

**Text Chunking:**

To ensure that the semantic meaning of the narratives was preserved, a text chunking strategy was implemented using LangChain's `RecursiveCharacterTextSplitter`. A `chunk_size` of 512 and a `chunk_overlap` of 50 were selected to create coherent and contextually relevant text chunks.

**Embedding Model:**

The `sentence-transformers/all-MiniLM-L6-v2` model was chosen to generate embeddings for the text chunks. This model offers a balance of speed and performance, making it well-suited for this application.

**Embedding and Indexing:**

Each text chunk was converted into a vector embedding and stored in a FAISS vector store for efficient similarity searches. Metadata, including the original complaint ID and product category, was stored alongside each vector to ensure that retrieved chunks can be traced back to their source.

**Deliverables:**

*   **Jupyter Notebook:** The script for chunking, embedding, and indexing is available in `notebooks/chunk_embed_index.ipynb`.
*   **Vector Store:** The persisted vector store, including the FAISS index and metadata, is saved in the `vector_store/` directory.
