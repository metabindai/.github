# Metabind

**The hosted platform for [MCP Apps](https://docs.metabind.ai/guides/concepts/what-is-an-mcp-app).** You define the tools: your UI components become Interactive Tools, your APIs become Data Tools. Metabind generates the MCP server, hosts it, and scales it. Your app runs across Claude, ChatGPT, and every MCP host, and inside your own product through the Assistant SDK.

**Everything that ships inside your app lives in this org, under Apache 2.0**: the BindJS runtime, all three renderers (SwiftUI, Jetpack Compose, React), and all three client SDKs. Nothing proprietary in your client path.

## One definition, three native renderers

Components are written in BindJS, a declarative, SwiftUI-shaped component language:

```typescript
const properties = {
  title: PropertyString({ title: "Title", required: true, defaultValue: "Welcome" }),
  showAction: PropertyBoolean({ title: "Show action", defaultValue: true }),
}

const body = (props, children) =>
  VStack({ spacing: 16 }, [
    Text(props.title)
      .font("headline")
      .foregroundStyle(Color("primary")),

    props.showAction
      ? Button("Get started", () => console.log("Tapped"))
      : Empty(),
  ])

export default defineComponent({
  metadata: { title: "Welcome card", description: "A simple example" },
  properties,
  body,
})
```

The same definition renders as native SwiftUI on iOS, Jetpack Compose on Android, and React on the web. Real platform components on mobile, not web views.

| Surface | Renderer |
|---|---|
| MCP hosts (Claude, ChatGPT, VS Code, Cursor) | [`@metabindai/bindjs-react`](https://github.com/metabindai/bindjs-runtime), sandboxed iframe |
| Your iOS app, via the Assistant SDK | [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) → SwiftUI |
| Your Android app, via the Assistant SDK | [`bindjs-android`](https://github.com/metabindai/bindjs-android) → Jetpack Compose |
| Your web app, via the Assistant SDK | [`@metabindai/bindjs-react`](https://github.com/metabindai/bindjs-runtime) |

## Repositories

| Repo | What it is |
|---|---|
| [`bindjs-runtime`](https://github.com/metabindai/bindjs-runtime) | The BindJS runtime and React renderer. On npm as `@metabindai/bindjs-runtime` and `@metabindai/bindjs-react` |
| [`bindjs-apple`](https://github.com/metabindai/bindjs-apple) | The SwiftUI rendering engine (SwiftPM, product `BindJS`) |
| [`bindjs-android`](https://github.com/metabindai/bindjs-android) | The Jetpack Compose rendering engine (`ai.metabind:bindjs-android`) |
| [`metabind-apple`](https://github.com/metabindai/metabind-apple) | Swift SDK: content client, `MCPAppsHost` (MCP tool results as SwiftUI), and the `MetabindAssistant` drop-in |
| [`metabind-android`](https://github.com/metabindai/metabind-android) | Kotlin SDK: content client and the drop-in Assistant, rendered as Jetpack Compose |
| [`metabind-web`](https://github.com/metabindai/metabind-web) | React SDK: the drop-in Assistant for web apps |

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
