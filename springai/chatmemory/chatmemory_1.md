⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Chat Memory

- Spring AI provides mechanisms to store and retrieve chat memory, enabling large language models (LLMs) to maintain context across multiple interactions by persisting conversation state and history.
- Spring AI addresses the **stateless** nature of LLMs by offering chat memory features that retain information throughout a conversation. This helps maintain contextual awareness and enables the model to produce coherent, contextually appropriate responses across several turns.

## ➡️ Components of Chat Memory

### 🟦 Chat Memory:

Stores key information to maintain context within conversations, allowing the model to reference previous exchanges for continuity.​

### 🟦 Chat History:

Retains the entire sequence of messages exchanged, forming the conversation log for retrieval and review.

## ➡️ Memory Implementation Types

- Spring AI allows developers to choose different storage backends for chat memory, based on scalability, durability, and relationship modeling needs

### 🟦 InMemoryChatMemory:

- Keeps conversation data within application memory; best for short-term, fast access where persistence is unnecessary.

### 🟦 CassandraChatMemory:

- Saves conversations in a Cassandra NoSQL database, supporting automatic expiration through TTL and suited to high-availability scenarios.

### 🟦 JdbcChatMemory:

- Persists conversations in a relational (SQL) database, making it appropriate for applications needing session durability.

### 🟦 Neo4jChatMemory:

Stores conversations in a Neo4j graph database, allowing for advanced relationship and graph queries over conversation data.

## ➡️ How Spring AI Chat Managed Chat Memory

- The ChatMemory interface provides standard methods for manipulating chat history within Spring AI:​

```java
add(conversationId, message): Adds a single message in the converstation
add(conversationId, List<messages>): Adds multiple messages at once in the converstation
get(conversationId, lastN): Retrieves the last N messages from the conversation.
clear(conversationId): Deletes all messages for a given conversation.
```
