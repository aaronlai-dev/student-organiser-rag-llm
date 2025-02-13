# Student Organiser RAG LLM

Langchain-based RAG LLM chatbot application designed for WEHI student interns to use and answer queries.

## Local Setup

Install python dependencies
```
pip install -r requirements.txt
```

Install Ollama and pull LLM from HuggingFace
```
pip install ollama
ollama pull hf.co/llmware/bling-phi-3-gguf
```

## Nectar Project Trial Setup

Login to [Nectar](https://dashboard.rc.nectar.org.au/) and create a new instance in a free project trial
+ Image: `NeCTAR Ubuntu 22.04 LTS (Jammy) amd64` (1.4 GB)
+ Flavour: `m3.small`

Connect to the instance via SSH
```
ssh-keygen -R [IP address]
ssh ubuntu@[IP address]
```

Setup python environment in the fresh instance
```
sudo apt install python3-pip
python3 -m venv .venv
source .venv/bin/activate
```

On local machine, send zipped folder `rag.zip` the script/chromadb/requirements.txt
```
scp rag.zip ubuntu@[IP address]:~/ 
```

Back on the python environment in the Nectar instance, unzip folder and download python dependecies
```
unzip rag.zip
cd rag
pip install -r requirements.txt
```

Given the limited storage/compute constraints of this instance, a smaller LLM must be pulled from `ollama`
The smaller 1.5B parameter model used in testing was the `bling-qwen-mini-tool`

[Hugging Face Repo](https://huggingface.co/llmware/bling-qwen-mini-tool)
```
pip install ollama
curl -fsSL https://ollama.com/install.sh | sh
ollama pull hf.co/llmware/bling-qwen-mini-tool
```

Run python script
```
python3 rag.py
```

## RAG Pipeline Overview

Pipeline: [Langchain](https://github.com/langchain-ai/langchain)  
Vector Database: [ChromaDB](https://github.com/chroma-core/chroma)  
Embeddings Model: [all-MiniLM-L6-v2](https://sbert.net/docs/sentence_transformer/pretrained_models.html#original-models)  
Language Model: [bling-phi-3-gguf](https://huggingface.co/llmware/bling-phi-3-gguf)  
Front-End Chat Interface: [Gradio](https://github.com/gradio-app/gradio)  

Notes on Language Model
+ Open-source
+ Trained and tuned Microsoft `phi-3` model by [llmware](https://github.com/llmware-ai/llmware)
+ Only 3.5B parameters, ~2.4 GB storage needed
+ BLING model series (best-little-instruct-no-gpu), essential for our application with no GPU

## Examples

<img width="1364" alt="image" src="https://github.com/user-attachments/assets/78def8e1-0d41-4397-9c56-77d04027d532" />
<img width="1365" alt="image" src="https://github.com/user-attachments/assets/c86e723d-d1e4-4f81-8331-9afcf0c7fae4" />

## Performance Evaluation

Average query time: 30-40s
Accuracy: Moderate accuracy, minimal hallucinations

## Future Tasks

1. Create interface for administrators to easily add websites/PDFs and update vector database
2. Cache chat history for greater context window
3. Weighting of higher priority sources (ie. FAQ Page)
