---
name: working-graph
description: Maintain a lightweight external problem topology for complex, branching, or long-running work. Use when a task has multiple interacting questions, branches, deferred issues, repeated revisiting, or parallel workstreams and a linear todo list is no longer enough. Do not use for simple, short, or obviously linear tasks.
---

# Working Graph

Use a Working Graph to keep the structure of a complex problem explicit while working.

The graph is not a knowledge ontology, not a project-management system, and not a replacement for context, memory, transcripts, plans, or artifacts.

Its job is narrower:

> Preserve the topology of the problem: what points exist, how they are connected, where the current focus is, and where deeper material can be found.

## Core model

Work in three layers:

- **Map**: a lightweight view of the problem's shape.
- **Node Card**: a compact explanation of one point and its nearby relationships.
- **Artifacts**: the detailed files, code, notes, data, experiments, commits, sources, or transcripts referenced by the node.

The graph should help you stay oriented. It should not become a second copy of the work.

## Storage

For project-scoped work, keep the active graph at:

```text
.working-graph/graph.md
```

If an existing Working Graph is present, continue it. Do not replace it with a new graph unless the user explicitly asks.

If no graph exists and this skill is useful, initialize one using [graph-template.md](graph-template.md). Resolve bundled template and example links relative to this skill's directory. Keep the active graph in the user's project, not in the installed skill directory.

The graph file is working state. Referenced artifacts remain the source material for detailed claims and evidence. Resolve project file refs from the project root unless the graph declares another base.

Use an existing graph for the same problem. If its scope is unrelated, use a separate, clearly named graph alongside it and state which one is active. When file access is unavailable, explain that persistence is unavailable in this environment; do not claim the graph was saved.

## Start of work

When entering a complex task with an existing graph:

1. Read the graph's **Focus**, **Map**, and the card for the focused node.
2. Read relation summaries for the focused node's immediate neighbors.
3. Open referenced artifacts only when the card or current task makes them relevant.
4. Continue from the current topology instead of reconstructing the task from scratch.

When initializing a graph:

1. Create only the few nodes already necessary to orient the current work.
2. Set one current focus.
3. Add obvious relationships.
4. Do not attempt to model the whole project in advance.

## Nodes

A node means:

> This is a point in the problem space worth being able to return to.

Do not require a predefined node type.

A node may represent a question, concern, hypothesis, bug, decision, finding, branch, code area, paper claim, constraint, or anything else useful to the current problem.

Each node card should contain:

- a stable short ID;
- a concise title;
- a short summary;
- zero or more refs.

The summary is required unless the node is an intentionally minimal placeholder. It should explain what the point means and why it exists in the current problem.

Keep summaries compact. They are for orientation and relevance judgment, not for duplicating the referenced artifacts.

### Placeholder nodes

A node may exist before it is understood.

Create a placeholder when you notice a meaningful direction that should not be pursued now.

Example:

```text
Migration semantics ?

Likely relevant to versioning; not yet explored.
```

Creating a placeholder should normally allow you to preserve the idea and return to the current focus.

## Edges

An edge means:

> These two nodes are meaningfully related.

Keep edges simple.

Use either:

```text
A --- B
```

or:

```text
A --> B
```

Use direction only when direction helps preserve how the relationship should be read.

Do not impose a fixed edge ontology such as `supports`, `depends_on`, `implements`, or `contradicts`.

Instead, attach a short natural-language **relation summary** explaining why the two nodes are connected.

The map carries connection. The relation summary carries explanation.

Store each relationship explanation once in the Relations section. When inspecting a node, treat all relation records touching that node as part of its card.

## Refs

Use refs to point from the graph to detailed material.

Refs may point to:

- Markdown files or sections;
- source code files or symbols;
- tests;
- datasets or revisions;
- experiment outputs;
- commits or issues;
- papers or web sources;
- transcripts or records;
- another graph.

Do not copy long artifact contents into node summaries.

A node may reference many artifacts. An artifact may be referenced by many nodes.

## Focus

Maintain one explicit current focus unless the task genuinely requires several simultaneous focal points.

The focus answers:

> Where are we working now?

When switching focus:

1. make the previous node's current state understandable;
2. preserve newly discovered branches as nodes if they matter;
3. set the new focus;
4. load only the new focus card, nearby relation summaries, and relevant refs.

Do not chase every newly created node immediately.

## Updating the graph

Update the graph when the **problem topology changes**, not after every action.

Good reasons to update include:

- a new independently meaningful point appears;
- two existing points are discovered to be related;
- a node is split because it became too broad;
- two nodes are discovered to represent the same point;
- a branch is resolved, rejected, deferred, or becomes important again;
- the current focus changes;
- an artifact becomes an important reference for a node.

Do not create nodes for:

- every tool call;
- every command;
- every sentence of reasoning;
- routine implementation steps that do not matter to the problem structure;
- transient details that are already clear from the active context.

## Staying focused

When a new issue appears during deep work, choose deliberately:

- **Pursue now** if it blocks or materially changes the current focus.
- **Create a placeholder and continue** if it matters but is not currently blocking.
- **Ignore it** if it does not deserve a stable place in the problem topology.

This is one of the main purposes of the graph.

## Rejected, completed, and deferred branches

Do not automatically delete a node when work stops on it.

Keep a node when its existence still explains the problem or prevents repeated work.

For example, a rejected direction may remain because knowing *why it was rejected* is useful later.

Use ordinary natural language to describe node state. Do not force a fixed status taxonomy unless the surrounding system already has one.

## Reading the graph

Use progressive disclosure.

First inspect the Map.

Then inspect the relevant Node Card and its immediate relation summaries.

Only then open refs that are needed.

Do not load the entire graph or all referenced artifacts into context merely because they exist.

The graph is a navigation structure, not a context dump.

## Multiple agents

When the surrounding task already uses multiple agents, a node or local subgraph can be a unit of delegation.

Prefer giving a delegated agent:

- the focus node;
- its summary;
- its immediate relations;
- relevant refs;
- the upstream goal or surrounding context needed to understand why the node matters.

Do not require a subagent to read the entire project graph.

When a subagent returns, integrate topology-changing results into the shared graph rather than copying its full transcript into the graph.

Examples of useful graph deltas:

- resolved node A;
- added node X;
- connected X to B;
- parked node C;
- attached artifact R.

Do not invent a complex multi-agent scheduler just to use this skill.

## Meta-rules for graph growth

Follow these rules:

1. Create a node when a point becomes independent enough that returning to it later would be useful.
2. Create a placeholder when a meaningful direction appears but should not interrupt the current focus.
3. Connect existing nodes instead of duplicating the same explanation into several places.
4. Split a node when keeping it as one point starts to hide important structure.
5. Merge or canonicalize nodes when they turn out to represent the same point.
6. Preserve meaningful rejected or failed directions when doing so can prevent repeated work.
7. Keep the map light; keep explanation in cards; keep detail in artifacts.
8. Treat summaries and relation descriptions as working interpretations, not unquestionable truth.
9. Let the graph grow from the task. Do not force the task into a predefined graph shape.
10. Prefer the smallest graph that still preserves orientation.

## Design principle

The skill is intentionally:

> **Schema-light, protocol-strong.**

Do not invent a universal taxonomy of thought.

Be precise about how the graph is read and maintained, but let the agent decide what deserves a node and how the problem grows.

For the file format and editing conventions, read [graph-template.md](graph-template.md) when initializing or repairing a graph.

For concrete examples of good and bad graph behavior, read [examples.md](examples.md) only when needed.
