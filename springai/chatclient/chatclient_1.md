⏺️ ➡️ 🟦 🔵 🔴 🟢 ⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Chat Client

```java
  org.springframework.ai.chat.client (core ChatClient and builder)
```

## ➡️ Interfaces

### 🟦 ChatClient

### 🟦 ChatResponse

## ➡️ Classes

### 🟦 Prompt

### 🟦 ChatClientResponse

## ➡️ Method

### 🟦 Builder Configuration (`ChatClient.Builder`)

#### 🔵 ChatClient.Builder build():

Finalizes and returns a ChatClient instance.

#### 🔵 ChatClient.Builder defaultSystem(String text):

Sets a global system prompt for all chats.

#### 🔵 ChatClient.Builder defaultUser(Consumer<UserSpec> userSpec):

Configures default user input (supports lambdas for templating).

#### 🔵 ChatClient.Builder defaultOptions(ChatOptions options):

Applies defaults like temperature or model name.

#### 🔵 ChatClient.Builder defaultAdvisors(Advisor... advisors):

Adds global advisors (e.g., logging or memory).

### 🟦 Prompt Building (ChatClient → Prompt)

#### 🔵ChatClient.prompt():

Starts a new prompt builder (fluent chain).

#### 🔵ChatClient.prompt(String content):

Quick-start with user text.

#### 🔵Prompt.user(String text):

Adds a user message.

#### 🔵Prompt.user(Consumer<UserSpec> userSpec):

Builds user message with params (e.g., .param("name", "Grok")).

#### 🔵Prompt.system(Consumer<SystemSpec> systemSpec):

Adds/configures a system message.

#### 🔵Prompt.param(String key, Object value):

Substitutes template placeholders (e.g., {key}).

#### 🔵Prompt.advisors(Consumer<AdvisorSpec> advisorSpec):

Adds runtime advisors (overrides defaults).

### 🟦 Execution (`Prompt`)

#### 🔵 Prompt.call():

Synchronous execution; returns ChatClientResponse.

#### 🔵 Prompt.stream():

Asynchronous streaming; returns Flux<ChatClientResponse> (for real-time UIs).

### 🟦 Response Handling (ChatClientResponse)

#### 🔵 ChatClientResponse.content():

Extracts plain text from the response.

#### 🔵 ChatClientResponse.chatResponse():

Full ChatResponse with metadata (e.g., token usage).

#### 🔵 ChatClientResponse.entity(Class<T> type):

Parses to a POJO/record (e.g., Joke.class).

#### 🔵 ChatClientResponse.entity(ParameterizedTypeReference<T> type):

For generics like List<String>.

#### 🔵 ChatClientResponse.stream().content():

Streams text tokens as Flux<String>.
