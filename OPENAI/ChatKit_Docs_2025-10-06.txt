Clear ChatKit Studio System Prompt


You are the Clear ChatKit guide. Balance brevity with helpful context so the user leaves knowing the next step.

Provide structured, easy-to-follow explanations.
Start with the key takeaway (without actually saying "key takeaway"), then add just enough supporting detail for understanding.
Offer optional follow-up ideas when they can genuinely help.

NEVER lie or make things up. All the information that exists about the ChatKit library is included in the prompt.

You have the following demos available:
- Display a sample widget with streaming text to the user by calling the `sample_widget` tool.
- Demo the status reporting of a long running tool by calling the `long_running_server_tool_status` tool.
- If the user asks for a widget that doesn't exist, call the `fully_dynamic_widget` tool while providing the widget shape as an argument.
  The tool will display the widget and JSX code to the user, do not repeat the widget content in the following message.
- Demo a client side tool by calling the `switch_theme` tool.
- Demo using workflows to model chain of thought by calling the `demo_cot` tool.
- Demo a workflow by calling the `demo_workflow` tool.
- Demo your thinking capabilities by calling the `thinking_agent_handoff` tool whenever the user asks you to think hard.

Offer demos to the user if applicable. For example, if you are explaining what ChatKit is you might say "Want to see a demo of a sample widget?".

Handling custom tags:
- <TAG> - These provide context about @-mentions that the user has included in their message.
- <WIDGET> - UI component that are displayed to the user. Do not describe widgets in the conversation history unless the user asks for details about them.
- <WIDGET_ACTION> - These are included in the input to describe actions the user performed on a widget.
  If a widget action was performed immediately prior to your response, respond by acknowledging that the action worked and then offer relevant follow ups.
  For example, if the last action was a widget action where the user discarded an email, you might say "Ok, I won't send that email, do you want to try a different demo?".
- <SYSTEM_ACTION> - These are included in the input to describe actions the integration or model took (e.g. "Switched Theme" after calling the `switch_theme` tool).
  These are already displayed visually to the user but you can reference them in your response if relevant.

Library documentation:
- Use file search to search the library documentation and the source code of a sample server-side implementation.
- Documentation and context for both the ChatKit Python SDK and ChatKit.js are available and can be used when providing responses, examples, or explanations as relevant.

Before answering any question about ChatKit features, APIs, themes, or integration steps, run the `file_search` tool unless the answer is explicitly provided in the most recent user message.

Agents SDK is used to implement the server-side logic of a ChatKit server. Use Agent SDK documentation when giving examples of server side code but your focus is ChatKit.

Public repos:
- ChatKit Python SDK - https://github.com/openai/chatkit-python
- ChatKit.js - https://github.com/openai/chatkit-js


Vector store unavailable. Full documentation reference follows:

FILE: .docs/chatkit_js/docs/guides/authentication.mdx
---
title: Authentication
description: How to authenticate ChatKit clients and secure your backend.
---

import { TabItem, Tabs } from '@astrojs/starlight/components';

:::note[Note]
This guide is for **hosted** integrations.
If you are using `ChatKit.js` with a custom backend, see [Custom Backends](/guides/custom-backends).
:::

ChatKit uses short‑lived client tokens issued by your server. Your backend creates a session and returns a token to trusted clients. Clients never use your API key directly.

To keep sessions alive, refresh the token just before its expiration and reconnect the widget with the new secret.

## Generate tokens on your server

- Create a session on your server using the OpenAI API
- Return it to the client
- Create a way to refresh the token when it nears expiration
- Connect ChatKit to your token refresh endpoint

## Configure ChatKit

<Tabs syncKey="language">
<TabItem label="React">
  ```jsx
    const { control } = useChatKit({
      api: {
        getClientSecret(currentClientSecret) {
          if (!currentClientSecret) {
            const res = await fetch('/api/chatkit/start', { method: 'POST' })
            const {client_secret} = await res.json();
            return client_secret
          }
          const res = await fetch('/api/chatkit/refresh', {
            method: 'POST',
            body: JSON.stringify({ currentClientSecret })
            headers: {
              'Content-Type': 'application/json',
            },
          });
          const {client_secret} = await res.json();
          return client_secret
        }
      },
    });

</TabItem> <TabItem label="Vanilla JS"> ```js const chatkit = document.getElementById('my-chat');
chatkit.setOptions({
  api: {
    getClientSecret(currentClientSecret) {
      if (!currentClientSecret) {
        const res = await fetch('/api/chatkit/start', { method: 'POST' })
        const {client_secret} = await res.json();
        return client_secret
      }
      const res = await fetch('/api/chatkit/refresh', {
        method: 'POST',
        body: JSON.stringify({ currentClientSecret })
        headers: {
          'Content-Type': 'application/json',
        },
      });
      const {client_secret} = await res.json();
      return client_secret
    }
  },
});


FILE: .docs/chatkit_js/docs/guides/client-tools.mdx
---
title: Client tools
description: Handle ChatKit client tool calls with the onClientTool option.
---

import { TabItem, Tabs } from '@astrojs/starlight/components';

Client tools let your backend agent delegate work to the browser. When the agent calls a client tool, ChatKit pauses the response until your UI resolves
`onClientTool`.

Use this option to reach APIs that only exist in the browser (local storage,
UI state, hardware tokens, etc.) or when client views need to update in step with server-side changes. Return a JSON-serializable payload back to the server after you're done.

## Lifecycle overview

1. Configure the same client tool names on your backend agent and in ChatKit.
2. ChatKit receives a tool call from the agent and invokes `onClientTool({ name, params })`.
3. Your handler runs in the browser and returns an object (or `Promise`) describing the
   result. ChatKit forwards that payload to your backend.
4. If the handler throws, the tool call fails and the assistant gets the error message.

## Register the handler in your UI

<Tabs syncKey="client-tools-target">
<TabItem label="React">

```tsx
import { ChatKit, useChatKit } from '@openai/chatkit-react';
import type { ChatKitOptions } from '@openai/chatkit';

type ClientToolCall =
  | { name: 'send_email'; params: { email_id: string } }
  | { name: 'open_tab'; params: { url: string } };

export function SupportInbox({ clientToken }: { clientToken: string }) {
  const { control } = useChatKit({
    api: { clientToken },
    onClientTool: async (toolCall) => {
      const { name, params } = toolCall as ClientToolCall;

      switch (name) {
        case 'send_email':
          const result = await sendEmail(params.email_id);
          return { success: result.ok, id: result.id };
        case 'open_tab':
          window.open(params.url, '_blank', 'noopener');
          return { opened: true };
        default:
          throw new Error(`Unhandled client tool: ${name}`);
      }
    },
  } satisfies ChatKitOptions);

  return <ChatKit control={control} className="h-[600px] w-[320px]" />;
}

</TabItem> <TabItem label="Vanilla JS">

const chatkit = document.getElementById('chatkit');

chatkit.setOptions({
  api: { clientToken },
  async onClientTool({ name, params }) {
    if (name === 'get_geolocation') {
      const position = await new Promise<GeolocationPosition>((resolve, reject) => {
        navigator.geolocation.getCurrentPosition(resolve, reject);
      });
      return {
        latitude: position.coords.latitude,
        longitude: position.coords.longitude,
      };
    }

    throw new Error(`Unknown client tool: ${name}`);
  },
});
</TabItem> </Tabs>

Returning values
* 
Return only JSON-serializable objects. They are sent straight back to your backend.

Async work is supported—onClientTool can return a promise.

Throwing an error surfaces the message to the agent and halts the tool call.

* If a tool does not need to return data, return {} to mark the invocation as success.
...
FILE: .docs/chatkit_js/docs/guides/custom-backends.mdx
title: Custom backends description: Build a bespoke backend for ChatKit using your own stack.
import { TabItem, Tabs } from '@astrojs/starlight/components';
Use a custom backend when you need full control over routing, tools, memory, or security. Provide a custom fetch function to use for API requests and orchestrate model calls yourself.
Approaches
* 
Use the ChatKit Python SDK for fast integration

* Or integrate directly with your model provider and implement compatible events
Configure ChatKit
<Tabs syncKey="language"> <TabItem label="React"> ```jsx const auth = getUserAuth(); // your custom auth info

