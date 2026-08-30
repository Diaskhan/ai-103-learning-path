# Develop an AI agent with Microsoft Agent Framework

# Introduction

AI agents use generative AI to interpret data, make decisions, and complete tasks with minimal human intervention—making them well suited for automating complex workflows. To build them effectively, developers need a framework that balances ease of use with enterprise-grade reliability.

The **Microsoft Agent Framework** is the next generation of both Semantic Kernel and AutoGen, built by the same teams. It combines AutoGen's straightforward agent abstractions with Semantic Kernel's enterprise features—session-based state management, type safety, middleware, and telemetry—and adds graph-based workflows for explicit multi-agent orchestration. The result is a flexible, production-ready SDK for building single-agent and multi-agent solutions.

This module focuses on using the Microsoft Agent Framework with Microsoft Foundry Agent Service to build AI agents. Suppose you need to develop an agent that extracts data from submitted expense reports, formats them correctly, and emails them to the appropriate recipients. The Agent Framework's tool integration and session management features make this kind of workflow straightforward to implement.

In this module, you learn about the core features of the Microsoft Agent Framework SDK and how to create AI agents with tool functions.

After completing this module, you're able to:

- Use the Microsoft Agent Framework to connect to a Microsoft Foundry project.
- Create Microsoft Foundry agents using the Microsoft Agent Framework.
- Integrate tool functions with your AI agent.

Note

We recognize that different people like to learn in different ways. You can choose to complete this module in video-based format or you can read the content as text and images. The text contains greater detail than the videos, so in some cases you might want to refer to it as supplemental material to the video presentation.

---

# Understand Microsoft Agent Framework AI agents

The **Microsoft Agent Framework** is the next generation of both Semantic Kernel and AutoGen, built by the same engineering teams. It combines AutoGen's intuitive agent abstractions with Semantic Kernel's enterprise-grade features—including session-based state management, type safety, execution filters, and telemetry.

The framework introduces graph-based workflows to give developers explicit control over multi-agent execution paths. Every agent is derived from a unified `Agent` base class, giving you a consistent interface regardless of which underlying model provider you use.

## Architecture and key features

Rather than requiring you to manually wire together separate libraries for memory, tool integration, and model access, the Microsoft Agent Framework bundles these components into a set of composable building blocks. You can use them individually or combine them as your solution grows in complexity.

| Feature | Description |
| --- | --- |
| **Model clients** | A single, unified interface to connect with multiple AI providers |
| **Agent session** | Native state management for persistent conversation context across multi-turn interactions |
| **Context providers** | Plug-and-play memory components that surface relevant information to agents dynamically |
| **Function tools** | Custom functions automatically registered with agents, with schema generation handled by the framework |
| **MCP clients** | Built-in support for the Model Context Protocol, enabling dynamic tool discovery at runtime |
| **Middleware** | Hooks to intercept, log, or modify agent actions before and after execution |
| **Workflow orchestration** | Graph-based workflows for managing sequential, concurrent, group chat, and agent handoff patterns |

## What agents can do

Because all agents share the same `Agent` base class, you get a consistent set of capabilities regardless of which provider powers your agent. This means you can focus on your application logic rather than adapting to provider-specific APIs.

Out of the box, every agent in the framework supports:

- **Function calling**—automatically invoke registered tools to interact with external APIs and services
- **Multi-turn conversations**—maintain chat history either locally or via service-provided history management
- **Structured outputs**—generate type-safe, schema-validated responses
- **Streaming responses**—receive results incrementally as they're generated
- **Service-provided tools**—use built-in capabilities such as code execution, file search, and web search where supported by the provider

## Using the Microsoft Agent Framework with AI Foundry

The Microsoft Agent Framework is designed to work seamlessly with your Azure AI Foundry projects. It provides a consistent interface for connecting to Foundry, managing agent sessions, and integrating with tools and services.

By authenticating with your Azure credentials, you can connect to your Foundry project and create agents that use the capabilities of the Foundry Agent Service. These capabilities include persistent chat history, dynamic tool discovery, and integration with Azure services.

### Why Foundry is the recommended provider

