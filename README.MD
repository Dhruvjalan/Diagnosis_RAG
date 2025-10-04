# Multi-Agent Clinical Decision Support System

This project is a sophisticated clinical decision support system that leverages a multi-agent architecture, a hybrid Retrieval-Augmented Generation (RAG) pipeline with cross-encoder re-ranking, and standardized MCP tools to assist in clinical diagnosis.

## 🌟 Key Features

* **Three-Agent Pipeline:** A structured workflow with a `ClinicalQAAgent`, `TriageAgent`, and `DiagnosisAgent` for comprehensive analysis.
* **Advanced RAG:** Implements a two-stage retrieval process using a fast bi-encoder (ClinicalBERT) and a more accurate cross-encoder for re-ranking, ensuring high-quality evidence is used.
* **MCP-Compliant Tools:** A full suite of `ehr.*` and `calc.*` tools for standardized data access and clinical calculations (qSOFA, eGFR).
* **Fine-Tuning Ready:** Includes complete scripts for Domain-Adaptive Pre-training (DAPT) and parameter-efficient fine-tuning with LoRA.
* **Comprehensive Evaluation:** A built-in evaluation framework that generates five required submission-ready output files (`retrieval_results.jsonl`, `qa_results.jsonl`, `triage_results.jsonl`, `diagnosis_results.jsonl`, `system_metrics.json`).

## 🏗️ System Architecture

The system operates on a structured pipeline that processes patient data through a series of specialized agents.

1.  **Data Pre-computation:** The Synthea 1k dataset is processed into a `jsonl` corpus of text snippets. These snippets are then encoded using ClinicalBERT and stored in a FAISS vector index.
2.  **Clinical QA Agent:** Receives the initial query, uses the two-stage RAG tool (`ehr.search_evidence`) to retrieve and re-rank relevant historical data, and synthesizes a summary.
3.  **Triage Agent:** Takes the QA summary, fetches recent labs and vitals using `ehr.get_labs` and `ehr.get_vitals`, and uses `calc.*` tools to interpret results and calculate clinical scores.
4.  **Diagnosis Agent:** Integrates the outputs from the previous two agents to generate a final, provisional diagnosis with confidence scores.



## 🚀 Setup and Execution

### 1. Initial Setup (Run Once)

This one-time process prepares the environment, downloads data, and builds the RAG index.

1.  **Clone the Repository & Install Dependencies:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-name>
    pip install -r requirements.txt
    ```
2.  **Configure Secrets:** In your Google Colab environment, go to the "Secrets" tab (key icon) and add your `GROQ_API_KEY`.
3.  **Run the Setup & Indexing Cells:** In your Colab notebook, execute the following cells in order:
    * **Cell 1 (Setup & Installation):** Mounts your Google Drive and downloads the Synthea dataset.
    * **Cell 2 (Data Loading & Pre-computation):** Loads the raw data, generates snippets, and builds the FAISS index. This may take several minutes.

### 2. Running a Diagnostic Session

After the initial setup is complete, follow these steps to run the main diagnostic pipeline.

1.  **Run Cell 1 (Setup & Installation):** This ensures your environment and API keys are loaded.
2.  **Run Cell 4 (MCP Tool Implementation):** This loads the RAG tools and clinical calculators.
3.  **Run Cell 5 (The Three-Agent Pipeline):** This defines the agents.
4.  **Run Cell 6 (Orchestrator & Main Execution):** This will start the interactive session, prompting you for a Patient ID and an initial query.

**Quick Order:** `Cell 1 -> Cell 4 -> Cell 5 -> Cell 6`

### 3. Running the Evaluation Framework

To generate all the required submission files:

1.  **Run the main setup:** Execute `Cell 1`, `Cell 2` (to ensure `ehr_data` is loaded), `Cell 4`, and `Cell 5`.
2.  **Run Cell 8 (Evaluation Framework):** This will execute the pipeline on 20 test cases and save the five required output files to your `rag_output` directory.

**Quick Order:** `Cell 1 -> Cell 2 -> Cell 4 -> Cell 5 -> Cell 8`



About out submission: 

--Extra_Model : .ipynb with the Extra model working
-- rag_output   
    Patient_Results: The reports outputted by the model
    eval_results: The asked evaluation metricsa in the required formats

    This also has the model files required. 
    rag_output/planner_finetuning_data.jsonl - fine tuining model data
    rag/output/rag_corpus = the corpus created out of snippets.
    rag_output/synthea_faiss.index = inrdex file of the synthea corpus data, converted into embeddings. 
Demo_Videos: https://drive.google.com/drive/folders/1VjIfOX23yUf9_gxnB2PH47jpx2UEHafm?usp=sharing
GITHUB: git@github.com:Dhruvjalan/Task3_HybridRAG.git
