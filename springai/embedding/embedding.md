⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Embedding

- "splitting the query into token → converted to numerical is called embedding."
- Embedding is the process of converting text (words, tokens, sentences) into dense numerical vectors (arrays of numbers) that capture semantic meaning.
- These vectors live in a high-dimensional space where similar meanings are close together.

### ➡️ Step-by-Step Example:

- Query = "The cat sat on the mat"
- Tokenization (splitting into tokens): `["The", "cat", "sat", "on", "the", "mat"]`
- Convert each token to a numerical vector (embedding):

```text
  "The"  → [0.12, -0.45, 0.89, ...]  (e.g., 768 dimensions)
   "cat"  → [0.67,  0.23, -0.11, ...]
...

```

- **Result:** A matrix of shape [num_tokens × embedding_dim] → This numerical representation is fed into the neural network.

### ➡️ Why?

- Computers can't understand text directly.
- Embeddings allow models to perform mathematical operations (like similarity, attention) on meaning.