A key differentiator of the Foundry Agent Service is its support for **service-side chat history**. With service-side history, the agent session persists across turns automatically—you don't need to manage conversation state yourself. Service-side history makes Foundry the recommended provider for production scenarios where maintaining context is critical.

## Provider matrix

One of the practical benefits of the Agent Framework's common interface is provider flexibility. As models improve or your requirements change, you can switch the underlying inference service without rewriting your agent logic—only the client configuration changes.

The framework supports the following providers:

| Provider | Service chat history |
| --- | --- |
| Foundry Agent Service | Yes |
| Azure OpenAI Responses | Yes |
| OpenAI Responses | Yes |
| Azure OpenAI Chat Completion | No |
| OpenAI Chat Completion | No |
| Anthropic Claude | No |
| Amazon Bedrock | No |
| GitHub Copilot | No |
| Ollama (OpenAI-compatible) | No |

This module focuses on the **Foundry Agent Service** provider, which offers enterprise-grade capabilities including persistent chat history, Model Context Protocol (MCP) tool support, and integration with Azure services.

---

# Create an Azure AI agent with Microsoft Agent Framework

The Foundry Agent Service is the recommended provider for production environments built with the Microsoft Agent Framework. It handles persistent conversation history on the service side, supports built-in tools such as code execution and file search, and integrates seamlessly with Azure identity management. These features allow you to focus on your agent's behavior rather than its infrastructure overhead.

## Configuring a Foundry agent

Creating and interacting with a Foundry agent follows a consistent sequence of steps.

### 1. Set up your Foundry project

Before writing any code, you need a Microsoft Foundry project with a deployed model. You connect to your project using two pieces of information:

- **Project endpoint**—the URL of your Foundry project.
- **Model deployment name**—the name of the model deployment you want to use for your agent.

### 2. Configure authentication

The Agent Framework connects to your Foundry project using Azure credentials. In most scenarios, `DefaultAzureCredential` resolves the right credential automatically based on your environment—Azure CLI during development, managed identity in production. No connection strings or API keys need to be hardcoded.

### 3. Initialize the Foundry chat client

Create a Foundry chat client by providing your credentials, project endpoint, and model name. This client is the bridge between your application and the Foundry Agent Service. It handles authentication, request routing, and service-side session management.

### 4. Create the agent

Using the chat client, create an agent by providing a set of instructions that define its behavior:

- **Instructions**—the system prompt that defines the agent's role, goals, and constraints
- **Tools** *(optional)*—Custom functions the agent can call to take actions or retrieve information

The framework registers any tools you provide and automatically generates their schemas, so the model knows when and how to invoke them.

### 5. Establish a session and run the agent

To begin interacting, you open a **session** via the agent instance. The session acts as the container for the conversation state. You send user messages to the session's execution method, which processes the prompt, coordinates any necessary tool calls, and returns the model's response.

## Multi-turn conversations

A single call to the agent's run method handles one exchange—one user message, one response. For a real conversation, you need the agent to remember what was said in earlier turns. That's what a **session** is for.

For the Foundry provider, the sessions are backed by **service-side storage**—the conversation history lives in the Foundry Agent Service rather than in your application's memory.

- **Persistent history**—Because the state lives on the service side, a user's conversation can continue across multiple requests, even if your application restarts or scales out to multiple instances.
- **Local history**—For providers that don't support service-side history, the framework maintains the conversation state in memory within the session object. Local history is suitable for short-lived or stateless applications, but it doesn't persist across process restarts.

## Nonstreaming vs. streaming responses

The Agent Framework supports two response modes:

- **Non-streaming (synchronous)**—the run method waits for the agent to finish processing and returns a complete response object. Non-streaming is the simplest pattern and works well when you don't need to display output incrementally.
- **Streaming (asynchronous)**—the run method returns a response stream that you iterate over asynchronously, receiving partial updates as the model generates them. Streaming is better suited for user-facing interfaces where showing output progressively improves the experience.

In both cases, the response exposes a `text` property that aggregates all text content from the agent's output, making it straightforward to extract the final answer regardless of which mode you use.

---

# Add tools to Azure AI agent

