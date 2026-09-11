# Metabind

**The hosted platform for [MCP Apps](https://docs.metabind.ai/guides/concepts/what-is-an-mcp-app).** Metabind builds agents that answer in your product's own UI, not in a chat window: interactive interfaces that take customers straight to what they came for, in the brand they already know. It's built from the UI, design system, and APIs the app already has. No rewrite. It's governed, rendering only components you've approved, enforced on every render. And it's hosted: you define the tools, we run the server. The same agent runs inside your own iOS, Android, and web apps, and across Claude, ChatGPT, and every MCP host, on the open [MCP](https://modelcontextprotocol.io) standard.

**Everything that ships inside your app lives in this org, under Apache 2.0**: the BindJS Specification, its runtime and all three renderers (SwiftUI, Jetpack Compose, React), the A2UI renderer built on it, all three client SDKs, and the demo apps. Nothing proprietary in your client path.

[**metabind.ai**](https://metabind.ai) (live demo) · [**Documentation**](https://docs.metabind.ai) · [**Your first MCP App**](https://docs.metabind.ai/guides/getting-started/your-first-mcp-app) · [**BindJS Specification**](https://github.com/metabindai/bindjs)

---

## BindJS: the open component language for agent UI

Write a UI component once, with its logic, and BindJS renders it as native SwiftUI, Jetpack Compose, and React, wherever an agent renders UI: as an [MCP Apps](https://github.com/modelcontextprotocol/ext-apps) View, as an A2UI catalog, or inside an in-app assistant. A component is written in JavaScript against a SwiftUI-shaped API; a runtime executes it in an isolated context and emits a JSON view tree, and a renderer on each platform draws that tree with real platform widgets. Typed props come from the component's property schema, and every render is validated against it.

| Repo | What it is | Packages |
|---|---|---|
| [`bindjs`](https://github.com/metabindai/bindjs) | The BindJS Specification 1.0: the language, the MCP Apps and A2UI bindings, conformance statements, and the BEP change process | Specification (Markdown) and `types/bindjs.d.ts` |
| [`bindjs-runtime`](https://github.com/metabindai/bindjs-runtime) | React, in MCP hosts (Claude, ChatGPT, VS Code, Cursor) via sandboxed iframe, and in your web app | npm: `@metabindai/bindjs-runtime`, `@metabindai/bindjs-react` |
| [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) | Native SwiftUI in your iOS, macOS, or visionOS app | SwiftPM, product `BindJS` |
| [`bindjs-android`](https://github.com/metabindai/bindjs-android) | Jetpack Compose in your Android app | `ai.metabind:bindjs-android` |
| [`a2ui-bindjs`](https://github.com/metabindai/a2ui-bindjs) | An [A2UI](https://a2ui.org) renderer built on BindJS: one catalog source, rendered natively as React, SwiftUI, and Jetpack Compose, plus A2UI over MCP | `@metabindai/a2ui-bindjs`, `@metabindai/a2ui-bindjs-react`, Swift and Android packages |

Start with the [quickstart](https://docs.metabind.ai/bindjs/quickstart), read the [language reference](https://docs.metabind.ai/bindjs/introduction), or go straight to the [specification](https://github.com/metabindai/bindjs).

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

## Demos

[`metabind-demos`](https://github.com/metabindai/metabind-demos) holds product-style apps built on the SDKs. Each demo pairs native iOS and Android clients with the MCP project they talk to, so you can install the project into your own Metabind organization and run the same assistant in a real app on your own devices.

## CLI

The `metabind` CLI is how you, or your coding agent (Claude Code, Cursor, Codex), author tools, validate BindJS locally, and publish:

```bash
brew install metabindai/tap/metabind
```

## Open and hosted

Apache 2.0, in this org: the specification, the component language and its runtime, every renderer, every client SDK, the A2UI renderer, and the demos. The hosted platform is the commercial product: Metabind Studio, the generated MCP server, the agent proxy, governance, analytics. The protocol path is standard MCP and standard JSON Schema, and tool definitions, schemas, and components are exportable.

---

**Make your app think.** **[Start free at metabind.ai](https://www.metabind.ai/signup)** · **[Read the docs](https://docs.metabind.ai)**. No credit card. No sales call. No server to run.
