# Metabind

**The hosted platform for [MCP Apps](https://docs.metabind.ai/guides/concepts/what-is-an-mcp-app).** You define the tools: your UI components become Interactive Tools, your APIs become Data Tools. Metabind generates the MCP server, hosts it, and scales it. Your app runs across Claude, ChatGPT, and every MCP host, and inside your own product through the Assistant SDK.

**Everything that ships inside your app lives in this org, under Apache 2.0**: the BindJS runtime, all three renderers (SwiftUI, Jetpack Compose, React), and all three client SDKs. Nothing proprietary in your client path.

[**metabind.ai**](https://metabind.ai) (live demo) · [**Documentation**](https://docs.metabind.ai) · [**Your first MCP App**](https://docs.metabind.ai/guides/getting-started/your-first-mcp-app) · [**BindJS reference**](https://docs.metabind.ai/bindjs/introduction)

---

## BindJS: one definition, three renderers

BindJS is the declarative, SwiftUI-shaped component language behind every Interactive Tool: 60+ components, 40+ modifiers, typed props generated from a property schema, and validation on every render. You write a component once. Each platform's renderer reads the same compiled bundle and renders real platform components, not web views on mobile.

| Repo | Renders | Packages |
|---|---|---|
| [`bindjs-runtime`](https://github.com/metabindai/bindjs-runtime) | React, in MCP hosts (Claude, ChatGPT, VS Code, Cursor) via sandboxed iframe, and in your web app | npm: `@metabindai/bindjs-runtime`, `@metabindai/bindjs-react` |
| [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) | Native SwiftUI in your iOS, macOS, or visionOS app | SwiftPM, product `BindJS` |
| [`bindjs-android`](https://github.com/metabindai/bindjs-android) | Jetpack Compose in your Android app | `ai.metabind:bindjs-android` |

Start with the [quickstart](https://docs.metabind.ai/bindjs/quickstart), or read the [language reference](https://docs.metabind.ai/bindjs/introduction).

## The Assistant SDK

The embed path. A governed agent runs inside your product, calling tools you built in Metabind, rendered with your components on your typography and color. It follows your project instructions and renders only your approved components. Your users never leave your app.

```swift
import MetabindAI

let assistant = MetabindAssistant(
  serverURL: URL(string: "https://mcp.metabind.ai/my-org/my-project")!,
  serverHeaders: ["Authorization": "Bearer \(projectToken)"],
  provider: MetabindAgentProvider(
    apiKey: projectToken,
    orgId: "my-org",
    projectId: "my-project"
  )
)

// Anywhere in your app:
MetabindAssistantView(assistant: assistant)
```

Same model on every platform: `MetabindAssistantView` in SwiftUI and Compose, `<AgentChat />` in React. Conversation UI, streaming, and Interactive Tool rendering are handled for you, respecting your app's color scheme, dynamic type, and accessibility settings.

| Platform | Repo | Package | Working example |
|---|---|---|---|
| iOS / macOS / visionOS | [`metabind-apple`](https://github.com/metabindai/metabind-apple) | `MetabindAI` (SwiftPM product) | [Assistant demo](https://github.com/metabindai/metabind-apple/tree/main/Samples/MetabindAI/AssistantDemo) |
| Android | [`metabind-android`](https://github.com/metabindai/metabind-android) | `ai.metabind:metabindai-android` | [Assistant demo](https://github.com/metabindai/metabind-android/tree/main/samples/assistant-demo) |
| Web | [`metabind-web`](https://github.com/metabindai/metabind-web) | `@metabindai/agent-ui` | [Example app](https://github.com/metabindai/metabind-web/tree/main/examples/example-metabind-react-app) |

Each SDK also ships the content layer (GraphQL client, caching, real-time updates), and the libraries are independently adoptable. LLM access goes through the Metabind Agent proxy by default: the key stays server-side, and every call lands in your tool-call analytics. Guide: [Embed an assistant](https://docs.metabind.ai/guides/getting-started/embed-an-assistant).

## CLI

The `metabind` CLI is how you, or your coding agent (Claude Code, Cursor, Codex), author tools, validate BindJS locally, and publish:

```sh
brew install metabindai/tap/metabind
```

## Open and hosted

Apache 2.0, in this org: the component language, the runtime, every renderer, every client SDK. The hosted platform is the commercial product: MCP App Studio, the generated MCP server, the agent proxy, governance, analytics. The protocol path is standard MCP and standard JSON Schema, and tool definitions, schemas, and components are exportable.

---

**Make your app think.** [Start free](https://metabind.ai). No credit card. No sales call. No server to run.