Tools are what allow your agent to take action—call APIs, execute code, search files, or interact with external services. Without tools, an agent can only generate text based on what it already knows. With tools, it becomes capable of acting on the world.

The Microsoft Agent Framework supports two broad categories of tools: **service-provided tools** that are hosted and managed by the provider, and **custom function tools** that you write yourself and register with the agent.

## Service-provided tools

When using the Foundry provider, a range of hosted tools are available without any extra implementation. You enable them by including them in the agent configuration—the provider handles the actual execution.

The most commonly used service-provided tools include:

| Tool | What it does |
| --- | --- |
| **Code Interpreter** | Executes Python code in a sandboxed environment for calculations and data analysis |
| **File Search** | Searches through and retrieves information from uploaded documents |
| **Web Search** | Retrieves up-to-date information from the internet |
| **Hosted MCP Tools** | MCP (Model Context Protocol) servers invoked directly by the provider runtime |
| **Azure AI Search** | Queries an Azure AI Search index through a Foundry connection |
| **Foundry Toolboxes** | Named, versioned bundles of hosted tool configurations managed in a Foundry project |

Note

Some tools—including Azure AI Search, Bing Grounding, SharePoint, and others—are in preview or experimental. They're available for Foundry agents but may have limited support across other providers.

## Custom function tools

Custom function tools let you extend your agent with any logic you need—calling internal APIs, querying databases, performing calculations, or anything else a Python function can do.

To register a function as a tool, you pass it directly to the agent during creation. The framework inspects the function's signature and generates a schema that tells the model what the function does, what parameters it expects, and what it returns.

For the model to call a tool reliably, your function needs to be clearly described. The Agent Framework supports two approaches:

- **Type annotations with descriptions**—use Python's `Annotated` type with a field description on each parameter. The function's docstring serves as the tool description.
- **The `@tool` decorator**—explicitly specify the tool's name and description as decorator arguments, giving you full control over what the model sees. You can also provide an explicit schema using a Pydantic model if you need precise control over the input structure.

In either case, the framework handles schema generation and tool invocation automatically. When the model determines a tool should be called, the framework executes the function and returns the result back to the model before the final response is generated.

### Adding multiple tools

You can register multiple tools with a single agent. Pass a list of functions when creating the agent, and the model automatically selects the most appropriate tool for each part of the conversation. You don't need to write any routing logic—the framework handles tool orchestration based on the conversation context and the tool descriptions you provide.

## Tool approval

For scenarios where tool invocations should require human review before execution, the Agent Framework supports a **tool approval** pattern. When approval mode is enabled on a tool, the agent pauses before calling that function and requests confirmation. This is useful for actions that are irreversible, expensive, or involve sensitive data. Approval behavior can be configured per tool using the `approval_mode` parameter on the `@tool` decorator.

## Using an agent as a tool

Agents can be composed by using one agent as a tool for another. You convert an inner agent into a function tool and pass it to an outer agent, which can then delegate specific tasks to it. This enables modular designs where specialized agents handle particular domains and a coordinating agent routes requests between them—a pattern explored in more depth in the multi-agent module.

## Best practices for custom tools

- **Write clear descriptions**—the model's ability to choose the right tool depends entirely on the descriptions you provide. Be specific about what the function does and when it should be used.
- **Annotate every parameter**—describe each input so the model can construct valid calls, especially when parameter names alone aren't self-explanatory.
- **Return meaningful data**—tools should return structured, interpretable output. The model uses the return value directly when forming its response.
- **Keep tools focused**—each tool should do one thing well. Combining multiple responsibilities into a single function makes it harder for the model to invoke it correctly.
- **Handle errors gracefully**—if a tool encounters an unexpected input or an external service fails, return an informative error message rather than raising an exception, so the model can respond helpfully.

---

# Summary

In this module, you learned how the Microsoft Agent Framework enables developers to build AI agents. You learned about the components and core concepts of the Microsoft Agent Framework. You also learned how to create custom tools to extend your agent's capabilities. By applying these concepts and skills, you can use the Microsoft Agent Framework to create dynamic, adaptable AI solutions that enhance user interactions and automate complex tasks.
