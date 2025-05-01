# 🦍 Gorilla-Inspired API Call Generator

This project is a **Generative AI system** that translates natural language instructions into executable API calls using models from HuggingFace, TorchHub, and TensorFlow Hub. Inspired by the [Gorilla paper (UC Berkeley)](https://arxiv.org/abs/2305.15334), this implementation focuses on instruction-tuning a language model to generate correct, constraint-aware API calls.

---

## 🚀 Project Overview

This system enables:

- **Instruction to API generation** (e.g., `"I want to classify objects in an image"` → `torch.hub.load(...)`)
- **Retriever-aware inference** (retrieves the relevant API doc during inference)
- **Evaluation via AST sub-tree matching** for functional correctness

---

## 📁 Dataset Structure (APIBench)

The dataset is divided into three model hubs: HuggingFace, TorchHub, and TensorFlow Hub.

| File               | Description                                  |
|--------------------|----------------------------------------------|
| `*_train.json`     | Training data (instruction → API call pairs) |
| `*_eval.json`      | Evaluation set for accuracy/AST matching     |
| `*_api.jsonl`      | Raw API documentation used by retriever      |

> Example: `huggingface_train.json` contains pairs like:
```json
{
  "instruction": "Generate image embeddings using a pre-trained vision transformer.",
  "api_call": "CLIPModel.from_pretrained('openai/clip-vit-base-patch32')"
}
```

---

## ✅ Supported APIs

You can choose to support any of these:

- ✅ **HuggingFace** (recommended: large model base, well-documented APIs)
- **TorchHub** (smaller set, but useful for PyTorch-native users)
- **TensorFlow Hub** (slightly more verbose APIs, used in TF ecosystems)

**Recommendation:**  
👉 Start with **HuggingFace**, as it has the most cleanly documented and useful model cards, ideal for prototyping.

---

## 🧠 Model Training (Gorilla-Inspired)

We fine-tune an open-source LLM using the instruction-API pairs in the dataset.

### Steps:

1. Format data in chat-style:
    ```
    User: I want to analyze sentiment in text.  
    Assistant: pipeline('sentiment-analysis')
    ```

2. Finetune with:
    - LLaMA / Mistral / GPT2
    - LoRA or PEFT for lightweight fine-tuning

3. Add retrieval augmentation using:
    - BM25 or LlamaIndex to fetch API descriptions
    - Concatenate retrieved API doc to user prompt during training/inference

---

## 🧪 Evaluation

Uses **AST Sub-Tree Matching**:
- Parses API calls as abstract syntax trees
- Matches candidate vs. ground truth for functional correctness
- Detects hallucinations (calls to non-existent APIs)

---

## 📚 Files

| File                  | Purpose                                 |
|------------------------|------------------------------------------|
| `huggingface_train.json` | Instruction → API pairs (HuggingFace)  |
| `huggingface_eval.json`  | Evaluation set                         |
| `huggingface_api.jsonl`  | Raw API docs                           |
| `torchhub_*` and `tensorflow_*` | Same structure for other frameworks |

---

## 🧰 Tech Stack

- Python
- Transformers / PEFT / LoRA
- FAISS / BM25 for retrieval
- AST (`ast` module in Python)
- JSONL/JSON format for data

---

## 🤝 Acknowledgements

This project is inspired by the [Gorilla paper (2023)](https://arxiv.org/abs/2305.15334) and the APIBench dataset.
