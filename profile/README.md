# Metabind

**The hosted platform for [MCP Apps](https://docs.metabind.ai/guides/concepts/what-is-an-mcp-app).** You define the tools: your UI components become Interactive Tools, your APIs become Data Tools. Metabind generates the MCP server, hosts it, and scales it. Your app runs across Claude, ChatGPT, and every MCP host, and inside your own product through the Assistant SDK.

**Everything that ships inside your app lives in this org, under Apache 2.0**: the BindJS runtime, all three renderers (SwiftUI, Jetpack Compose, React), and all three client SDKs. Nothing proprietary in your client path.

---

## The BindJS engine

Components are written once in BindJS, a declarative, SwiftUI-shaped component language. This is what the agent renders when a customer asks about a product:

```typescript
const properties = {
  name: PropertyString({ title: "Name", required: true, defaultValue: "Trail Runner 2" }),
  price: PropertyString({ title: "Price", required: true, defaultValue: "$129" }),
  imageUrl: PropertyString({ title: "Image URL", required: true }),
  inStock: PropertyBoolean({ title: "In stock", defaultValue: true }),
}

const body = (props) =>
  VStack({ spacing: 12, alignment: "leading" }, [
    Image({ url: props.imageUrl, contentMode: "fill" })
      .frame({ height: 180 })
      .cornerRadius(16),

    Text(props.name).font("headline"),

    HStack({ spacing: 8 }, [
      Text(props.price).font("title3").fontWeight("bold"),
      props.inStock
        ? Text("In stock").font("caption").foregroundStyle(Color("green"))
        : Text("Sold out").font("caption").foregroundStyle(Color("secondary")),
    ]),

    Button("Add to cart", () => console.log("add to cart:", props.name)),
  ]).padding(16)

export default defineComponent({
  metadata: {
    title: "Product card",
    description: "Rendered inline when the agent answers a product question",
  },
  properties,
  body,
})
```

The same definition renders as real platform components on every surface. Not web views on mobile: the schema generates typed `props`, validates every render, and each renderer reads the same compiled bundle.

| Surface | Renderer |
|---|---|
| MCP hosts (Claude, ChatGPT, VS Code, Cursor) | [`@metabindai/bindjs-react`](https://github.com/metabindai/bindjs-runtime) in a sandboxed iframe |
| Your iOS app | [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) → native SwiftUI |
| Your Android app | [`bindjs-android`](https://github.com/metabindai/bindjs-android) → Jetpack Compose |
| Your web app | [`@metabindai/bindjs-react`](https://github.com/metabindai/bindjs-runtime) → React |

| Repo | What it is |
|---|---|
| [`bindjs-runtime`](https://github.com/metabindai/bindjs-runtime) | The canonical BindJS runtime and React renderer. On npm as `@metabindai/bindjs-runtime` and `@metabindai/bindjs-react` |
| [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) | The SwiftUI rendering engine (SwiftPM, product `BindJS`) |
| [`bindjs-android`](https://github.com/metabindai/bindjs-android) | The Jetpack Compose rendering engine (`ai.metabind:bindjs-android`) |

Full language reference: [BindJS docs](https://docs.metabind.ai/bindjs/introduction) — 60+ components, 40+ modifiers, state, hooks, and styles.

---

## The Assistant SDK

The embed path. A governed agent runs **inside your product**, calling tools you built in Metabind, rendered with your components on your typography and color. It follows your project instructions and renders only your approved components — your users never leave your app.

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

`MetabindAssistantView` renders the conversation, streams responses, and renders Interactive Tool output as native SwiftUI inline — respecting your app's color scheme, dynamic type, and accessibility settings. Same model on every platform: `MetabindAssistantView` in Compose, `<AgentChat />` in React.

| Platform | Package | Renders via | Working example |
|---|---|---|---|
| iOS / macOS / visionOS | `MetabindAI`, a product of [`metabind-apple`](https://github.com/metabindai/metabind-apple) | SwiftUI (`bindjs-apple`) | [Assistant demo](https://github.com/metabindai/metabind-apple/tree/main/Samples/MetabindAI/AssistantDemo) |
| Android | `ai.metabind:metabindai-android` from [`metabind-android`](https://github.com/metabindai/metabind-android) | Jetpack Compose (`bindjs-android`) | [Assistant demo](https://github.com/metabindai/metabind-android/tree/main/samples/assistant-demo) |
| Web | [`@metabindai/agent-ui`](https://github.com/metabindai/metabind-web) from `metabind-web` | React | [Example app](https://github.com/metabindai/metabind-web/tree/main/examples/example-metabind-react-app) |

The SDKs also ship the content layer (GraphQL client, caching, real-time updates) — each library is independently adoptable. LLM access goes through the Metabind Agent proxy by default: the key stays server-side, and every call lands in your tool-call analytics. Guide: [Embed an assistant](https://docs.metabind.ai/guides/getting-started/embed-an-assistant).

---

## CLI

The `metabind` CLI is how you, or your coding agent (Claude Code, Cursor, Codex), author tools, validate BindJS locally, and publish:

```sh
brew install metabindai/tap/metabind
```

## Open and hosted

Apache 2.0, in this org: the component language, the runtime, every renderer, every client SDK. The hosted platform is the commercial product: MCP App Studio, the generated MCP server, the agent proxy, governance, analytics. The protocol path is standard MCP and standard JSON Schema, and tool definitions, schemas, and components are exportable.

## Start here

- [metabind.ai](https://metabind.ai) — the live demo. Free to start, no credit card.
- [Your first MCP App](https://docs.metabind.ai/guides/getting-started/your-first-mcp-app)
- [Documentation](https://docs.metabind.ai) · [BindJS reference](https://docs.metabind.ai/bindjs/introduction)