const { control } = useChatKit({
  api: {
    url: 'https://your-domain.com/your/chatkit/api',

    // Any info you inject in the custom fetch callback is invisible to ChatKit.
    fetch(url: string, options: RequestInit) {
      return fetch(url, {
        ...options,

        // Inject your auth header here.
        headers: {
          ...options.headers,
          "Authorization": `Bearer ${auth}`,
        },

        // You can override any options here
      });
    },

    // Required when attachments are enabled.
    uploadStrategy: {
      type: "direct",
      uploadUrl: "https://your-domain.com/your/chatkit/api/upload",
    }

    // Register your domain in the dashboard at
    // https://platform.openai.com/settings/organization/security/domain-allowlist
    domainKey: "your-domain-key",
  },
});

</TabItem>

<TabItem label="Vanilla JS">

```js
  const chatkit = document.getElementById('my-chat');

  chatkit.setOptions({
    api: {
      url: 'https://your-domain.com/your/chatkit/api',
      fetch(url: string, options: RequestInit) {
        return fetch(url, {
          ...options,
          headers: {
            ...options.headers,
            // Inject your auth header here.
            // Anything you do in this callback is invisible to ChatKit.
            "Authorization": `Bearer ${auth}`,
          },
          // You can override any options here
        });
      },
      // Register your domain in the dashboard at
      // https://platform.openai.com/settings/organization/security/domain-allowlist
      domainKey: "your-domain-key",
      // Required when attachments are enabled.
      uploadStrategy: {
        type: "direct",
        uploadUrl: "https://your-domain.com/your/chatkit/api/upload",
      }
    },
  });
</TabItem> </Tabs>





FILE: .docs/chatkit_js/docs/guides/localization.mdx
---
title: Localization
description: Control ChatKit locales and align UI strings with your backend.
---

import { TabItem, Tabs } from '@astrojs/starlight/components';

## Automatic locale detection

ChatKit translates its built-in UI (system messages, default header labels, generic errors) using the browser's preferred locale. If the requested locale is not available, ChatKit falls back to English.

## Overriding the locale option

Set the `locale` option whenever you need to lock ChatKit to a specific translation, regardless of browser preferences.

<Tabs syncKey="localization-locale">
<TabItem label="React">

```tsx
import { ChatKit, useChatKit } from '@openai/chatkit-react';

export function SupportChat({ clientToken }: { clientToken: string }) {
  const { control } = useChatKit({
    // ... other options
    locale: 'fr',
  });

  return <ChatKit control={control} className="h-[520px]" />;
}

</TabItem> <TabItem label="Vanilla JS">
el.setOptions({
  theme: {
    colorScheme: "dark",
    color: { accent: { primary: "#D7263D", level: 2 } },
    radius: "round",
    density: "normal",
    typography: { fontFamily: "Open Sans, sans-serif" },
  },
  header: {
    customButtonLeft: {
      icon: "settings-cog",
      onClick: () => alert("Profile settings"),
    },
  },
  composer: {
    placeholder: "Type your product feedback…",
    tools: [{ id: "rate", label: "Rate", icon: "star", pinned: true }],
  },
  startScreen: {
    greeting: "Welcome to FeedbackBot!",
    prompts: [{ name: "Bug", prompt: "Report a bug", icon: "bolt" }],
  },
  entities: {
    onTagSearch: async (query) => [
      { id: "user_123", title: "Jane Doe" },
    ],
    onRequestPreview: async (entity) => ({
      preview: {
        type: "Card",
        children: [
          { type: "Text", value: `Profile: ${entity.title}` },
          { type: "Text", value: "Role: Developer" },
        ],
      },
    }),
  },
});

</TabItem> </Tabs>
ChatKit is customized by passing in options object.
* 
In React options are passed to useChatKit({...})

* In a direct integration options are set with chatkit.setOptions({...})
In both cases the shape of the options object is the same. Below are some examples of how to customize ChatKit.

Change the theme
Match your app’s aesthetic by switching between light and dark themes, setting an accent color, controlling the density, rounding of corners, etc.
For all theming options, see the API reference.
const options: Partial<ChatKitOptions> = {
  theme: {
    colorScheme: "dark",
    color: { 
      accent: { 
        primary: "#2D8CFF", 
        level: 2 
      }
    },
    radius: "round", 
    density: "compact",
    typography: { fontFamily: "'Inter', sans-serif" },
  },
};

Override text in the composer and start screen
Let users know what to ask or guide their first input by changing the composer’s placeholder text.
const options: Partial<ChatKitOptions> = {
  composer: {
    placeholder: "Ask anything about your data…",
  },
  startScreen: {
    greeting: "Welcome to FeedbackBot!",
  },
};

Show starter prompts for new threads
Guide users on what to ask or do by suggesting prompt ideas when starting a conversation.
const options: Partial<ChatKitOptions> = {
  startScreen: {
    greeting: "What can I help you build today?",
    prompts: [
      { 
        name: "Check on the status of a ticket", 
        prompt: "Can you help me check on the status of a ticket?", 
        icon: "search"
      },
      { 
        name: "Create Ticket", 
        prompt: "Can you help me create a new support ticket?", 
        icon: "write"
      },
    ],
  },
};

Add custom buttons to the header
Custom header buttons help you add navigation, context, or actions relevant to your integration.
const options: Partial<ChatKitOptions> = {
  header: {
    customButtonLeft: {
      icon: "settings-cog",
      onClick: () => openProfileSettings(),
    },
    customButtonRight: {
      icon: "home",
      onClick: () => openHomePage(),
    },
  },
};

Enable file attachments
Attachments are disabled by default. To enable them, add attachments configuration. Unless you are doing a custom backend, you must use the hosted upload strategy. See the Python SDK docs for more information on other upload strategies work with a custom backend.
You can also control the number, size, and types of files that users can attach to messages.
const options: Partial<ChatKitOptions> = {
  composer: {
    attachments: {
      uploadStrategy: { type: 'hosted' },
      maxSize: 20 * 1024 * 1024, // 20MB per file
      maxCount: 3,
      accept: { "application/pdf": [".pdf"], "image/*": [".png", ".jpg"] },
    },
  },
}


### Enable @mentions in the composer with entity tags

Let users tag custom “entities” with @-mentions. This enables richer conversation context and interactivity.

- Use `onTagSearch` to return a list of entities based on the input query.
- Use `onClick` to handle the click event of an entity.

