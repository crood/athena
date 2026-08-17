# Why Athena is Event-Driven

One of the early architectural decisions in Athena was to treat it as an event-emitting system rather than a purely conversational one.

This post explains what that means, why it matters, and what it makes possible.

---

## The problem with sourcing state from model output

In a typical AI harness, the model is the only visible actor. When it reads a file, it says it is reading a file. When it runs a tool, it reports what happened. The interface knows about system state only because the model described it.

This creates a category error. The model is a reasoning component. It is good at understanding problems, generating responses, and deciding what to do next. It is not especially good at serving as a reliable real-time state machine.

If you want to show a spinner while a tool is running, you're waiting for the model to start describing it. If you want to highlight a document in a knowledge graph while Athena reads it, you need the model to emit that information in a parseable way — which means prompting for it, which means paying for it.

---

## What the harness already knows

Athena takes a different position: **if the harness is performing an operation, it knows it is performing that operation.** This is just a fact about software. You don't need to ask the model.

So Athena emits events directly from its own infrastructure:

```
tool.requested       — the model has asked to run a tool
tool.started         — the harness is executing it
tool.completed       — execution finished, result ready

file.reading         — a file tool is in progress
file.editing         — a write operation is underway

approval.requested   — a sensitive action needs confirmation
approval.accepted    — the user approved
approval.rejected    — the user declined

agent.started        — a sub-agent was spawned
agent.completed      — the sub-agent finished
```

These events originate from the harness, not from model output. They are deterministic and don't consume context window.

---

## What this makes possible

### Interfaces that react to real state

Any client connected to Athena via its API can subscribe to these events and update its UI accordingly — without needing the model to narrate what it's doing.

An Obsidian plugin can highlight the note Athena is currently reading. Not because the model said "I am reading this note," but because the harness emitted a `file.reading` event with a path.

A Galaxy Watch companion can show a "thinking" animation when `model.thinking` fires and clear it when the response arrives. Pure infrastructure signal — zero model tokens.

### Approval routing that makes sense

When Athena encounters a sensitive action, it emits `approval.requested` and pauses. Any client that handles approvals can respond — the desktop interface, a web client, a watch app. The approval isn't baked into the chat window; it's a first-class event that can go wherever you are.

### Better debugging

When something goes wrong, event logs give you a timestamped sequence of what the harness actually did. Not what the model claimed it did — what happened.

---

## The tradeoff

The event architecture requires more upfront design. Every tool call, state transition, and approval needs to be wired into the event system rather than inferred from model output.

This pays for itself quickly. But it does mean Athena's architecture is more explicit than a harness that just lets the model run and scrapes its output.

That explicitness is intentional. The point of Athena is to make the infrastructure visible.

---

## Next steps

The current event system covers the core tool and approval lifecycle. I'm working on extending it to agent orchestration and memory retrieval — so that when Athena queries a vector store or triggers a semantic search, those operations are also surfaced as events rather than opaque model steps.

More in a future post.
