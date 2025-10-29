⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Prompt, PromptTemplate and Messages

## ➡️ Prompt

- The core object that represents a complete request sent to an AI model.
- A Prompt is the main input you send to the AI model.
- It represents the final text (or structured message) that the model will use to generate a response.
- Encapsulates the full input (messages, system instructions, user input, etc.) to be sent to the LLM.

```java
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.ChatClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class SimplePromptExample {

    @Autowired
    private ChatClient chatClient;

    public String getResponse() {
        Prompt prompt = new Prompt("Explain Spring Boot in simple words.");
        return chatClient.call(prompt).getResult().getOutput().getContent();
    }
}

```

- `Prompt` → wraps your plain text question.
- `chatClient.call(prompt)` → sends it to the LLM.
- `.getResult().getOutput().getContent()` → returns the model’s reply.

## ➡️ PromptTemplate

- A templating mechanism to create dynamic Prompt objects using placeholders.
- It allows you to define a prompt with placeholders, and later replace them dynamically with actual values.
- Supports **Spring Expression Language (SpEL)** or simple ${} placeholders.
- Great for building prompts from user input, context, or database data.
- PromtpTemplate implements two **interfaces**
  - **PromtpTemplateMessageActions**
  - **PromtpTemplateActions**

```java
import org.springframework.ai.chat.prompt.PromptTemplate;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.ChatClient;
import org.springframework.stereotype.Service;
import java.util.Map;

@Service
public class PromptTemplateExample {

    private final ChatClient chatClient;

    public PromptTemplateExample(ChatClient chatClient) {
        this.chatClient = chatClient;
    }

    public String getDynamicResponse(String topic) {
        String template = "Explain {topic} in one paragraph for a beginner.";
        PromptTemplate promptTemplate = new PromptTemplate(template);

        Prompt prompt = promptTemplate.create(Map.of("topic", topic));
        return chatClient.call(prompt).getResult().getOutput().getContent();
    }
}

```

- The template is: `"Explain {topic} in one paragraph for a beginner."`
- **{topic}** → placeholder replaced dynamically.
- `PromptTemplate.create()` → generates a final Prompt.

## ➡️ Messages

- A Message represents a structured conversation between roles like:
- You use them when you need multi-turn or role-based communication (like chatbots).

### 🟦 System Role

- `system` → sets context/instructions for the AI.
- Class `SystemMessage`

### 🟦 User Role

- `user` → user inputs/questions.
- Class `UserMessage`

### 🟦 Assistant Role

- `assistant` → AI responses.
- Class `AssistantMessage`

### 🟦 Tool/Function Role

- Result from function call
- Class `ToolResponseMessage`

```java
import org.springframework.ai.chat.messages.*;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.ChatClient;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class MessagesExample {

    private final ChatClient chatClient;

    public MessagesExample(ChatClient chatClient) {
        this.chatClient = chatClient;
    }

    public String runChat() {
        SystemMessage system = new SystemMessage("You are a helpful assistant who explains technical topics.");
        UserMessage user = new UserMessage("What is dependency injection in Spring?");

        Prompt prompt = new Prompt(List.of(system, user));

        return chatClient.call(prompt).getResult().getOutput().getContent();
    }
}

```

- `SystemMessage` → sets tone/behavior of the AI.
- `UserMessage` → user’s question.
- `Prompt(List.of(...))` → sends multiple messages as one context.

### ➡️ Combined Usage Example

```java
String template = """
You are an expert in {domain}.
Explain {topic} in simple words for a new developer.
""";

PromptTemplate promptTemplate = new PromptTemplate(template);

Prompt prompt = promptTemplate.create(Map.of(
    "domain", "Spring Boot",
    "topic", "Dependency Injection"
));

String response = chatClient.call(prompt)
                            .getResult()
                            .getOutput()
                            .getContent();

System.out.println(response);


```