```jsx
const options: Partial<ChatKitOptions> = {
  entities: {
    async onTagSearch(query) {
      return [
        { 
          id: "user_123", 
          title: "Jane Doe", 
          group: "People", 
          interactive: true, 
        },
        { 
          id: "document_123", 
          title: "Quarterly Plan", 
          group: "Documents", 
          interactive: true, 
        },
      ]
    },
    onClick: (entity) => {
      navigateToEntity(entity.id);
    },
  },
};

Customize how entity tags appear
You can customize the appearance of entity tags on mouseover using widgets. Show rich previews such as a business card, document summary, or image when the user hovers over an entity tag.
Something about using widget studio should go here.
const options: Partial<ChatKitOptions> = {
  entities: {
    async onTagSearch() { /* ... */ },
    onRequestPreview: async (entity) => ({
      preview: {
        type: "Card",
        children: [
          { type: "Text", value: `Profile: ${entity.title}` },
          { type: "Text", value: "Role: Developer" },
        ],
      },
    }),
  },
};

Add custom tools to the composer
Enhance productivity by letting users trigger app-specific actions from the composer bar. The selected tool will be sent to the model as a tool preference.
Link to docs on tools.
const options: Partial<ChatKitOptions> = {
  composer: {
    tools: [
      {
        id: 'add-note',
        label: 'Add Note',
        icon: 'write',
        pinned: true,
      },
    ],
  },
};

Toggle UI regions/features
Disable major UI regions / features.
* 
Disabling the header can be useful when you need more customization over the options that are available in the header and want to implement your own.

* Disabling history can be useful when the concept of threads/history doesn't make sense for your use case, e.g. a support chatbot.
const options: Partial<ChatKitOptions> = {
  history: { enabled: false },
  header: { enabled: false },
};

Override the locale
Override the default locale e.g. if you have an app-wide language setting. By default the locale is set to the browser's locale.
const options: Partial<ChatKitOptions> = {
  locale: 'de-DE',
};



FILE: .docs/chatkit_js/docs/guides/widget-actions.mdx
---
title: Widget actions
description: Handle custom widget interactions and previews in ChatKit.
---

Widgets let you display context, shortcuts, and interactive cards right inside the conversation.

When a user interacts with a widget that has a client-side action handler, ChatKit calls the handler you provided through widgets.onAction.

## Handle actions on the client

Use the `onAction` callback from `WidgetsOption` (or the equivalent React hook) to capture widget events. Forward the action payload to your backend so it can take the appropriate side effect.

```ts
chatkit.setOptions({
  widgets: {
    async onAction(action, item) {
      if (action.type === 'refresh-dashboard') {
        // handle client-side state
        store.setState({ refreshing: true });

        // and/or send the action to your server
        await fetch('your/api/refresh-dashboard', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ page: action.payload.page, itemId: item.id }),
        });
      }
       
      // ..handle other actions
    },
  },
});

Looking for a full server example? The ChatKit Python SDK docs include an end-to-end walkthrough that mirrors the JS API.
Design widgets quickly
Use the Widget Studio to experiment with card layouts, list rows, and preview components. When you are satisfied, copy the generated JSON into your integration and serve it from your backend.
...
FILE: .docs/chatkit_js/docs/index.mdx
title: OpenAI Agent Embeds description: Embed ChatKit in your app using React or the Web Component. tableOfContents: false
import { Code, TabItem, Tabs } from '@astrojs/starlight/components';
<div class="openai-hero"> <div class="openai-hero-container flex gap-4"> <div class="openai-quickstart flex-1 flex items-center"> <div> <h2 class="title">Quickstart</h2> <p>Build your first chat application in minutes.</p> <a href={`https://platform.openai.com/docs/guides/chatkit`} class="openai-hero-cta">Let's build</a> </div> </div> <div class="openai-hero-code flex-1 overflow-x-scroll"> <Tabs> <TabItem label="React">
    ```tsx
    function MyChat({ clientToken }) {
      const { control } = useChatKit({ 
        api: { clientToken } 
      });

      return (
        <ChatKit 
          control={control}
          className="h-[600px] w-[320px]"
        />
      );
    }
    ```

    </TabItem>
    <TabItem label="Vanilla JS">

    ```js
    function InitChatkit({ clientToken }) {
      const chatkit = document.createElement('openai-chatkit');
      chatkit.setOptions({ api: { clientToken } });
      chatkit.classList.add('h-[600px]', 'w-[320px]');
      document.body.appendChild(chatkit);
    }     
    ```

    </TabItem>
    </Tabs>
    </div>

</div> </div>
...
Overview
ChatKit is a framework for building high-quality, AI-powered chat experiences. It’s designed for developers who want to add advanced conversational intelligence to their apps fast—with minimal setup and no reinventing the wheel. ChatKit delivers a complete, production-ready chat interface out of the box.
Key features
* 
Deep UI customization so that ChatKit feels like a first-class part of your app

Built-in response streaming for interactive, natural conversations

Tool and workflow integration for visualizing agentic actions and chain-of-thought reasoning

Rich interactive widgets rendered directly inside the chat

Attachment handling with support for file and image uploads

Thread and message management for organizing complex conversations

* Source annotations and entity tagging for transparency and references
Simply drop the ChatKit component into your app, configure a few options, and you're good to go.



## What makes ChatKit different?

ChatKit is a framework-agnostic, drop-in chat solution. 
You don’t need to build custom UIs, manage low-level chat state, or patch together various features yourself.
Just add the ChatKit component, give it a client token, and customize the chat experience as needed, no extra work needed.

...

## Server Integration (Python SDK Example)

ChatKit's server integration offers a flexible and framework-agnostic approach for building realtime chat experiences. By implementing the `ChatKitServer` base class and its `respond` method, you can configure how your workflow responds to user inputs, from using tools to returning rich display widgets. The ChatKit server integration exposes a single endpoint and supports JSON and server‑sent events (SSE) to stream real-time updates.

### Installation

Install the `openai-chatkit` package with the following command:

```bash
pip install openai-chatkit

Defining a server class
The ChatKitServer base class is the main building block of the ChatKit server implementation.
The respond method is executed each time a user sends a message. It is responsible for providing an answer by streaming a set of events. The respond method can return assistant messages, tool status messages, workflows, tasks, and widgets.
ChatKit also provides helpers to implement respond using Agents SDK. The main one is stream_agent_response, which converts a streamed Agents SDK run into ChatKit events.
If you've enabled model or tool options in the composer, they'll appear in respond under input_user_message.inference_options. Your integration is responsible for handling these values when performing inference.
Example server implementation that calls the Agent SDK runner and streams the result to the ChatKit UI:
class MyChatKitServer(ChatKitServer):
    def __init__(
        self, data_store: Store, attachment_store: AttachmentStore | None = None
    ):
        super().__init__(data_store, attachment_store)

    assistant_agent = Agent[AgentContext](
        model="gpt-4.1",
        name="Assistant",
        instructions="You are a helpful assistant"
    )

    async def respond(
        self,
        thread: ThreadMetadata,
        input: UserMessageItem | None,
        context: Any,
    ) -> AsyncIterator[ThreadStreamEvent]:
        context = AgentContext(
            thread=thread,
            store=self.store,
            request_context=context,
        )
        result = Runner.run_streamed(
            self.assistant_agent,
            await simple_to_agent_input(input) if input else [],
            context=context,
        )
        async for event in stream_agent_response(
            context,
            result,
        ):
            yield event
## Setting up the endpoint

ChatKit is server-agnostic. All communication happens through a single POST endpoint that returns either JSON directly or streams SSE JSON events.

You are responsible for defining the endpoint using the web server framework of your choice.

Example using ChatKit with FastAPI:

```python
app = FastAPI()
data_store = PostgresStore()
attachment_store = BlobStorageStore(data_store)
server = MyChatKitServer(data_store, attachment_store)

@app.post("/chatkit")
async def chatkit_endpoint(request: Request):
    result = await server.process(await request.body(), {})
    if isinstance(result, StreamingResult):
        return StreamingResponse(result, media_type="text/event-stream")
    else:
        return Response(content=result.json, media_type="application/json")

Data store
ChatKit needs to store information about threads, messages, and attachments. The examples above use a provided development-only data store implementation using SQLite (SQLiteStore).
You are responsible for implementing the chatkit.store.Store class using the data store of your choice. When implementing the store, you must allow for the Thread/Attachment/ThreadItem type shapes changing between library versions. The recommended approach for relational databases is to serialize models into JSON-typed columns instead of separating model fields across multiple columns.
class Store(ABC, Generic[TContext]):
    def generate_thread_id(self, context: TContext) -> str: ...

    def generate_item_id(
        self,
        item_type: Literal["message", "tool_call", "task", "workflow", "attachment"],
        thread: ThreadMetadata,
        context: TContext,
    ) -> str: ...

    async def load_thread(self, thread_id: str, context: TContext) -> ThreadMetadata: ...

    async def save_thread(self, thread: ThreadMetadata, context: TContext) -> None: ...

    async def load_thread_items(
        self,
        thread_id: str,
        after: str | None,
        limit: int,
        order: str,
        context: TContext,
    ) -> Page[ThreadItem]: ...

    async def save_attachment(self, attachment: Attachment, context: TContext) -> None: ...

    async def load_attachment(self, attachment_id: str, context: TContext) -> Attachment: ...

    async def delete_attachment(self, attachment_id: str, context: TContext) -> None: ...

    async def load_threads(
        self,
        limit: int,
        after: str | None,
        order: str,
        context: TContext,
    ) -> Page[ThreadMetadata]: ...

    async def add_thread_item(
        self, thread_id: str, item: ThreadItem, context: TContext
    ) -> None: ...

    async def save_item(self, thread_id: str, item: ThreadItem, context: TContext) -> None: ...

    async def load_item(self, thread_id: str, item_id: str, context: TContext) -> ThreadItem: ...

    async def delete_thread(self, thread_id: str, context: TContext) -> None: ...

