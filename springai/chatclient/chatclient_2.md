⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

## ➡️ Three Ways To Instantiate A ChatClient

### 🟦 Builder Pattern (Spring-Managed)

- Use when you need custom advisors, system prompts, or configuration.

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.openai.OpenAiChatModel;

@Service
public class AiService {

    private final ChatClient chatClient;

    public AiService(OpenAiChatModel chatModel) {
        this.chatClient = ChatClient.builder(chatModel)
                .defaultAdvisors(...) // optional
                .build();
    }

    public String summarize(String text) {
        return chatClient.prompt()
                .user("Summarize: " + text)
                .call()
                .content();
    }
}
```

### 🟦 Direct Autowiring (Recommended for Simplicity)

- Zero boilerplate.
- Auto-configured from `spring.ai.\*` properties.
- Works out-of-the-box with any supported provider.

```java
  @Service
public class AiService {

    private final ChatClient chatClient;

    public AiService(ChatClient chatClient) { // Direct injection!
        this.chatClient = chatClient;
    }

    public String chat(String input) {
        return chatClient.prompt(input).call().content();
    }
}
```

### 🟦 Static Factory Method

- Use when you want explicit control over model injection.

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.openai.OpenAiChatModel;

@Service
public class AiService {

    private final ChatClient chatClient;

    public AiService(OpenAiChatModel chatModel) {
        this.chatClient = ChatClient.create(chatModel); // Static factory
    }

    public String ask(String question) {
        return chatClient.prompt()
                .user(question)
                .call()
                .content();
    }
}
```
