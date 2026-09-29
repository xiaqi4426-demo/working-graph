# Cognitive Graph

**An external problem topology for agents.**

Cognitive Graph is a concept for giving agents a lightweight, editable map of the problem they are currently solving.

Most agent systems already have some combination of:

- conversation / transcript — what happened;
- memory — what should be retained across time;
- context — what the model should see right now;
- plans or to-do lists — what to do next;
- artifacts — files, code, data, notes, experiments, and other concrete outputs.

What is often missing is an explicit representation of:

> **What does the problem currently look like? Where am I in it? What other points exist, and how are they connected?**

A complex problem is rarely a list. It branches, converges, produces side questions, dead ends, dependencies, unresolved points, and new directions. A conversation is linear, but the problem itself is not.

Cognitive Graph is intended to represent that missing structure.

## Core idea

The system is deliberately small:

```text
Map
  ↓
Node Card
  ↓
Underlying Artifacts
```

- **Map** shows the topology: what points exist, which points are connected, direction when useful, and current focus.
- **Node Card** explains what a point means, why it matters, how it relates to nearby points, and where to look for more detail.
- **Artifacts** contain the actual detailed material: Markdown, code, experiments, data, commits, papers, transcripts, etc.

The graph is not a second source of truth for those artifacts. It is an external working model of the problem space.

## Minimal concepts

### Node

A node means:

> There is a point in the problem space worth being able to return to.

A node does **not** need a predefined type such as `Task`, `Hypothesis`, `Bug`, or `Decision`.

A minimal node needs:

```text
id
title
summary
refs
```

The `summary` matters: a title alone is often not enough for an agent to decide whether a node is worth exploring.

Nodes may also be incomplete. A placeholder such as:

```text
Migration semantics ?
```

is valid even when the agent has not yet investigated it.

### Edge

An edge means:

> These two points are related.

The map only needs to distinguish, at most:

```text
A ─── B
A ───→ B
```

The arrow does not carry a fixed ontology such as `depends_on` or `supports`. It only preserves direction when direction is useful.

The detailed meaning of the relationship belongs in a short natural-language **relation summary**, surfaced in the relevant node card.

### Focus

An agent should not need to load the whole graph.

It can work from a current focus node or subgraph, inspect nearby cards, and open referenced artifacts only when needed.

This enables progressive disclosure:

```text
Map
↓
Card
↓
Artifact
```

## What Cognitive Graph is not

Cognitive Graph is not:

- a knowledge graph ontology;
- a task manager;
- a replacement for memory;
- a replacement for context;
- a full project-management system;
- a requirement that every thought become a node;
- a requirement that one project have one giant graph.

The design principle is:

> **Schema-light, protocol-strong.**

The harness should define how nodes are addressed, inspected, connected, referenced, focused, updated, and traversed.

It should avoid defining what kinds of things the agent is allowed to think about.

## Why it may matter

A Cognitive Graph gives an agent somewhere to put a thought without either:

1. pursuing it immediately and losing focus, or
2. dropping it and hoping it is remembered later.

It can also provide a shared coordination surface for multiple agents. Different agents may work on different nodes or subgraphs while sharing the same evolving problem topology.

## Status

This repository currently captures the concept only.

The next useful step is not to design a large graph framework. It is to dogfood the smallest possible protocol on real work — architecture design, coding/debugging, research, and paper analysis — and observe whether agents actually use the graph to stay oriented, preserve side branches, and navigate artifacts more effectively.

See [docs/concept.md](docs/concept.md) for the full concept.