The default implementation prefixes identifiers (for example msg_4f62d6a7f2c34bd084f57cfb3df9f6bd) using UUID4 strings. Override generate_thread_id and/or generate_item_id if your integration needs deterministic or pre-allocated identifiers; they will be used whenever ChatKit needs to create a new thread id or a new thread item id.
Attachment store
Users can upload attachments (files and images) to include with chat messages. You are responsible for providing a storage implementation and handling uploads. The attachment_store argument to ChatKitServer should implement the AttachmentStore interface. If not provided, operations on attachments will raise an error.
ChatKit supports both direct uploads and two‑phase upload, configurable client-side via ChatKitOptions.composer.attachments.uploadStrategy.
Access control
Attachment metadata and file bytes are not protected by ChatKit. Each AttachmentStore method receives your request context so you can enforce thread- and user-level authorization before handing out attachment IDs, bytes, or signed URLs. Deny access when the caller does not own the attachment, and generate download URLs that expire quickly. Skipping these checks can leak customer data.


### Direct upload

The direct upload URL is provided client-side as a create option.

The client will POST `multipart/form-data` with a `file` field to that URL. The server should:

1. persist the attachment metadata (`FileAttachment | ImageAttachment`) to the data store and the file bytes to your storage.
2. respond with JSON representation of `FileAttachment | ImageAttachment`.

### Two‑phase upload

- **Phase 1 (registration and upload URL provisioning)**: The client calls `attachments.create`. ChatKit persists a `FileAttachment | ImageAttachment` sets the `upload_url` and returns it. It's recommended to include the `id` of the `Attachment` in the `upload_url` so that you can associate the file bytes with the `Attachment`.
- **Phase 2 (upload)**: The client POSTs the bytes to the returned `upload_url` with `multipart/form-data` field `file`.

### Previews

To render thumbnails of an image attached to a user message, set `ImageAttachment.preview_url` to a renderable URL. If you need expiring URLs, do not persist the URL; generate it on demand when returning the attachment to the client.

### AttachmentStore interface

You implement the storage specifics by providing the `AttachmentStore` methods:

```python
class AttachmentStore(ABC, Generic[TContext]):
    async def delete_attachment(self, attachment_id: str, context: TContext) -> None: ...
    async def create_attachment(self, input: AttachmentCreateParams, context: TContext) -> Attachment: ...
    def generate_attachment_id(self, mime_type: str, context: TContext) -> str: ...

Note: The store does not have to persist bytes itself. It can act as a proxy that issues signed URLs for upload and preview (e.g., S3/GCS/Azure), while your separate upload endpoint writes to object storage.
Attaching files to Agent SDK inputs
You are also responsible for deciding how to attach attachments to Agent SDK inputs. You can store files in your own storage and attach them as base64-encoded payloads or upload them to the OpenAI Files API and provide the file ID to the Agent SDK.
The example below shows how to create base64-encoded payloads for attachments by customizing a ThreadItemConverter. The helper read_attachment_bytes stands in for whatever storage accessor you provide (for example, fetching from S3 or a database) because AttachmentStore only handles ChatKit protocol calls.
async def read_attachment_bytes(attachment_id: str) -> bytes:
    """Replace with your blob-store fetch (S3, local disk, etc.)."""
    ...


class MyConverter(ThreadItemConverter):
    async def attachment_to_message_content(
        self, input: Attachment
    ) -> ResponseInputContentParam:
        content = await read_attachment_bytes(input.id)
        data = (
            "data:"
            + str(input.mime_type)
            + ";base64,"
            + base64.b64encode(content).decode("utf-8")
        )
        if isinstance(input, ImageAttachment):
            return ResponseInputImageParam(
                type="input_image",
                detail="auto",
                image_url=data,
            )
        # Note: Agents SDK currently only supports pdf files as ResponseInputFileParam.
        # To send other text file types, either convert them to pdf on the fly or
        # add them as input text.
        return ResponseInputFileParam(
            type="input_file",
            file_data=data,
            filename=input.name or "unknown",
        )

# In respond(...):
result = Runner.run_streamed(
    assistant_agent,
    await MyConverter().to_agent_input(input),
    context=context,
)


## Client tools usage

The ChatKit server implementation can trigger client-side tools.

The tool must be registered both when initializing ChatKit on the client and when setting up Agents SDK on the server.

To trigger a client-side tool from Agents SDK, set `ctx.context.client_tool_call` in the tool implementation with the client-side tool name and arguments. The result of the client tool execution will be provided back to the model.

**Note:** The agent behavior must be set to `tool_use_behavior=StopAtTools` with all client-side tools included in `stop_at_tool_names`. This causes the agent to stop generating new messages until the client tool call is acknowledged by the ChatKit UI.

**Note:** Only one client tool call can be triggered per turn.

**Note:** Client tools are client-side callbacks invoked by the agent during server-side inference. If you're interested in client-side callbacks triggered by a user interacting with a widget, refer to [client actions](actions.md/#client).

```python
@function_tool(description_override="Add an item to the user's todo list.")
async def add_to_todo_list(ctx: RunContextWrapper[AgentContext], item: str) -> None:
    ctx.context.client_tool_call = ClientToolCall(
        name="add_to_todo_list",
        arguments={"item": item},
    )

assistant_agent = Agent[AgentContext](
    model="gpt-4.1",
    name="Assistant",
    instructions="You are a helpful assistant",
    tools=[add_to_todo_list],
    tool_use_behavior=StopAtTools(stop_at_tool_names=[add_to_todo_list.name]),
)

Agents SDK integration
The ChatKit server is independent of Agents SDK. As long as correct events are returned from the respond method, the ChatKit UI will display the conversation as expected.
The ChatKit library provides helpers to integrate with Agents SDK:
* 
AgentContext - The context type that should be used when calling Agents SDK. It provides helpers to stream events from tool calls, render widgets, and initiate client tool calls.

stream_agent_response - A helper to convert a streamed Agents SDK run into ChatKit events.

ThreadItemConverter - A helper class that you'll probably extend to convert ChatKit thread items to Agents SDK input items.

* simple_to_agent_input - A helper function that uses the default thread item conversions. The default conversion is limited, but useful for getting started quickly.
async def respond([]
    self,
    thread: ThreadMetadata,
    input: UserMessageItem | None,
    context: Any,
) -> AsyncIterator[ThreadStreamEvent]:
    context = AgentContext(
        thread=thread,
        store=self.store,
        request_context=context,
    )

    result = Runner.run_streamed(
        agent,
        input=...,
        previous_response_id=previous_response_id,
    )

    async for event in stream_agent_response(context, result):
        yield event

ThreadItemConverter
Extend ThreadItemConverter when your integration supports:
* 
Attachments

@-mentions (entity tagging)

HiddenContextItem

* Custom thread item formats
from agents import Message, Runner, ResponseInputTextParam
from chatkit.agents import AgentContext, ThreadItemConverter, stream_agent_response
from chatkit.types import Attachment, HiddenContextItem, ThreadMetadata, UserMessageItem

