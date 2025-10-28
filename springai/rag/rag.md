⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ RAG (Retrieval-Augmented Generation)

- A technique to make LLMs smarter by fetching real-time or external knowledge before answering.

### ➡️ Problem with Pure LLMs:

- Pretrained on fixed data → hallucinations (making up facts).
- No access to latest info or private documents.

### ➡️ RAG Solves This:

- **Retrieve** relevant documents from a knowledge base(Knowledge Source) (e.g., your company docs, web, database).
- **Augment** the user query with retrieved info.
- **Generate** answer using LLM + retrieved context.

```text
User Query: "Explain AI terms"
        ↓
[Tokenization] → ["Explain", "AI", "terms"]
        ↓
[Embedding] → [[0.1, -0.3, ...], [0.5, 0.2, ...], ...]
        ↓
[RAG Search] → Retrieve docs about "embedding", "pretraining"
        ↓
[LLM + Context] → Generate this explanation!
```
