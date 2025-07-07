# Intelligent Complaint Analysis for Financial Services

This project aims to build a complaint-answering chatbot using Retrieval-Augmented Generation (RAG). The project is completed step-by-step, with clean code, organized folders, and clear documentation.

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── main.yml
├── data/
│   └── filtered_complaints.csv
├── notebooks/
│   ├── eda_and_preprocessing.ipynb
│   └── chunk_embed_index.ipynb
├── src/
│   ├── app.py
│   └── rag_pipeline_and_evaluation.py
├── tests/
├── vector_store/
│   ├── faiss_index.index
│   └── metadata.pkl
├── .gitignore
├── README.md
└── requirements.txt
```

## Getting Started

### Prerequisites

- Python 3.9 or later
- pip

### Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/your-username/Intelligent-Complaint-Analysis-for-Financial-Services.git
    cd Intelligent-Complaint-Analysis-for-Financial-Services
    ```

2.  **Create and activate a virtual environment (recommended):**

    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install the required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

## Usage

### Jupyter Notebooks

To run the notebooks, you can use Jupyter Notebook:

```bash
jupyter notebook notebooks/
```

### Interactive Chat Application

To run the interactive chat application:

```bash
python src/app.py
```

## CI/CD

This project uses GitHub Actions for basic Continuous Integration. The workflow, defined in `.github/workflows/main.yml`, is triggered on every push and pull request to the `main` branch. It performs the following checks:

- Installs project dependencies.
- Runs `flake8` to lint the Python code for style and syntax errors.

This ensures that the codebase maintains a consistent quality standard.

## Task 1: Exploratory Data Analysis and Data Preprocessing

The first step is to understand the structure, content, and quality of the complaint data and prepare it for the RAG pipeline.

### Summary of Findings

The initial Exploratory Data Analysis (EDA) was performed on the full CFPB complaint dataset, which contains over 9.6 million records. A significant finding was the high number of complaints lacking a consumer narrative; over 6.6 million entries had this field empty. For the complaints that did include a narrative, the length varied drastically, with some being as short as a single word and others exceeding 6,000 words. This wide range highlights the diverse nature of the consumer feedback and underscores the need for robust preprocessing to handle such variations.

The analysis also included an examination of complaint distribution across different financial products. Visualizations showed which products received the most complaints, providing context for the subsequent filtering step. The EDA confirmed that to build a high-quality RAG model, it is essential to filter the dataset to a specific subset of products and ensure that only complaints with detailed narratives are used for training and retrieval.

### Preprocessing and Cleaning

The dataset was filtered and cleaned to prepare it for the embedding model. The following steps were performed:

1.  **Filtered by Product:** The dataset was narrowed down to include only complaints related to five specific products: `Credit card`, `Personal loan`, `Buy Now, Pay Later (BNPL)`, `Savings account`, and `Money transfer, virtual currency`.
2.  **Removed Empty Narratives:** All records without a `Consumer complaint narrative` were removed.
3.  **Text Cleaning:** The complaint narratives were cleaned to improve the quality of future text embeddings. The cleaning process included:
    - Converting all text to lowercase.
    - Removing special characters and newline characters.
    - Stripping out common boilerplate text (e.g., "I am writing to file a complaint...").
    - Normalizing whitespace.

After these steps, the resulting filtered and cleaned dataset contains 80,667 records.

### Deliverables

- **Jupyter Notebook:** The complete code for the EDA and preprocessing is available in `notebooks/eda_and_preprocessing.ipynb`.
- **Cleaned Dataset:** The final, cleaned dataset is saved as `data/filtered_complaints.csv`.

## Task 2: Text Chunking, Embedding, and Vector Store Indexing

To convert the cleaned text narratives into a format suitable for efficient semantic search, the following steps were taken:

### Text Chunking

Long narratives are often ineffective when embedded as a single vector. To address this, a text chunking strategy was implemented using LangChain's `RecursiveCharacterTextSplitter`. After experimenting with different configurations, a `chunk_size` of 512 and a `chunk_overlap` of 50 were chosen. This balance ensures that the chunks are small enough to maintain semantic coherence while the overlap helps preserve context between them.

### Embedding Model

The `sentence-transformers/all-MiniLM-L6-v2` model was chosen for generating embeddings. This model is a good starting point because it is lightweight, fast, and provides high-quality sentence embeddings, making it ideal for semantic search applications where efficiency is important.

### Embedding and Indexing

For each text chunk, a vector embedding was generated. These embeddings were then stored in a FAISS vector store, which allows for efficient similarity searches. Crucially, metadata—including the original complaint ID and product category—was stored alongside each vector. This ensures that any retrieved chunk can be traced back to its source complaint.

### Deliverables

- **Jupyter Notebook:** The script that performs chunking, embedding, and indexing is available in `notebooks/chunk_embed_index.ipynb`.
- **Vector Store:** The persisted vector store, including the FAISS index and metadata, is saved in the `vector_store/` directory.

## Task 3: Building the RAG Core Logic and Evaluation

To build the retrieval and generation pipeline and, most importantly, to evaluate its effectiveness.

### Retriever Implementation

Create a function that takes a user's question (string) as input.

- Embeds the question using the same model from Task 2.
- Performs a similarity search against the vector store to retrieve the top-k most relevant text chunks. k=5 is a good starting point.

### Prompt Engineering

Design a robust prompt template. This is critical for guiding the LLM. The template should instruct the model to act as a helpful analyst, use only the provided context, and answer the user's question based on that context.

**Example Template:**

```
You are a financial analyst assistant for CrediTrust. Your task is to answer questions about customer complaints. Use the following retrieved complaint excerpts to formulate your answer. If the context doesn't contain the answer, state that you don't have enough information.

Context: {context}
Question: {question}
Answer:
```

### Generator Implementation

- Combine the prompt, the user question, and the retrieved chunks.
- Send the combined input to an LLM (e.g., using Hugging Face's pipeline or LangChain's integrations with Mistral, Llama, etc.).
- Return the LLM's generated response.

### Qualitative Evaluation

This is the most important step.

- Create a list of 5-10 representative questions you want your system to answer well.
- For each question, run your RAG pipeline and analyze the results.
- Create an evaluation table in your report (Markdown format is fine) with columns: `Question`, `Generated Answer`, `Retrieved Sources` (show 1-2), `Quality Score` (1-5), and `Comments/Analysis`.

### Deliverables

- Python modules (.py file) containing your RAG pipeline logic.
- The evaluation table and your analysis in the final report, explaining what worked well and what could be improved.
- The `src/rag_pipeline_and_evaluation.py` file contains the implementation of the RAG pipeline.

## Task 4: Interactive Chat Interface

To provide a user-friendly way to interact with the RAG system, an interactive chat interface was built using Gradio.

### Features

-   **Text Input:** A simple text box for users to ask questions about financial complaints.
-   **Submit Button:** A button to send the question to the RAG pipeline.
-   **Answer Display:** A dedicated area to show the AI-generated answer.
-   **Source Display:** To enhance trust and transparency, the interface displays the raw text chunks that the RAG system used as a source for its answer.
-   **Clear Button:** Allows the user to reset the input and output fields.

### How to Run the Application

1.  **Ensure all dependencies are installed:**
    ```bash
    pip install -r requirements.txt
    ```
2.  **Run the Gradio application:**
    ```bash
    python src/app.py
    ```
    The application will be available at a local URL provided by Gradio (usually `http://127.0.0.1:7860`).

### Deliverables

-   **Application Script:** The Gradio application is implemented in `src/app.py`.
-   **Screenshots:** Screenshots of the application are included in the `Screenshots/` directory.
