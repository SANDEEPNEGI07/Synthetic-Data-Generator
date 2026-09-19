# Synthetic Data Generator

A chat-based tool that generates realistic, internally consistent **fake data for testing purposes**. Describe a schema in plain English (fields, data types, constraints, number of records) and the model returns a ready-to-use JSON or CSV dataset — no real personal data involved.

Built with:
- **Meta Llama 3.2 3B Instruct** (4-bit quantized) for generation
- **Gradio ChatInterface** for the UI
- Runs on **Google Colab** with a free GPU runtime

## Features

- Generate synthetic datasets as JSON, CSV, or JSONL
- Specify exact fields, data types, ranges, and record counts in natural language
- Multi-turn chat — ask for conversions, edits, or additional records referencing what was already generated
- Streaming output in the chat window as the model generates

## Model Used: Llama 3.2 3B Instruct

This project uses [`meta-llama/Llama-3.2-3B-Instruct`](https://huggingface.co/meta-llama/Llama-3.2-3B-Instruct), a small instruction-tuned model from Meta, loaded in 4-bit precision via `bitsandbytes` so it fits comfortably on a free-tier Colab GPU (e.g. T4).

**This is a gated model.** Meta requires you to request access before you can download it — a Hugging Face account alone isn't enough.

### Getting access to the model

1. Create a free account at [huggingface.co](https://huggingface.co) if you don't have one.
2. Visit the [model page](https://huggingface.co/meta-llama/Llama-3.2-3B-Instruct) and click **"Request access"** / agree to Meta's license terms.
3. Approval is usually near-instant, but can take longer in some cases — check the model page for your access status.
4. Once approved, generate a **Hugging Face access token**:
   - Go to [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
   - Click **"New token"**, give it a name, and set the role to at least **Read**
   - Copy the token — you won't be able to view it again later

## Prerequisites

- A Google account (to use Colab)
- A Colab runtime with GPU enabled: **Runtime → Change runtime type → GPU** (T4 works for the 4-bit setup)
- A Hugging Face account with **approved access** to `meta-llama/Llama-3.2-3B-Instruct` (see above)
- A Hugging Face access token (see above)

### Adding the token to Colab

Store the token as a Colab secret so it isn't hardcoded in the notebook:

1. In Colab, click the **key icon (🔑)** in the left sidebar to open **Secrets**
2. Add a new secret named `HF_TOKEN` with your Hugging Face token as the value
3. Toggle **"Notebook access"** on for that secret

The notebook reads it with:
```python
from google.colab import userdata
hf_token = userdata.get('HF_TOKEN')
```

## Setup & Running

1. Open the notebook in Google Colab
2. Set the runtime to GPU (see Prerequisites above)
3. Add your `HF_TOKEN` as a Colab secret (see above)
4. Run the cells in order:
   - Install dependencies (`bitsandbytes`, `accelerate`, `transformers`, `gradio`)
   - Import libraries and log in to Hugging Face
   - Load the model (4-bit quantized)
   - Define the chat function
   - Launch the Gradio interface
5. Open the Gradio link (local or `share=True` public link) and start chatting

## Example Prompts

```
Generate 10 records as JSON: id, full_name, email, signup_date (2024), plan (free/pro/enterprise), monthly_spend_usd.

Give me a CSV of 25 e-commerce orders: order_id, customer_id, product, quantity (1-5), unit_price, order_date, status.

Create 15 employee records in JSON: emp_id, name, department, joining_date, salary_lpa between 4 and 25, manager_id.
```

## Notes & Limitations

- All data generated is entirely fictional — no real personal information is used or referenced.
- As a 3B model, output may occasionally have formatting issues (e.g. malformed JSON) on complex or very long schemas; regenerating or simplifying the request usually resolves this.
- The public Gradio share link (`share=True`) is temporary (~1 week) and intended for quick testing, not production use.
- GPU access on Colab's free tier is not guaranteed and may be rate-limited depending on usage.

## Tech Stack

| Component | Purpose |
|---|---|
| `transformers` | Load and run the Llama model |
| `bitsandbytes` | 4-bit quantization for GPU-efficient inference |
| `accelerate` | Model device placement |
| `huggingface_hub` | Authentication and model download |
| `gradio` | Chat UI |
| `torch` | Underlying deep learning framework |
