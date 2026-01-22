# Building Agentic Apps in Swift

- Swift packages
  - [Swift AI](https://github.com/DePasqualeOrg/swift-ai)
    - API clients for Anthropic, Gemini, OpenAI, xAI, and more with tool use and MCP integration
    - Abstraction layer across all providers: simplifies usage and makes it easy to swap models
  - [Swift MCP](https://github.com/DePasqualeOrg/swift-mcp)
    - Full-featured Swift SDK for Model Context Protocol servers and clients
    - High-level abstractions make it easy to use MCP
- Resources
  - [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro): A standardized way for AI-enabled apps to integrate with tools, prompts, and resources
- Example apps
  - [iMCP](https://github.com/mattt/iMCP): A macOS app that provides an MCP server for Messages, Contacts, Reminders, and more
  - Coming soon: my own MCP app with native Apple tools and UI automation

## The Agentic Loop

Tools are exposed to the model along with the user's input. If any tools are relevant to the user's input, the model can request that the client execute them. After execution, the results of the tool calls are presented to the model, which evaluates the results and can continue requesting tool calls in a loop until a final result is reached. Finally, the model presents the result to the user.

```
User message
     ↓
┌─→ LLM API call (with available tools)
│        ↓
│   Response contains tool_use?
│      ↓           ↓
│     Yes          No → Return final text to user
│      ↓
│   Call tools
│      ↓
│   Add tool results to conversation
│      ↓
└──────┘
```

## Swift MCP Use Cases

- Client only (HTTP or stdio)

  - Give a model access to tools from a third-party MCP server (local or remote) in an iOS or macOS app
- Server only (HTTP or stdio)

  - Generic use case: expose tools, resources, etc. to clients
    - Ideal if you prefer to use Swift
    - Can run on Apple platforms and Linux
  - Expose native Apple tools on a Mac for use locally, on a local network, or through a VPN like Tailscale
- Client and server in same process (in-memory transport)

  - Expose native Apple tools to a model in an app on any Apple device

  - Works on iOS, where HTTP and stdio are not an option
