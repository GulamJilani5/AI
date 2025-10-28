⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Vector Store (or Vector Database)

A Vector Store (also called a Vector Database) is a special type of database that stores embeddings — which are numerical vector representations of text (or images, etc.).

- **Analogy**

  - Normal databases search for exact words (**e.g.**, `“car”` ≠ `“automobile”`).
  - A **vector store** searches by semantic meaning (**e.g.**, `“car”` ≈ `“automobile”` because their vectors are close in space).

- **Example**
  - Storing and Searching

```java
import org.springframework.ai.document.Document;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;
import java.util.Map;

@RestController
public class VectorStoreExample {

    @Autowired
    private VectorStore vectorStore;  // Auto-configured PgVectorStore

    // Step 1: Add documents (auto-embeds and stores them)
    public void storeDocuments() {
        List<Document> docs = List.of(
            new Document("Spring Boot makes Java web apps easy.",
                         Map.of("topic", "java", "year", 2023)),
            new Document("AI is transforming software development.",
                         Map.of("topic", "ai", "year", 2024)),
            new Document("Vector databases power semantic search.",
                         Map.of("topic", "db", "year", 2024))
        );
        vectorStore.add(docs);
        System.out.println("Documents stored!");
    }

    // Step 2: Simple similarity search
    @GetMapping("/search")
    public List<Document> search() {
        String query = "What is Spring for AI?";
        // Returns top 4 similar docs (default); scores show relevance (0-1)
        return vectorStore.similaritySearch(query);
    }

    // Advanced: Search with filters (e.g., only recent AI topics)
    @GetMapping("/filtered-search")
    public List<Document> filteredSearch() {
        SearchRequest request = SearchRequest.builder()
                .query("AI and databases")
                .withTopK(2)  // Limit results
                .withFilterExpression("topic == 'ai' && year >= 2024")  // Metadata filter
                .build();
        return vectorStore.similaritySearch(request);
    }
}
```

- **How It Works**

- **Storing:** Call `storeDocuments()` — Spring AI embeds each doc using OpenAI, stores the vectors + original text/metadata in the DB.
- **Searching:** Hit `/search` — It embeds your query, finds nearest vectors, returns matching docs (e.g., the AI one might score **0.85** similarity).

### ➡️ How It’s Used in AI Applications (Especially with Spring AI)

- When building AI-powered apps (like chatbots, search tools, or knowledge assistants), you often want your LLM (e.g., OpenAI, Azure etc) to use your custom data.
- But LLMs don’t “know” your data — so here’s how a **vector store** helps:

- **Flow:**

  - Convert your documents into embeddings using an embedding model (e.g., OpenAIEmbeddingModel).
  - Store those embeddings in a vector store.
  - When a user asks a question, convert their query into an embedding too.
  - The vector store finds the most similar stored embeddings (using cosine similarity).
  - Those relevant text chunks are sent to the LLM as context → making it seem like the LLM “knows” your data.

- This process is called **RAG** (Retrieval-Augmented Generation).