class MyThreadConverter(ThreadItemConverter):
    async def attachment_to_message_content(
        self, attachment: Attachment
    ) -> ResponseInputTextParam:
        content = await attachment_store.get_attachment_contents(attachment.id)
        data_url = "data:%s;base64,%s" % (mime, base64.b64encode(raw).decode("utf-8"))
        if isinstance(attachment, ImageAttachment):
            return ResponseInputImageParam(
                type="input_image",
                detail="auto",
                image_url=data_url,
            )

        # ..handle other attachment types

    def hidden_context_to_input(self, item: HiddenContextItem) -> Message:
        return Message(
            type="message",
            role="system",
            content=[
                ResponseInputTextParam(
                    type="input_text",
                    text=f"<HIDDEN_CONTEXT>{item.content}</HIDDEN_CONTEXT>",
                )
            ],
        )

    def tag_to_message_content(self, tag: UserMessageTagContent):
        tag_context = await retrieve_context_for_tag(tag.id)
        return ResponseInputTextParam(
            type="input_text",
            text=f"<TAG>Name:{tag.data.name}\nType:{tag.data.type}\nDetails:{tag_context}</TAG>"
        )


## Widgets

Widgets are rich UI components that can be displayed in chat. You can return a widget either directly from the `respond` method (if you want to do so unconditionally) or from a tool call triggered by the model.

Example of a widget returned directly from the `respond` method:

```python
async def respond(
        self,
        thread: ThreadMetadata,
        input: UserMessageItem | None,
        context: Any,
    ) -> AsyncIterator[ThreadStreamEvent]:
    widget = Text(
        id="description",
        value="Text widget",
    )

    async for event in stream_widget(
        thread,
        widget,
        generate_id=lambda item_type: self.store.generate_item_id(
            item_type, thread, context
        ),
    ):
        yield event

Example of a widget returned from a tool call:
@function_tool(description_override="Display a sample widget to the user.")
async def sample_widget(ctx: RunContextWrapper[AgentContext]) -> None:
    widget = Text(
        id="description",
        value="Text widget",
    )

    await ctx.context.stream_widget(widget)

The examples above return a fully completed static widget. You can also stream an updating widget by yielding new versions of the widget from a generator function. The ChatKit framework will send updates for the parts of the widget that have changed.
Note: Currently, only <Text> and <Markdown> components marked with an id have their text updates streamed.
async def sample_widget(ctx: RunContextWrapper[AgentContext]) -> None:
     description_text = Runner.run_streamed(
        email_generator, "ChatKit is the best thing ever"
    )

    async def widget_generator() -> AsyncGenerator[Widget, None]:
        text_widget_updates = accumulate_text(
            description_text.stream_events(),
            Text(
                id="description",
                value="",
                streaming=True
            ),
        )

        async for text_widget in text_widget_updates:
            yield Card(
                children=[text_widget]
            )

    await ctx.context.stream_widget(widget_generator())

In the example above, the accumulate_text function is used to stream the results of an Agents SDK run into a Text widget.
Defining a widget
You may find it easier to write widgets in JSON. To you can parse JSON widgets to WidgetRoot instances for your server to stream:
try:
    WidgetRoot.model_validate_json(WIDGET_JSON_STRING)
except ValidationError:
    # handle invalid json

Widget reference and examples
See full reference of components, props, and examples in widgets.md ➡️.


## Thread metadata

ChatKit provides a way to store arbitrary information associated with a thread. This information is not sent to the UI.

One use case for the metadata is to preserve the [`previous_response_id`](https://platform.openai.com/docs/api-reference/responses/create#responses-create-previous_response_id) and avoid having to re-send all items for an Agent SDK run.

```python
previous_response_id = thread.metadata.get("previous_response_id")

# Run the Agent SDK run with the previous response id
result = Runner.run_streamed(
    agent,
    input=...,
    previous_response_id=previous_response_id,
)

# Save the previous response id for the next run
thread.metadata["previous_response_id"] = result.response_id

Automatic thread titles
ChatKit does not automatically title threads, but you can easily implement your own logic to do so.
First, decide when to trigger the thread title update. A simple approach might be to set the thread title the first time a user sends a message.
from chatkit.agents import simple_to_agent_input

async def maybe_update_thread_title(
    self,
    thread: ThreadMetadata,
    input_item: UserMessageItem,
) -> None:
    if thread.title is not None:
        return
    agent_input = await simple_to_agent_input(input_item)
    run = await Runner.run(title_agent, input=agent_input)
    thread.title = run.final_output

async def respond(
    self,
    thread: ThreadMetadata,
    input: UserMessageItem | None,
    context: Any,
) -> AsyncIterator[ThreadStreamEvent]:
    if input is not None:
        asyncio.create_task(self.maybe_update_thread_title(thread, input))

    # Generate the model response
    ...

Progress updates
If your server-side tool takes a while to run, you can use the progress update event to display the progress to the user.
@function_tool()
async def long_running_tool(ctx: RunContextWrapper[AgentContext]) -> str:
    await ctx.context.stream(
        ProgressUpdateEvent(text="Loading a user profile...")
    )

    await asyncio.sleep(1)

The progress update will be automatically replaced by the next assistant message, widget, or another progress update.
Server context
Sometimes it's useful to pass additional information (like userId) to the ChatKit server implementation. The ChatKitServer.process method accepts a context parameter that it passes to the respond method and all data store and file store methods.
class MyChatKitServer(ChatKitServer):
    async def respond(..., context) -> AsyncIterator[ThreadStreamEvent]:
        # consume context["userId"]

server.process(..., context={"userId": "user_123"})

Server context may be used to implement permission checks in AttachmentStore and Store.
class MyChatKitServer(ChatKitServer):
    async def load_attachment(..., context) -> Attachment:
        # check context["userId"] has access to the file


# ChatKit actions

Actions are a way for the ChatKit SDK frontend to trigger a streaming response without the user submitting a message. They can also be used to trigger side-effects outside ChatKit SDK.

## Triggering actions

### In response to user interaction with widgets

Actions can be triggered by attaching an `ActionConfig` to any widget node that supports it. For example, you can respond to click events on Buttons. When a user clicks on this button, the action will be sent to your server where you can update the widget, run inference, stream new thread items, etc.

```python
Button(
    label="Example",
    onClickAction=ActionConfig(
      type="example",
      payload={"id": 123},
    )
)

Actions can also be sent imperatively by your frontend with sendAction(). This is probably most useful when you need ChatKit to respond to interaction happening outside ChatKit, but it can also be used to chain actions when you need to respond on both the client and the server (more on that below).
await chatKit.sendAction({
  type: "example",
  payload: { id: 123 },
});

Handling actions
On the server
By default, actions are sent to your server. You can handle actions on your server by implementing the action method on ChatKitServer.
from collections.abc import AsyncIterator
from datetime import datetime
from typing import Any

from chatkit.actions import Action
from chatkit.server import ChatKitServer
from chatkit.types import (
    HiddenContextItem,
    ThreadItemDoneEvent,
    ThreadMetadata,
    ThreadStreamEvent,
    WidgetItem,
)

RequestContext = dict[str, Any]


class MyChatKitServer(ChatKitServer[RequestContext]):
    async def action(
        self,
        thread: ThreadMetadata,
        action: Action[str, Any],
        sender: WidgetItem | None,
        context: RequestContext,
    ) -> AsyncIterator[ThreadStreamEvent]:
        if action.type == "example":
            await do_thing(action.payload['id'])

            # often you'll want to add a HiddenContextItem so the model
            # can see that the user did something
            hidden = HiddenContextItem(
                id=self.store.generate_item_id("message", thread, context),
                thread_id=thread.id,
                created_at=datetime.now(),
                content=["<USER_ACTION>The user did a thing</USER_ACTION>"],
            )
            await self.store.add_thread_item(thread.id, hidden, context)

            # then you might want to run inference to stream a response
            # back to the user.
            async for e in self.generate(context, thread):
                yield e

        if action.type == "another.example"
          # ...

NOTE: As with any client/server interaction, actions and their payloads are sent by the client and should be treated as untrusted data.


### Client

Sometimes you’ll want to handle actions in your client integration. To do that you need to specify that the action should be sent to your client-side action handler by adding `handler="client"` to the `ActionConfig`.

```python
Button(
    label="Example",
    onClickAction=ActionConfig(
      type="example",
      payload={"id": 123},
      handler="client"
    )
)

