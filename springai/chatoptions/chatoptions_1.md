⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ ChatOptions

- In Spring AI, the ChatOptions interface (not a class) provides a portable set of configuration options for chat-based AI models.
- **ChatOptions** promotes interoperability across different AI providers (**e.g.**, `OpenAI`, `Mistral AI`) by standardizing key settings. For provider-specific features, Spring AI offers implementations like `OpenAiChatOptions` or `MistralAiChatOptions`, which extend or implement **ChatOptions**.

## ➡️ Key Methods and Parameters

```java
   public interface ChatOptions extends ModelOptions {
    String getModel();                  // The AI model ID (e.g., "gpt-4o-mini")
    Float getFrequencyPenalty();        // Penalty for repeating tokens (-2.0 to 2.0; default often 0.0)
    Integer getMaxTokens();             // Max tokens in the response (e.g., 100-1000)
    Float getPresencePenalty();         // Penalty for introducing new topics (-2.0 to 2.0; default often 0.0)
    List<String> getStopSequences();    // Sequences to stop generation (e.g., ["\n\n"])
    Float getTemperature();             // Controls randomness (0.0-2.0; lower = more deterministic)
    Integer getTopK();                  // Limits token sampling to top K (e.g., 40)
    Float getTopP();                    // Nucleus sampling threshold (0.0-1.0; e.g., 0.9)
    ChatOptions copy();                 // Creates a mutable copy for modifications
}
```

## ➡️ Relevant Chat Options in Spring AI

- **Temperature:** Adjusts the randomness or creativity of the output. Lower values make results more deterministic; higher values introduce more variety.​
- **Max Tokens:** Sets the maximum length for generated responses, measured in tokens (words or word pieces).​
- **Frequency Penalty:** Discourages the model from repeating tokens that have already appeared in the output, promoting varied responses.​
- **Presence Penalty:** Prevents the model from repeating words even if used just once, encouraging new content generation.​
- **TopP (Nucleus Sampling):** Determines diversity by considering a dynamic subset of likely next tokens (higher values give more diversity).​
- **TopK:** Considers a fixed number of most likely tokens when selecting the next token in a response.​
- **Stop Sequence:** Stops generating text when a specific sequence appears, useful for controlling the end of generated output.

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.model.ChatOptions;
import org.springframework.ai.openai.OpenAiChatOptions;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ChatService {

    @Autowired
    private ChatClient chatClient;

    public String generateResponse(String question) {
        ChatOptions options = OpenAiChatOptions.builder()
                .withModel("gpt-4-turbo")      // specify which model to use
                .withTemperature(0.7)          // creativity level
                .withMaxTokens(500)            // response length limit
                .build();

        return chatClient.prompt()
                .user(question)
                .options(options)
                .call()
                .content();
    }
}

```

### 🟦

##### 🔵
