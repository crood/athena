![Athena Mini inside Obsidian](<assets/AthenaMini.png>)

# Athena Mini - First Look Inside Obsidian

One of the first places I wanted Athena to exist outside its own window was inside Obsidian. Obsidian does such a great job at being a second brain and note taking app. And I wanted to rather integrate my systems into it, instead of cluttering the Harness with an extension which would attempt to replace Obsidian.

It was important to me that this implementation is not just another chatbot pasted into the sidebar, but something that genuinely understands the vault and what's in it, how notes relate to each other, and what's worth surfacing when you ask a question.

Athena Mini is the early result of that experiment.

![Book logo](/least-github-pages/assets/logo.png)

![Athena Mini inside Obsidian](/assets/AthenaMiniObsidian.png)

---

## What it's trying to do

The goal is not retrieval for its own sake. It's building a workspace where Athena can navigate knowledge the same way a person would. Following relationships between notes, understanding which documents are relevant to a question, and doing that without loading the entire vault into a context window every time.

The approach combines a few things:

- **Lexical search** - find actual deterministic data first instead of doing predictictions over a large collection of data
- **Semantic search over the vault** - notes are embedded and indexed so that queries match on meaning, not just keywords. Combine with deterministic search to raise the chance of obtaining meaningful results.
- **Vector retrieval** - relevant documents are pulled based on the actual question, not a predetermined structure
- **Graph-aware context** - Obsidian's link graph adds structural signals on top of semantic similarity; a note that is heavily linked tends to be more foundational
- **Real-time event markers** - because reads and edits come from Athena's own tool layer, the graph can show which nodes Athena is currently touching

That last point is the part that's hardest to replicate with a third-party plugin. Most "AI in Obsidian" tools are stateless. They call an API and display the result. Athena Mini knows what it's doing because the harness knows what it's doing.

![Athena — system overview](<assets/GraphBackend.png>)
`database, embedding models and structural graph YAML index settings`

---

## The graph as interface

The most interesting design territory is using Obsidian's graph view not just for navigation, but as a live display of Athena's activity.

When Athena reads a note to answer a question, that note can be highlighted in the graph. When it edits or annotates something, the change is visible as a graph state, not just a file diff. The result is a kind of spatial awareness. You can watch Athena move through your knowledge base in real time.

![Athena — system overview](<assets/Athena Mini - Obsidian_Graph.png>)
`user activity in the knowledge graph`

---

This isn't cosmetic. It makes the system's reasoning more legible. If Athena answers a question about a project and you can see it moved through three specific notes to get there, you understand something about the quality of the answer that you wouldn't get from the text alone.

![Athena — system overview](<assets/Athena Mini - Obsidian_Graph2.png>)
`athena mini - traversing the knowledge graph (pink nodes)`

---

## Current state

Athena Mini is functional but still early. Semantic indexing works across a vault, retrieval is fast enough for interactive use, and the event hooks into Athena's tool layer are wired up.

The graph highlighting is partially implemented. Events fire correctly, but the Obsidian plugin side needs more work to reflect them smoothly without interrupting the user's own navigation.

The next area of focus is improving how Athena decides *when* to retrieve versus when the current context is sufficient, avoiding unnecessary searches that add latency without adding useful information.

More updates to follow as this develops.
