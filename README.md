# Pipeline for Context-Aware Question-Answering on Tesla’s 10-K Filing using RAG

## Overview
This pipeline implements a **Retrieval-Augmented Generation (RAG)** system to extract contextually relevant answers from Tesla's most recent 10-K filing. It involves several stages: data acquisition, content preprocessing, context-aware retrieval, and integration of the Gemini generative model to answer specific questions.

## Methodology

1. **Data Acquisition**  
   The first step involves using the **SEC Edgar Downloader** package to download Tesla's most recent 10-K filing. The document is stored in a local directory for further processing.

2. **Text Preprocessing**  
   The raw text is cleaned by removing XML tags, metadata, and irrelevant patterns (e.g., whitespace and section headers). This ensures structured content for efficient retrieval.

3. **Content Chunking**  
   The cleaned text is split into smaller chunks of approximately 1,000 characters. This chunking facilitates retrieval of relevant sections while maintaining context.

4. **Context-Aware Retrieval**  
   A **FAISS (Facebook AI Similarity Search)** index is built using sentence embeddings. The **Sentence-BERT** model converts each chunk into vector embeddings, which are stored in the FAISS index for efficient similarity searches.

5. **Model Integration (Gemini)**  
   When a query is made, the system retrieves the most relevant chunks from the FAISS index. These chunks, along with the query, are passed to the **Gemini model** (via Google Cloud’s Vertex AI). The model generates a coherent, contextually relevant answer.

6. **Question Handling**  
   Various sample questions (e.g., “What are Tesla’s major expenses?”) are tested to validate the pipeline. The model retrieves pertinent document sections and generates precise answers, demonstrating the RAG system’s effectiveness.

## Key Findings
- The RAG pipeline efficiently extracts contextually relevant information, showcasing the power of combining retrieval and generative models.
- FAISS significantly improves the speed and accuracy of retrieving relevant chunks.
- Gemini’s ability to generate coherent answers confirms the effectiveness of integrating retrieval-augmented generation for large document analysis.

## Limitations
- **Chunk Size Constraints:** The model's context length is limited (2,048 tokens), meaning longer documents or highly detailed questions may require truncation, potentially losing context.
- **Quality of Retrieval:** The embedding model (Sentence-BERT) affects retrieval quality. Poor embeddings may lead to imprecise answers.
- **Dependency on Preprocessing:** The accuracy of the pipeline depends heavily on document cleaning and chunking. Inconsistent preprocessing could degrade retrieval performance.

## Future Improvements
- **Optimizing Preprocessing:** Using NLP models specifically trained on financial documents could enhance cleaning and chunking.
- **Enhanced Retrieval Models:** Experimenting with other retrieval models or combining multiple retrieval mechanisms could improve relevance.
- **Fine-Tuning Gemini:** Fine-tuning the generative model on financial datasets may increase accuracy and specificity, especially for domain-specific queries.

---

## Setup Instructions

### Prerequisites
- Python 3.8 or higher
- Google Cloud account with access to **Vertex AI**
- The following Python packages:
  - `faiss`
  - `transformers`
  - `sentence-transformers`
  - `edgar_downloader`
  - `google-cloud-vertex-ai`
  - `pandas`, `numpy`

### Installation

1. Clone the repository:
    ```bash
    git clone https://github.com/lrosok/Context-Aware-Question-Answering-on-Tesla-s-10-K-Filing-using-RAG.git
    cd Context-Aware-Question-Answering-on-Tesla-s-10-K-Filing-using-RAG
    ```

2. Create a virtual environment and activate it:
    ```bash
    python -m venv venv
    source venv/bin/activate   # On Windows use `venv\Scripts\activate`
    ```

3. Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```

4. Set up your Google Cloud credentials:
    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS="path/to/your/credentials.json"
    ```

---

## Usage Instructions

1. **Download Tesla's 10-K Filing**  
   Use the `edgar_downloader` to fetch the most recent Tesla 10-K filing:
    ```bash
    python download_10k.py
    ```

2. **Preprocess the Data**  
   Run the preprocessing script to clean and chunk the document:
    ```bash
    python preprocess_10k.py
    ```

3. **Build the FAISS Index**  
   Create the FAISS index from the preprocessed chunks:
    ```bash
    python build_faiss_index.py
    ```

4. **Ask Questions**  
   Use the pipeline to submit questions and get answers:
    ```bash
    python query_pipeline.py --question "What are Tesla’s major expenses?"
    ```

---

## Contributing
Feel free to fork the repository, make changes, and submit a pull request. Contributions that improve retrieval or generative performance are welcome.

## License
This project is licensed under the MIT License.