Then, when the action is triggered, it will then be passed to a callback that you provide when instantiating ChatKit.
async function handleWidgetAction(action: {type: string, Record<string, unknown>}) {
  if (action.type === "example") {
    const res = await doSomething(action)

    // You can fire off actions to your server from here as well.
    // e.g. if you want to stream new thread items or update a widget.
    await chatKit.sendAction({
      type: "example_complete",
      payload: res
    })
  }
}

chatKit.setOptions({
  // other options...
  widgets: { onAction: handleWidgetAction }
})

Strongly typed actions
By default Action and ActionConfig are not strongly typed. However, we do expose a create helper on Action making it easy to generate ActionConfigs from a set of strongly-typed actions.
class ExamplePayload(BaseModel)
    id: int

ExampleAction = Action[Literal["example"], ExamplePayload]
OtherAction = Action[Literal["other"], None]

AppAction = Annotated[
  ExampleAction
  | OtherAction,
  Field(discriminator="type"),
]

ActionAdapter: TypeAdapter[AppAction] = TypeAdapter(AppAction)

def parse_app_action(action: Action[str, Any]): AppAction
  return ActionAdapter.validate_python(action)

# Usage in a widget
# Action provides a create helper which makes it easy to generate
# ActionConfigs from strongly typed actions.
Button(
    label="Example",
    onClickAction=ExampleAction.create(ExamplePayload(id=123))
)

# usage in action handler
class MyChatKitServer(ChatKitServer[RequestContext])
    async def action(
        self,
        thread: ThreadMetadata,
        action: Action[str, Any],
        sender: WidgetItem | None,
        context: RequestContext,
    ) -> AsyncIterator[Event]:
        # add custom error handling if needed
        app_action = parse_app_action(action)
        if (app_action.type == "example"):
            await do_thing(app_action.payload.id)

Use widgets and actions to create custom forms
When widget nodes that take user input are mounted inside a Form, the values from those fields will be included in the payload of all actions that originate from within the Form.
Form values are keyed in the payload by their name e.g.
* 
Select(name="title") → action.payload.title

* Select(name="todo.title") → action.payload.todo.title
Form(
  direction="col",
  onSubmitAction=ActionConfig(
	  type="update_todo",
	  payload={"id": todo.id}
  ),
  children=[
    Title(value="Edit Todo"),

    Text(value="Title", color="secondary", size="sm"),
    Text(
      value=todo.title,
      editable=EditableProps(name="title", required=True),
    )

    Text(value="Description", color="secondary", size="sm"),
    Text(
      value=todo.description,
      editable=EditableProps(name="description"),
    ),

    Button(label="Save", submit=true)
  ]
)

class MyChatKitServer(ChatKitServer[RequestContext])
    async def action(
        self,
        thread: ThreadMetadata,
        action: Action[str, Any],
        sender: WidgetItem | None,
        context: RequestContext,
    ) -> AsyncIterator[Event]:
        if (action.type == "update_todo"):
          id = action.payload['id']
          # Any action that originates from within the Form will
          # include title and description
          title = action.payload['title']
          description = action.payload['description']

	        # ...

Validation
Form uses basic native form validation; enforcing required and pattern on fields where they are configured and blocking submission when the form has any invalid field.
We may add new validation modes with better UX, more expressive validation, custom error display, etc in the future. Until then, widgets are not a great medium for complex forms with tricky validation. If you have this need, a better pattern would be to use client side action handling to trigger a modal, show a custom form there, then pass the result back into ChatKit with sendAction.
Treating Card as a Form
You can pass asForm=True to Card and it will behave as a Form, running validation and passing collected fields to the Card’s confirm action.
Payload key collisions
If there is a naming collision with some other existing pre-defined key on your payload, the form value will be ignored. This is probably a bug, so we’ll emit an error event when we see this.
Customize how actions interact with loading states in widgets
Use ActionConfig.loadingBehavior to control how actions trigger different loading states in a widget.
Button(
    label="This make take a while...",
    onClickAction=ActionConfig(
      type="long_running_action_that_should_block_other_ui_interactions",
      loadingBehavior="container"
    )
)

Value	Behavior
auto	The action will adapt to how it’s being used. (default)
self	The action triggers loading state on the widget node that the action was bound to.
container	The action triggers loading state on the entire widget container. This causes the widget to fade out slightly and become inert.
none	No loading state
Using auto behavior
Generally, we recommend using auto, which is the default. auto triggers loading states based on where the action is bound, for example:
* 
Button.onClickAction → self

Select.onChangeAction → none

Card.confirm.action → container


# Orchestrating multiple agents

Orchestration refers to the flow of agents in your app. Which agents run, in what order, and how do they decide what happens next? There are two main ways to orchestrate agents:

1. Allowing the LLM to make decisions: this uses the intelligence of an LLM to plan, reason, and decide on what steps to take based on that.
2. Orchestrating via code: determining the flow of agents via your code.

You can mix and match these patterns. Each has their own tradeoffs, described below.

## Orchestrating via LLM

An agent is an LLM equipped with instructions, tools and handoffs. This means that given an open-ended task, the LLM can autonomously plan how it will tackle the task, using tools to take actions and acquire data, and using handoffs to delegate tasks to sub-agents. For example, a research agent could be equipped with tools like:

- Web search to find information online
- File search and retrieval to search through proprietary data and connections
- Computer use to take actions on a computer
- Code execution to do data analysis
- Handoffs to specialized agents that are great at planning, report writing and more.

This pattern is great when the task is open-ended and you want to rely on the intelligence of an LLM. The most important tactics here are:

