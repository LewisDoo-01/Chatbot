# Local RAG Chatbot with PDF and Llama 2

This is a simple chatbot project utilizing a Retrieval-Augmented Generation (RAG) architecture to answer questions based on the content of provided PDF documents. The entire application runs 100% locally, requiring no API keys, and uses Llama 2 with a ChromaDB vector database.

## How It Works

The project is split into two main parts:

1.  **Data Ingestion (`create_db.ipynb`)**:
    * Scans the `/data` directory for all PDF files.
    * Reads and splits their content into manageable text chunks.
    * Uses the `sentence-transformers/all-MiniLM-L6-v2` model to create vector embeddings for each chunk.
    * Stores these vectors in a local ChromaDB vector database in the `/database` directory.

2.  **Chat Application (`app.ipynb`)**:
    * Launches a simple web server using Flask.
    * Loads a local LLM (Llama 2 GGML model) using `CTransformers`.
    * Loads the persistent ChromaDB vector database created in the first step.
    * Sets up a `RetrievalQA` chain from LangChain.
    * When a user submits a query:
        * The system retrieves the most relevant text chunks (context) from ChromaDB.
        * It passes the context and the query to Llama 2 to generate an answer.
        * The answer is displayed on the web interface.

## Tech Stack

* **Python**
* **LangChain**: The primary framework for connecting the RAG components.
* **Flask**: For creating the simple web app UI.
* **ChromaDB**: The local vector database.
* **CTransformers**: A library to run GGUF/GGML models locally on CPU/GPU.
* **Sentence Transformers**: Used for generating embeddings.
* **Llama 2**: The `TheBloke/Llama-2-7B-Chat-GGML` model is used as the generative AI.

## Setup

1.  Clone this repository:
    ```bash
    git clone [https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git)
    cd YOUR_REPO_NAME
    ```

2.  Create a virtual environment and activate it:
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  Create a `requirements.txt` file with the following content and install the dependencies:
    ```ini
    langchain
    flask
    ctransformers
    sentence-transformers
    chromadb
    pypdf
    tqdm
    ```
    Then run:
    ```bash
    pip install -r requirements.txt
    ```

4.  Download the Llama 2 model (GGML or GGUF). You can download it from [TheBloke/Llama-2-7B-Chat-GGML](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML).
    * Create a `models/` directory.
    * Place the model file (e.g., `llama-2-7b-chat.ggmlv3.q8_0.bin`) inside the `models/` directory.
    * *Note: You may need to update the model name in `app.ipynb` to match the exact filename you downloaded.*

## How to Use

1.  **Add Your Data:**
    * Place all the PDF files you want the chatbot to learn from into the `data/` directory.

2.  **Create the Vector Database:**
    * Open and run all cells in the `create_db.ipynb` notebook.
    * This may take a while depending on the number and size of your PDF files.
    * Once finished, you should see a `database/` directory.

3.  **Run the Chatbot:**
    * Open and run all cells in the `app.ipynb` notebook.
    * The Flask server will start.

4.  **Access the App:**
    * Open your browser and navigate to `http://127.0.0.1:8090` (or the port shown in your terminal).
