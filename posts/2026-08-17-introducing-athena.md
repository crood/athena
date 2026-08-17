# Introducing Athena

For the last eight months I've been using, testing, and breaking almost every AI harness available — the major cloud ones, and a good portion of the open-source field.

Most of them are genuinely impressive in parts. Some have ideas I haven't seen elsewhere. None of them were the thing I actually wanted to build with.

This is a summary of what's missing and what Athena is trying to be instead.

---

## What I kept running into

The problems roughly cluster into two categories depending on whether you're using a cloud harness or an open-source one.

**Cloud harnesses** (Claude, Codex, Cursor) are often excellent — but opaque by design. You can't see how much context is being sent between turns. You don't control which tool schemas are included. You don't know what sub-agents are being spawned, what they cost, or what they do. For a broad audience this is probably fine. For building on top of them, it's frustrating.

**Open-source harnesses** give you more control, but the tradeoff is fragmentation. Each one reflects one person's opinion about what the perfect harness should do. Most require Docker, or have key features locked behind paywalls, or assume a particular model family. Local model support is usually an afterthought.

The deeper problem across all of them is that **none of them feel connected to you**. They can read your notes folder and search your documents. But how your information is stored, retrieved, and weighted isn't something you can inspect or adjust without digging into source code.

---

## What I actually wanted

The clearest way to put it: I wanted an AI system where the infrastructure is visible.

Specifically:

- **Model agnostic** — not just in principle, but in practice. Different tasks need different models. Swapping should be trivial.
- **Context you control** — which tool schemas are included, how much history is sent, when to compact. Not something you change manually every session.
- **Human-in-the-loop** — a real approval layer, not just a confirmation dialog. Sensitive actions should be pauseable and routable to wherever you happen to be.
- **Events from infrastructure** — if the harness is reading a file, it knows it is reading a file. The model doesn't need to tell the UI about it.
- **Memory you can reason about** — not a black box vector store. Structured enough that you can understand what Athena knows and why.
- **Interfaces that make sense for the moment** — a chat window is one interface. A knowledge graph is another. A watch. A piece of e-paper. The harness shouldn't assume which one you need.

---

## What Athena is

Athena is a central harness that coordinates models, tools, agents, memory, events, and external interfaces — without belonging to any one provider or device.

Around the core harness sits a growing ecosystem of companion projects that extend Athena into different environments: an Obsidian knowledge workspace, a Galaxy Watch companion, a reMarkable integration, and an Identity Engine for structured personal history.

None of these extensions are required. They exist to broaden Athena's reach.

---

## Where things stand

Athena is under active development. Parts of it are functional. Parts are prototypes. Parts are ideas I haven't built yet.

This blog will track progress honestly — what works, what doesn't, what I'm currently thinking about, and what I've abandoned.

If you're interested in AI harness architecture, local LLMs, unusual interfaces, or the question of what an AI system can become when the model is only the beginning — there will be something here worth reading.

More soon.
