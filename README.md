# Korean LLM API with Retrieval (Ollama and FastAPI)

A first prototype of a Korean question-answering API. It runs a LLaMA-family model locally through Ollama, adds context retrieved from a Korean text corpus, and serves answers through a FastAPI endpoint.

The evaluated, more complete version of this idea is in [LLM-project-RAG](https://github.com/ndrarendra/LLM-project-RAG).

## How it works

1. Loads 50,000 English-Korean sentence pairs from the OPUS-100 dataset (Hugging Face `datasets`) and keeps the Korean side as the knowledge corpus.
2. Embeds the corpus with the multilingual `paraphrase-multilingual-MiniLM-L12-v2` model and indexes it with FAISS.
3. For each question, retrieves the two closest sentences and sends them with the question to `llava:v1.6` through Ollama, with a system prompt that asks for answers in Korean.
4. Serves this through `POST /predict`.

## Run it

```bash
pip install torch transformers datasets sentence-transformers faiss-cpu fastapi uvicorn ollama
ollama serve
python test_llm.py
```

The script pulls the model on first run, builds the index (this takes a while), then starts the API on port 8000. Try it from http://localhost:8000/docs, or:

```bash
curl -X POST -G "http://localhost:8000/predict" --data-urlencode "prompt=서울에서 가볼 만한 곳을 알려줘"
```
