# HARRY POTTER RAG CHATBOT 

A Retrieval-Augmented Generation (RAG) chatbot built with **LlamaIndex** and the **Gemini API** that allows you to ask questions based on the entire Harry Potter book collection.

## Features

* **RAG System:** Answers questions using only the information contained within the `harrypotter.pdf` document.
* **Contextual Chat:** Maintains conversation history for follow-up questions using the `gemini-2.5-flash` model.
* **Professional Setup:** Uses a `.env` file for secure API key management.

## Setup & Installation

## Prerequisites
1.  **Python 3.9+**
2.  A **Gemini API Key** (Get one from [Google AI Studio](https://makersuite.google.com/app/apikey))

## Steps 
1.  **Clone the Repository**
    git clone [your-repo-link]
    cd harry-potter-rag-chatbot
    
2.  **Install Dependencies**
    pip install -r requirements.txt

3.  **Set Up Data and API Key**
    * Create a folder named `data/` and place your `harrypotter.pdf` file inside it.
    * Create a file named **`.env`** in the root directory and add your API key:
        
# .env
    GEMINI_API_KEY="YOUR_API_KEY_HERE"
        

## Usage
Run the main script from your terminal:
python chatbot.py

The chatbot will initialize, load the document, and then prompt you for questions. Type quit to exit.

```mermaid
graph TD
    %% --- Setup & Ingestion ---
    subgraph Ingestion [📚 Data Ingestion Phase]
        PDF[📄 PDF File<br>harrypotter.pdf] --> Reader[SimpleDirectoryReader]
        Reader --> Docs[Documents]
        Docs --> Settings{LlamaIndex Settings<br>Chunk Size: 512}
        Settings --> Indexer[VectorStoreIndex]
        Indexer --> Embed[💎 Embeddings<br>text-embedding-004]
        Embed --> VectorDB[(🗄️ Vector Store)]
    end

    %% --- Chat Loop ---
    subgraph ChatLoop [💬 Chat Engine: Condense Question Mode]
        User([👤 User Input]) --> Memory[🧠 Chat History<br>ChatMemoryBuffer]
        
        %% Step 1: Condense
        Memory --> Condenser[🔄 Query Condenser]
        Condenser -- "Context + History" --> LLM1[🤖 Gemini 2.5 Flash]
        LLM1 -- "Rewrites Question" --> StandaloneQ[Standalone Query]
        
        %% Step 2: Retrieve
        StandaloneQ --> Retriever[🔍 Retrieve Context]
        Retriever -.-> VectorDB
        VectorDB -.-> Context[📄 Relevant Chunks]
        
        %% Step 3: Answer
        Context --> Generator[📝 Response Generator]
        StandaloneQ --> Generator
        Generator --> LLM2[🤖 Gemini 2.5 Flash]
        LLM2 --> Output([🤖 Final Answer])
    end

    %% Styling
    style VectorDB fill:#FFD966,stroke:#D6B656,stroke-width:2px
    style LLM1 fill:#E1D5E7,stroke:#9673A6,stroke-width:2px
    style LLM2 fill:#E1D5E7,stroke:#9673A6,stroke-width:2px
    style User fill:#DAE8FC,stroke:#6C8EBF,stroke-width:2px
```
