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
│   └── eda_and_preprocessing.ipynb
├── src/
├── tests/
├── .gitignore
├── README.md
└── requirements.txt
```

## Getting Started

### Prerequisites

*   Python 3.9 or later
*   pip

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

To run the Exploratory Data Analysis and Data Preprocessing, you can use Jupyter Notebook:

```bash
jupyter notebook notebooks/eda_and_preprocessing.ipynb
```

## CI/CD

This project uses GitHub Actions for basic Continuous Integration. The workflow, defined in `.github/workflows/main.yml`, is triggered on every push and pull request to the `main` branch. It performs the following checks:

*   Installs project dependencies.
*   Runs `flake8` to lint the Python code for style and syntax errors.

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
    *   Converting all text to lowercase.
    *   Removing special characters and newline characters.
    *   Stripping out common boilerplate text (e.g., "I am writing to file a complaint...").
    *   Normalizing whitespace.

After these steps, the resulting filtered and cleaned dataset contains 80,667 records.

### Deliverables

*   **Jupyter Notebook:** The complete code for the EDA and preprocessing is available in `notebooks/eda_and_preprocessing.ipynb`.
*   **Cleaned Dataset:** The final, cleaned dataset is saved as `data/filtered_complaints.csv`.