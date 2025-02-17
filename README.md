# :zap: Student Organiser RAG LLM

Langchain-based RAG LLM chatbot application designed for WEHI student interns to use and answer queries.

## :computer: Local Setup

Install python dependencies
```
pip install -r requirements.txt
```

Install Ollama and pull LLM from HuggingFace
```
pip install ollama
ollama pull hf.co/llmware/bling-phi-3-gguf
```

Run the python script
```
python3 rag.py
```

## :honey_pot: Nectar Project Trial Setup

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

Optional: Install and run a [tmux](https://github.com/tmux/tmux/wiki) session so that the application can
continue to run after disconnecting to SSH.
```
sudo apt install tmux
tmux new -s gradio_session
python3 rag.py
```
Detach to the tmux session by pressing `Ctrl + B` followed by `D`
Reattach to the session later if needed via
```
tmux attach -t gradio_session
```

## :fishing_pole_and_fish:  RAG Pipeline Overview

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

## :memo:  Examples

<img width="1364" alt="image" src="https://github.com/user-attachments/assets/78def8e1-0d41-4397-9c56-77d04027d532" />
<img width="1365" alt="image" src="https://github.com/user-attachments/assets/c86e723d-d1e4-4f81-8331-9afcf0c7fae4" />

## :microscope:  Performance Evaluation

Average query time: 30-40s  
Accuracy: Moderate accuracy, minimal hallucinations

Accuracy Evaluation Methodology:
+ 10 queries tested across 3 different tieres of complexity
+ Complexity Levels: Basic matching keyword, one source semantic, multiple source semantic)
+ 1 point for correct response/sources, 0.5 point for correct sources, 0 points for incorrect responses/sources

The model achieved an accuracy score of 6.5/10.

## :rocket:  Future Tasks

1. Create interface for administrators to easily add websites/PDFs and update vector database
2. Cache chat history for greater context window
3. Weighting of higher priority sources (ie. FAQ Page)
4. Host indefinitely on a permanent Nectar instance
5. Request for a larger instance allocation in Nectar to use better/larger models for improved accuracy