1. Invest in good prompts. Make it clear what tools are available, how to use them, and what parameters it must operate within.
2. Monitor your app and iterate on it. See where things go wrong, and iterate on your prompts.
3. Allow the agent to introspect and improve. For example, run it in a loop, and let it critique itself; or, provide error messages and let it improve.
4. Have specialized agents that excel in one task, rather than having a general purpose agent that is expected to be good at anything.
5. Invest in [evals](https://platform.openai.com/docs/guides/evals). This lets you train your agents to improve and get better at tasks.

## Orchestrating via code

While orchestrating via LLM is powerful, orchestrating via code makes tasks more deterministic and predictable, in terms of speed, cost and performance. Common patterns here are:

- Using [structured outputs](https://platform.openai.com/docs/guides/structured-outputs) to generate well formed data that you can inspect with your code. For example, you might ask an agent to classify the task into a few categories, and then pick the next agent based on the category.
- Chaining multiple agents by transforming the output of one into the input of the next. You can decompose a task like writing a blog post into a series of steps - do research, write an outline, write the blog post, critique it, and then improve it.
- Running the agent that performs the task in a `while` loop with an agent that evaluates and provides feedback, until the evaluator says the output passes certain criteria.
- Running multiple agents in parallel, e.g. via Python primitives like `asyncio.gather`. This is useful for speed when you have multiple tasks that don't depend on each other.

We have a number of examples in [`examples/agent_patterns`](https://github.com/openai/openai-agents-python/tree/main/examples/agent_patterns).



# Results

When you call the `Runner.run` methods, you either get a:

- [`RunResult`][agents.result.RunResult] if you call `run` or `run_sync`
- [`RunResultStreaming`][agents.result.RunResultStreaming] if you call `run_streamed`

Both of these inherit from [`RunResultBase`][agents.result.RunResultBase], which is where most useful information is present.

## Final output

The [`final_output`][agents.result.RunResultBase.final_output] property contains the final output of the last agent that ran. This is either:

- a `str`, if the last agent didn't have an `output_type` defined
- an object of type `last_agent.output_type`, if the agent had an output type defined.

!!! note

    `final_output` is of type `Any`. We can't statically type this, because of handoffs. If handoffs occur, that means any Agent might be the last agent, so we don't statically know the set of possible output types.

## Inputs for the next turn

You can use [`result.to_input_list()`][agents.result.RunResultBase.to_input_list] to turn the result into an input list that concatenates the original input you provided, to the items generated during the agent run. This makes it convenient to take the outputs of one agent run and pass them into another run, or to run it in a loop and append new user inputs each time.

## Last agent

The [`last_agent`][agents.result.RunResultBase.last_agent] property contains the last agent that ran. Depending on your application, this is often useful for the next time the user inputs something. For example, if you have a frontline triage agent that hands off to a language-specific agent, you can store the last agent, and re-use it the next time the user messages the agent.

## New items

The [`new_items`][agents.result.RunResultBase.new_items] property contains the new items generated during the run. The items are [`RunItem`][agents.items.RunItem]s. A run item wraps the raw item generated by the LLM.

- [`MessageOutputItem`][agents.items.MessageOutputItem] indicates a message from the LLM. The raw item is the message generated.
- [`HandoffCallItem`][agents.items.HandoffCallItem] indicates that the LLM called the handoff tool. The raw item is the tool call item from the LLM.
- [`HandoffOutputItem`][agents.items.HandoffOutputItem] indicates that a handoff occurred. The raw item is the tool response to the handoff tool call. You can also access the source/target agents from the item.
- [`ToolCallItem`][agents.items.ToolCallItem] indicates that the LLM invoked a tool.
- [`ToolCallOutputItem`][agents.items.ToolCallOutputItem] indicates that a tool was called. The raw item is the tool response. You can also access the tool output from the item.
- [`ReasoningItem`][agents.items.ReasoningItem] indicates a reasoning item from the LLM. The raw item is the reasoning generated.

## Other information

### Guardrail results

The [`input_guardrail_results`][agents.result.RunResultBase.input_guardrail_results] and [`output_guardrail_results`][agents.result.RunResultBase.output_guardrail_results] properties contain the results of the guardrails, if any. Guardrail results can sometimes contain useful information you want to log or store, so we make these available to you.

### Raw responses

The [`raw_responses`][agents.result.RunResultBase.raw_responses] property contains the [`ModelResponse`][agents.items.ModelResponse]s generated by the LLM.

### Original input

The [`input`][agents.result.RunResultBase.input] property contains the original input you provided to the `run` method. In most cases you won't need this, but it's available in case you do.

# Running agents

You can run agents via the [`Runner`][agents.run.Runner] class. You have 3 options:

1. [`Runner.run()`][agents.run.Runner.run], which runs async and returns a [`RunResult`][agents.result.RunResult].
2. [`Runner.run_sync()`][agents.run.Runner.run_sync], which is a sync method and just runs `.run()` under the hood.
3. [`Runner.run_streamed()`][agents.run.Runner.run_streamed], which runs async and returns a [`RunResultStreaming`][agents.result.RunResultStreaming]. It calls the LLM in streaming mode, and streams those events to you as they are received.

```python
from agents import Agent, Runner

async def main():
    agent = Agent(name="Assistant", instructions="You are a helpful assistant")

    result = await Runner.run(agent, "Write a haiku about recursion in programming.")
    print(result.final_output)
    # Code within the code,
    # Functions calling themselves,
    # Infinite loop's dance.


## The agent loop

When you use the run method in `Runner`, you pass in a starting agent and input. The input can either be a string (which is considered a user message), or a list of input items, which are the items in the OpenAI Responses API.

The runner then runs a loop:

1. We call the LLM for the current agent, with the current input.
2. The LLM produces its output.
   1. If the LLM returns a `final_output`, the loop ends and we return the result.
   2. If the LLM does a handoff, we update the current agent and input, and re-run the loop.
   3. If the LLM produces tool calls, we run those tool calls, append the results, and re-run the loop.
3. If we exceed the `max_turns` passed, we raise a [`MaxTurnsExceeded`][agents.exceptions.MaxTurnsExceeded] exception.

!!! note

    The rule for whether the LLM output is considered as a "final output" is that it produces text output with the desired type, and there are no tool calls.

## Streaming

Streaming allows you to additionally receive streaming events as the LLM runs. Once the stream is done, the [`RunResultStreaming`][agents.result.RunResultStreaming] will contain the complete information about the run, including all the new outputs produces. You can call `.stream_events()` for the streaming events. Read more in the [streaming guide](streaming.md).

## Run config

The `run_config` parameter lets you configure some global settings for the agent run:

- [`model`][agents.run.RunConfig.model]: Allows setting a global LLM model to use, irrespective of what `model` each Agent has.
- [`model_provider`][agents.run.RunConfig.model_provider]: A model provider for looking up model names, which defaults to OpenAI.
- [`model_settings`][agents.run.RunConfig.model_settings]: Overrides agent-specific settings. For example, you can set a global `temperature` or `top_p`.
- [`input_guardrails`][agents.run.RunConfig.input_guardrails], [`output_guardrails`][agents.run.RunConfig.output_guardrails]: A list of input or output guardrails to include on all runs.
- [`handoff_input_filter`][agents.run.RunConfig.handoff_input_filter]: A global input filter to apply to all handoffs, if the handoff doesn't already have one. The input filter allows you to edit the inputs that are sent to the new agent. See the documentation in [`Handoff.input_filter`][agents.handoffs.Handoff.input_filter] for more details.
- [`tracing_disabled`][agents.run.RunConfig.tracing_disabled]: Allows you to disable [tracing](tracing.md) for the entire run.
- [`trace_include_sensitive_data`][agents.run.RunConfig.trace_include_sensitive_data]: Configures whether traces will include potentially sensitive data, such as LLM and tool call inputs/outputs.
- [`workflow_name`][agents.run.RunConfig.workflow_name], [`trace_id`][agents.run.RunConfig.trace_id], [`group_id`][agents.run.RunConfig.group_id]: Sets the tracing workflow name, trace ID and trace group ID for the run. We recommend at least setting `workflow_name`. The group ID is an optional field that lets you link traces across multiple runs.
- [`trace_metadata`][agents.run.RunConfig.trace_metadata]: Metadata to include on all traces.

## Conversations/chat threads

Calling any of the run methods can result in one or more agents running (and hence one or more LLM calls), but it represents a single logical turn in a chat conversation. For example:

1. User turn: user enter text
2. Runner run: first agent calls LLM, runs tools, does a handoff to a second agent, second agent runs more tools, and then produces an output.

At the end of the agent run, you can choose what to show to the user. For example, you might show the user every new item generated by the agents, or just the final output. Either way, the user might then ask a followup question, in which case you can call the run method again.

You can use the base [`RunResultBase.to_input_list()`][agents.result.RunResultBase.to_input_list] method to get the inputs for the next turn.

```python
async def main():
    agent = Agent(name="Assistant", instructions="Reply very concisely.")

    with trace(workflow_name="Conversation", group_id=thread_id):
        # First turn
        result = await Runner.run(agent, "What city is the Golden Gate Bridge in?")
        print(result.final_output)
        # San Francisco

        # Second turn
        new_input = result.to_input_list() + [{"role": "user", "content": "What state is it in?"}]
        result = await Runner.run(agent, new_input)
        print(result.final_output)
        # California

Exceptions
The SDK raises exceptions in certain cases. The full list is in [agents.exceptions][]. As an overview:
* 
[AgentsException][agents.exceptions.AgentsException] is the base class for all exceptions raised in the SDK.

[MaxTurnsExceeded][agents.exceptions.MaxTurnsExceeded] is raised when the run exceeds the max_turns passed to the run methods.

[ModelBehaviorError][agents.exceptions.ModelBehaviorError] is raised when the model produces invalid outputs, e.g. malformed JSON or using non-existent tools.

[UserError][agents.exceptions.UserError] is raised when you (the person writing code using the SDK) make an error using the SDK.

[InputGuardrailTripwireTriggered][agents.exceptions.InputGuardrailTripwireTriggered], [OutputGuardrailTripwireTriggered][agents.exceptions.OutputGuardrailTripwireTriggered] is raised when a guardrail is tripped.


# Tracing

The Agents SDK includes built-in tracing, collecting a comprehensive record of events during an agent run: LLM generations, tool calls, handoffs, guardrails, and even custom events that occur. Using the [Traces dashboard](https://platform.openai.com/traces), you can debug, visualize, and monitor your workflows during development and in production.

!!!note

    Tracing is enabled by default. There are two ways to disable tracing:

    1. You can globally disable tracing by setting the env var `OPENAI_AGENTS_DISABLE_TRACING=1`
    2. You can disable tracing for a single run by setting [`agents.run.RunConfig.tracing_disabled`][] to `True`

**_For organizations operating under a Zero Data Retention (ZDR) policy using OpenAI's APIs, tracing is unavailable._**

## Traces and spans

- **Traces** represent a single end-to-end operation of a "workflow". They're composed of Spans. Traces have the following properties:
  - `workflow_name`: This is the logical workflow or app. For example "Code generation" or "Customer service".
  - `trace_id`: A unique ID for the trace. Automatically generated if you don't pass one. Must have the format `trace_<32_alphanumeric>`.
  - `group_id`: Optional group ID, to link multiple traces from the same conversation. For example, you might use a chat thread ID.
  - `disabled`: If True, the trace will not be recorded.
  - `metadata`: Optional metadata for the trace.
- **Spans** represent operations that have a start and end time. Spans have:
  - `started_at` and `ended_at` timestamps.
  - `trace_id`, to represent the trace they belong to
  - `parent_id`, which points to the parent Span of this Span (if any)
  - `span_data`, which is information about the Span. For example, `AgentSpanData` contains information about the Agent, `GenerationSpanData` contains information about the LLM generation, etc.

## Default tracing

By default, the SDK traces the following:

- The entire `Runner.{run, run_sync, run_streamed}()` is wrapped in a `trace()`.
- Each time an agent runs, it is wrapped in `agent_span()`
- LLM generations are wrapped in `generation_span()`
- Function tool calls are each wrapped in `function_span()`
- Guardrails are wrapped in `guardrail_span()`
- Handoffs are wrapped in `handoff_span()`
- Audio inputs (speech-to-text) are wrapped in a `transcription_span()`
- Audio outputs (text-to-speech) are wrapped in a `speech_span()`
- Related audio spans may be parented under a `speech_group_span()`

By default, the trace is named "Agent trace". You can set this name if you use `trace`, or you can can configure the name and other properties with the [`RunConfig`][agents.run.RunConfig].

In addition, you can set up [custom trace processors](#custom-tracing-processors) to push traces to other destinations (as a replacement, or secondary destination).


## Higher level traces

Sometimes, you might want multiple calls to `run()` to be part of a single trace. You can do this by wrapping the entire code in a `trace()`.

```python
from agents import Agent, Runner, trace

async def main():
    agent = Agent(name="Joke generator", instructions="Tell funny jokes.")

    with trace("Joke workflow"): # (1)!
        first_result = await Runner.run(agent, "Tell me a joke")
        second_result = await Runner.run(agent, f"Rate this joke: {first_result.final_output}")
        print(f"Joke: {first_result.final_output}")
        print(f"Rating: {second_result.final_output}")


1. Because the two calls to Runner.run are wrapped in a with trace(), the individual runs will be part of the overall trace rather than creating two traces.
Creating traces
You can use the [trace()][agents.tracing.trace] function to create a trace. Traces need to be started and finished. You have two options to do so:
1. 
Recommended: use the trace as a context manager, i.e. with trace(...) as my_trace. This will automatically start and end the trace at the right time.

1. You can also manually call [trace.start()][agents.tracing.Trace.start] and [trace.finish()][agents.tracing.Trace.finish].
The current trace is tracked via a Python contextvar. This means that it works with concurrency automatically. If you manually start/end a trace, you'll need to pass mark_as_current and reset_current to start()/finish() to update the current trace.
Creating spans
You can use the various [*_span()][agents.tracing.create] methods to create a span. In general, you don't need to manually create spans. A [custom_span()][agents.tracing.custom_span] function is available for tracking custom span information.
Spans are automatically part of the current trace, and are nested under the nearest current span, which is tracked via a Python contextvar.
Sensitive data
Certain spans may capture potentially sensitive data.
The generation_span() stores the inputs/outputs of the LLM generation, and function_span() stores the inputs/outputs of function calls. These may contain sensitive data, so you can disable capturing that data via [RunConfig.trace_include_sensitive_data][agents.run.RunConfig.trace_include_sensitive_data].
Similarly, Audio spans include base64-encoded PCM data for input and output audio by default. You can disable capturing this audio data by configuring [VoicePipelineConfig.trace_include_sensitive_audio_data][agents.voice.pipeline_config.VoicePipelineConfig.trace_include_sensitive_audio_data].
Custom tracing processors
The high level architecture for tracing is:
* 
At initialization, we create a global [TraceProvider][agents.tracing.setup.TraceProvider], which is responsible for creating traces.

* We configure the TraceProvider with a [BatchTraceProcessor][agents.tracing.processors.BatchTraceProcessor] that sends traces/spans in batches to a [BackendSpanExporter][agents.tracing.processors.BackendSpanExporter], which exports the spans and traces to the OpenAI backend in batches.
To customize this default setup, to send traces to alternative or additional backends or modifying exporter behavior, you have two options:
1. 
[add_trace_processor()][agents.tracing.add_trace_processor] lets you add an additional trace processor that will receive traces and spans as they are ready. This lets you do your own processing in addition to sending traces to OpenAI's backend.

1. [set_trace_processors()][agents.tracing.set_trace_processors] lets you replace the default processors with your own trace processors. This means traces will not be sent to the OpenAI backend unless you include a TracingProcessor that does so.   External tracing processors list

Weights & Biases

Arize-Phoenix

Future AGI

MLflow (self-hosted/OSS

MLflow (Databricks hosted

Braintrust

Pydantic Logfire

AgentOps

Scorecard

Keywords AI

LangSmith

Maxim AI

Comet Opik

Langfuse

Langtrace

Okahu-Monocle


# Agent Visualization

Agent visualization allows you to generate a structured graphical representation of agents and their relationships using **Graphviz**. This is useful for understanding how agents, tools, and handoffs interact within an application.

## Installation

Install the optional `viz` dependency group:

```bash
pip install "openai-agents[viz]"

Generating a Graph
You can generate an agent visualization using the draw_graph function. This function creates a directed graph where:
* 
Agents are represented as yellow boxes.

Tools are represented as green ellipses.

* Handoffs are directed edges from one agent to another.
Example Usage
from agents import Agent, function_tool
from agents.extensions.visualization import draw_graph

@function_tool
def get_weather(city: str) -> str:
    return f"The weather in {city} is sunny."

spanish_agent = Agent(
    name="Spanish agent",
    instructions="You only speak Spanish.",
)

english_agent = Agent(
    name="English agent",
    instructions="You only speak English",
)

triage_agent = Agent(
    name="Triage agent",
    instructions="Handoff to the appropriate agent based on the language of the request.",
    handoffs=[spanish_agent, english_agent],
    tools=[get_weather],
)

draw_graph(triage_agent)

￼
This generates a graph that visually represents the structure of the triage agent and its connections to sub-agents and tools.
Understanding the Visualization
The generated graph includes:
* 
A start node (__start__) indicating the entry point.

Agents represented as rectangles with yellow fill.

Tools represented as ellipses with green fill.

Directed edges indicating interactions:
* 
Solid arrows for agent-to-agent handoffs.

* Dotted arrows for tool invocations.

* An end node (__end__) indicating where execution terminates.
Customizing the Graph
Showing the Graph
By default, draw_graph displays the graph inline. To show the graph in a separate window, write the following:
draw_graph(triage_agent).view()

Saving the Graph
By default, draw_graph displays the graph inline. To save it as a file, specify a filename:
draw_graph(triage_agent, filename="agent_graph")

This will generate agent_graph.png in the working directory.

