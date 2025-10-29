⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Advisor

**Advisors** API in Spring AI is a mechanism to **intercept**, **modify**, **augment** or **block** interactions between your application and a **Language Model** (`LLM/Chat Model`). It acts like an **AOP** (aspect-oriented) layer for AI calls.

- It acts like a chain of interceptors, allowing you to inject reusable patterns for tasks such as memory management, content safety, retrieval-augmented generation (RAG), and reasoning improvements.
- Advisors can operate on both synchronous (non-streaming) and reactive (streaming) flows, and they integrate seamlessly with the ChatClient API.

```java
   ChatClient client = ChatClient.builder(chatModel)
    .defaultAdvisors(
        new MessageChatMemoryAdvisor(memoryStore),
        new SafeGuardAdvisor(),
        new MariaDBVectorStoreQAAdvisor(vectorStore)
    )
    .build();
```

- Here, each **Advisor** extends or implements **ChatAdvisor**.

## ➡️ MessageChatMemoryAdvisor:

- Retrieves and attaches conversation memory as message objects, enabling more natural multi-turn chat with role-based context.
- Stores and retrieves chat messages (user and AI) to maintain conversation context across multiple turns.

```java
@Bean
public ChatClient chatClient(ChatModel model, ChatMemory chatMemory) {
    return ChatClient.builder(model)
        .defaultAdvisors(new MessageChatMemoryAdvisor(chatMemory))
        .build();
}

```

## ➡️ PromptChatMemoryAdvisor:

- Simpler and best for single, non-role-based interactions where the context is injected directly into the prompt
- Instead of storing full conversation messages, it injects summarized or relevant memory into the prompt itself.
  - It keeps prompts shorter and cheaper than full message history.
  - Good for contextual prompting (like "remind me what I said earlier").

```java
    @Bean
    public ChatClient chatClient(ChatModel model, ChatMemory memory) {
        return ChatClient.builder(model)
            .defaultAdvisors(new PromptChatMemoryAdvisor(memory))
            .build();
    }

```

## ➡️ PromptChatMemoryAdvisor vs MessageChatMemoryAdvisor

| **Feature**                 | **PromptChatMemoryAdvisor**                                                                              | **MessageChatMemoryAdvisor**                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Context format**          | Injects conversation history directly into the **prompt string**                                         | Maintains history as structured **message objects** (`user`, `assistant`, `system`)           |
| **AI input style**          | Suited for **completion-style models** (e.g., `text-davinci-003`, older LLMs that expect raw text input) | Designed for **chat-style models** (e.g., GPT-4, Gemini, Claude) that use role-based messages |
| **Memory storage behavior** | Stores summaries or key context snippets                                                                 | Stores the entire sequence of messages                                                        |
| **Use case**                | Ideal for **simple prompt-driven tasks** (e.g., summarization, sentiment analysis)                       | Ideal for **multi-turn conversations** (e.g., chatbots, assistants)                           |
| **Prompt template needed**  | ✅ Required (you need to embed past context manually)                                                    | ❌ Not required — message history is managed automatically                                    |
| **Ease of multi-turn chat** | 🧩 Manual formatting and prompt stitching needed                                                         | ⚡ Seamless multi-turn memory via message roles                                               |
| **Token usage**             | More efficient (shorter context, summarized)                                                             | Higher token cost (stores full chat history)                                                  |
| **Best for**                | Lightweight apps, constrained token budgets                                                              | Persistent chatbots and assistants                                                            |
| **Example advisor**         | `new PromptChatMemoryAdvisor(memory)`                                                                    | `new MessageChatMemoryAdvisor(memory)`                                                        |

## ➡️ Safeguard Advisor

- Ensures content safety and policy compliance.
- It filters or rejects unsafe, toxic, or prompt-injection-like content before sending to the model.

```java
 @Bean
public ChatClient secureChatClient(ChatModel model) {
    return ChatClient.builder(model)
        .defaultAdvisors(new SafeGuardAdvisor())
        .build();
}

```

- When you’re building public-facing apps (chatbots, customer assistants), this prevents:
  - Leaking sensitive info
  - Harmful or offensive content
  - Jailbreak attempts

## ➡️ ReReadingAdvisor

- Allows the model to "re-read" or re-analyze previous messages** to refine answers** or add corrections.
- It acts like a second-pass reasoning layer, improving accuracy and coherence

```java
   @Bean
    public ChatClient rereadingClient(ChatModel model) {
        return ChatClient.builder(model)
            .defaultAdvisors(new ReReadingAdvisor())
            .build();
    }

```

- When your app needs the model to double-check or self-verify its answers before responding — e.g., summarization, legal text, or analysis apps.

### ➡️ MariaDBVectorQAAdvisor

- Connects your AI assistant to a vector database (like MariaDB’s vector store) to enable retrieval-augmented generation (RAG).
- It helps the model fetch relevant documents or embeddings and use them for question-answering.

```java
  @Bean
 public ChatClient vectorQaClient(ChatModel model, VectorStore vectorStore) {
    return ChatClient.builder(model)
        .defaultAdvisors(new MariaDBVectorStoreQAAdvisor(vectorStore))
        .build();
 }

```

- **How it Works:**
  - Converts user query → embedding.
  - Finds similar embeddings in MariaDB Vector Store.
  - Injects retrieved content into the prompt context.
  - Model generates an informed answer.
