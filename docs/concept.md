# Cognitive Graph — Concept v0.1

> Naming note: **Working Graph** is the public name of the skill in this repository. This original concept note uses **Cognitive Graph** for the broader idea; the skill implements the working-graph form in §10.2. Other lifecycle and runtime possibilities below are conceptual directions, not shipped features.

## 1. Motivation

Complex problem solving is not naturally linear.

A problem may begin as one question:

```text
A
```

and quickly become:

```text
                A
          ┌─────┼─────┐
          B     C     D
        / | \
      B1 B2 B3
         /|\
        X Y Z
```

But most agent interaction is recorded and consumed as a sequence:

```text
A
↓
B
↓
B2
↓
X
↓
discussion of X
↓
discussion of Y
↓
return to C
↓
revise B
...
```

The problem is a graph. The conversation is a line.

This mismatch produces familiar failure modes:

- a local branch becomes deep enough that the global structure is forgotten;
- several issues become mixed together and their relationships are lost;
- a newly noticed issue either causes immediate drift or disappears;
- old questions are reopened because the agent no longer remembers why they were rejected;
- multiple threads are discussed without a shared view of which one is current;
- after context compression or a new session, the agent reconstructs the problem differently;
- several agents duplicate work or lose the relationship between their local tasks and the overall problem.

These are not only memory failures.

They are failures to preserve **problem topology**.

Cognitive Graph is intended to make that topology explicit.

---

## 2. Definition

**Cognitive Graph is an external, editable representation of the structure of an ongoing problem.**

It gives an agent a place to represent:

- points that matter;
- relations between those points;
- current focus;
- unexplored or deferred directions;
- rejected or completed branches;
- references to the detailed material behind a point.

It is a working model, not an ontology of reality.

The graph may be temporary or persistent. It may be used by one agent, several agents, or a human and agents together.

---

## 3. Boundary with context, memory, records, plans, and artifacts

Cognitive Graph is distinct from several nearby concepts.

| System | Primary question |
|---|---|
| Record / transcript | What happened? |
| Memory | What is worth retaining across time? |
| Context | What should the model see right now? |
| Plan / todo | What should be done next? |
| Artifact | Where is the concrete material? |
| **Cognitive Graph** | **What does the problem currently look like? Where am I in it, and how are the important points connected?** |

These systems can interact without being collapsed into one another.

A node can exist in the graph without being in current context.

A node can exist in the graph without being worth long-term memory.

A rejected node can remain in the graph because it still explains part of the problem's history and prevents repeated exploration.

A memory can matter to the agent without becoming a node in the current graph.

A todo list can be derived from graph state instead of being a separate source of structure.

For example:

```text
Cognitive Graph
      ↓
pending + actionable nodes
      ↓
Todo View
```

Cognitive Graph may also become an upstream signal for context selection:

```text
Current focus
      ↓
Local graph neighborhood
      ↓
Relevant node cards and refs
      ↓
Context
```

---

## 4. The three-layer model

The current concept uses three layers:

```text
Map
  ↓
Node Card
  ↓
Underlying Artifacts
```

### 4.1 Map

The map answers:

> What does the problem look like?

It should stay visually and cognitively light.

It may show:

- nodes;
- connections;
- optional direction;
- current focus;
- possibly runtime overlays such as which agent is working where.

The map should not try to carry all semantics.

Example:

```text
              Configuration
              /      |      \
             /       |       \
      Authority    Merge    Storage
                             |
                       Local auth
```

The map is primarily for orientation and navigation.

### 4.2 Node Card

The node card answers:

> What is this point, why does it matter, how is it related to nearby points, and where can I learn more?

A minimal card contains:

```text
Title

Summary
A short explanation of what this point means and why it exists.

Relations
Nearby nodes, with short natural-language explanations of why they are related.

Refs
Pointers to detailed material.
```

Example:

```text
Storage scope

Summary
Configuration state has a separate question of where it is stored
and whether it propagates through synchronization. This is distinct
from authority and merge semantics.

Relations

Configuration
Storage scope fills a dimension the earlier configuration model could
not express.

Local authorization
Local-only authorization is a concrete case where storage locality matters.

Refs
configuration.md#storage
AGENTS.md#configuration
```

The card is the agent's basic unit for understanding the graph.

### 4.3 Artifacts

Artifacts contain the detail.

They may include:

- Markdown documents;
- sections of documents;
- code;
- test results;
- experiment outputs;
- datasets or revisions;
- commits;
- issues;
- papers;
- web sources;
- transcripts;
- other graphs.

The graph should point to artifacts rather than duplicating them.

A node and an artifact are not the same primitive.

---

## 5. Nodes

A node means:

> There is a point in the problem space that is worth being able to return to.

The harness should not require a global ontology of node types.

A node may represent, depending on the task:

- a question;
- a subproblem;
- a design concern;
- a hypothesis;
- a bug;
- a finding;
- a decision;
- a possible direction;
- a code area;
- a paper claim;
- a constraint;
- something else the agent finds useful.

The same protocol should work across architecture design, coding, debugging, research, paper analysis, and other complex work.

### 5.1 Minimal node information

Conceptually, a node needs only:

```text
id
title
summary
refs
```

This is not yet a final storage schema.

#### `id`

The node must be addressable.

#### `title`

A short label suitable for the map.

#### `summary`

A compact working interpretation of the node.

The summary is necessary because a title alone often does not contain enough information for an agent to decide whether the node is relevant.

The summary is not necessarily a document summary. It may represent an interpretation that only exists at the level of the working problem.

#### `refs`

Pointers to deeper material.

A node may have zero, one, or many refs.

Multiple nodes may point to the same artifact or to different regions of the same artifact.

### 5.2 Placeholder nodes

A node does not need to be fully understood before it exists.

Example:

```text
Migration semantics ?
```

with a summary such as:

```text
Likely relevant to versioning; not yet explored.
```

This gives an agent a third option between:

- immediately pursuing every new idea, and
- losing the idea.

A placeholder can reserve a location in the problem topology without forcing immediate investigation.

### 5.3 Rejected, completed, and deferred nodes

A node does not necessarily disappear when it stops being active.

Keeping a rejected branch may prevent the agent from repeating a failed path.

Keeping a completed branch may preserve the shape of how the solution was reached.

Keeping a deferred branch allows an agent to remain focused without losing the existence of a later concern.

The graph should therefore support evolution rather than treating deletion as the only way to stop working on a node.

---

## 6. Edges

An edge means:

> These two nodes are related.

The map does not need a large edge ontology.

At minimum, it may distinguish:

```text
A ─── B
```

and:

```text
A ───→ B
```

The first represents a relationship where direction is unnecessary.

The second preserves direction when direction is useful.

### 6.1 Direction does not define ontology

`A → B` must not automatically mean:

- depends on;
- supports;
- causes;
- implements;
- blocks;
- derives from.

The arrow only preserves a useful direction.

The actual relationship is explained in natural language.

### 6.2 Relation summary

Conceptually, a connection may carry:

```text
node A
node B
direction
summary
```

Again, this is not a final schema.

The summary answers:

> Why are these two points connected?

Example:

```text
Storage scope ─── Local authorization

Relation summary:
Local-only authorization exposed the need to model storage locality as
an independent configuration dimension.
```

This avoids prematurely forcing complex real-world relationships into a growing list of rigid relation types.

### 6.3 Map relation vs. explanation

The map only needs to show that a relationship exists and, optionally, its direction.

The explanation is surfaced when the agent inspects the relevant card.

This separation keeps the map small without making the graph semantically empty.

---

## 7. Focus and progressive disclosure

An agent should not need the entire graph in context.

It can operate from a current **focus**.

For example:

```text
FOCUS: Storage scope
```

The runtime may initially provide:

```text
Storage scope
summary: ...

neighbors:
- Configuration
- Local authorization
- Sync semantics
```

The agent can then decide whether a neighbor is worth inspecting.

If so:

```text
inspect(Local authorization)
```

The card provides more information.

Only if deeper evidence is needed does the agent open refs.

This gives three levels of information cost:

```text
Map
↓
cheap orientation

Card
↓
compact semantic understanding

Artifact
↓
full detail and evidence
```

This can support **agent-controlled structural retrieval**.

Instead of the system always deciding which full text chunks should enter context, the agent can navigate the problem topology and choose where to look deeper.

---

## 8. The graph should grow with the agent

The system should not prescribe a fixed diagram for each problem domain.

It should provide a small set of graph operations and a set of meta-rules.

The agent decides what points deserve nodes and how the graph should evolve.

This is the central design principle.

### 8.1 Candidate meta-rules

These are conceptual rules, not yet a final prompt or protocol:

1. Create a node when a point has become sufficiently independent that it is worth returning to later.
2. When a new direction matters but should not be pursued now, create a placeholder and keep the current focus.
3. When two existing points have an important relationship, connect them instead of duplicating the same content into both.
4. When a node becomes too complex to understand as one point, split it.
5. When two nodes are discovered to represent the same point, merge them or establish one as canonical.
6. Do not turn every action, tool call, sentence, or transient thought into a node.
7. Prefer preserving a meaningful rejected or failed direction over deleting it when that history may prevent repeated work.
8. Use refs for detailed evidence rather than copying artifact contents into the graph.
9. Keep the map light; place explanation in cards and source material in artifacts.
10. Treat the graph as a working interpretation, not as unquestionable truth.

The purpose of these rules is to guide graph growth without defining what the graph must contain.

---

## 9. Schema-light, protocol-strong

Cognitive Graph should be:

> **Schema-light, protocol-strong.**

### Schema-light

Avoid prematurely defining:

- a universal `NodeType` hierarchy;
- dozens of `EdgeType` values;
- a fixed research workflow;
- a fixed coding workflow;
- a fixed architecture-design workflow;
- a universal ontology of problem solving.

The graph should be able to adapt to the agent's task.

### Protocol-strong

The harness should be precise about mechanics such as:

- how nodes are addressed;
- how a node is inspected;
- how nodes reference artifacts;
- how nodes are connected;
- how direction is represented;
- how summaries are surfaced;
- how focus is represented;
- how local neighborhoods are read;
- how nodes and edges are updated or retracted;
- how graph revisions are handled if concurrency becomes necessary.

The system defines the rules of the medium, not the content of thought.

---

## 10. Lifecycles

A graph does not need one universal persistence model.

The concept currently allows multiple lifecycles.

### 10.1 Ephemeral graph

A temporary scratch graph for a local investigation.

Example:

```text
Crash
├── parser?
├── cache?
├── bad input?
└── concurrency?
```

Once the issue is resolved, most of this graph may be discarded.

### 10.2 Working graph

A graph that survives across multiple turns, context compressions, sessions, or agent runs while a problem remains active.

This is the primary form discussed in this document.

### 10.3 Durable graph

Some parts of a working graph may become valuable long-term structure.

Those parts can later be promoted into a durable graph or another persistent knowledge representation.

The entire working graph should not automatically become permanent.

The promotion policy is deliberately unspecified at this stage.

### 10.4 Multiple graphs

A project does not need one giant graph.

Several graphs may coexist, for example:

```text
Project
├── architecture graph
├── bug investigation graph
├── release planning graph
└── temporary scratch graphs
```

Graphs may reference each other if useful.

The concept does not yet require a special "graph of graphs" abstraction.

---

## 11. Multi-agent work

Cognitive Graph may also serve as a shared coordination surface for multiple agents.

The key idea is that an agent can receive a **position in the problem space**, not just an isolated prompt.

Example:

```text
                    Main question
                  /       |       \
                 /        |        \
             Branch A   Branch B   Branch C
                │          │          │
             Agent 1    Agent 2    Agent 3
```

An agent working on a node can know:

- why the node exists;
- its parent or surrounding structure;
- nearby nodes;
- relevant refs;
- which results may affect other points.

Different agents can therefore work on different frontiers without losing the common topology.

### 11.1 Focus as a unit of delegation

Delegation may eventually look more like:

```text
delegate(node)
```

or:

```text
delegate(subgraph)
```

than:

```text
delegate(prompt)
```

The delegated agent receives the node card, local neighborhood, relevant refs, and whatever additional context is needed.

### 11.2 Graph delta instead of transcript replay

A parent agent should not necessarily need to reread every sub-agent transcript.

A sub-agent may update the shared problem structure with a compact delta such as:

```text
resolved node A
added node X
connected X to B
deferred node C
attached artifact R
```

The detailed record remains available through refs if needed.

### 11.3 Concurrency is an implementation concern, not a v0 concept requirement

If multiple agents edit the same graph, revision control, optimistic concurrency, proposals, or merge semantics may eventually be needed.

Those mechanisms should not be designed prematurely.

For now, it is enough that the conceptual model does not prevent multiple agents from working on different local regions.

---

## 12. Human-agent collaboration

The same graph can be read differently by humans and agents.

A human may prefer:

- a visual graph;
- an outline;
- a tree;
- a table;
- a kanban-like view.

An agent may prefer:

- structured neighborhood queries;
- card inspection;
- ref traversal;
- focus-aware context loading.

These can be projections over the same underlying graph.

The map visualization is therefore not the graph itself.

A human can also modify the working topology directly:

- defer a node;
- add a missing point;
- change focus;
- attach a reference;
- connect two concerns;
- mark a direction as not worth pursuing.

This gives humans and agents a shared external representation rather than requiring the human to repeatedly correct the agent's implicit mental model through conversation alone.

---

## 13. What we deliberately do not design yet

The concept is intentionally stopped before several implementation choices.

Do **not** define yet:

- a universal node taxonomy;
- a large edge ontology;
- a complete graph storage schema;
- a graph database;
- automatic graph promotion rules;
- automatic community detection;
- GraphRAG or embedding integration;
- a multi-agent scheduler;
- a complex permission model;
- advanced graph UI;
- exact concurrency and merge semantics;
- a fixed lifecycle policy.

These decisions should be driven by real usage.

The next question is not:

> What other fields could the graph have?

It is:

> Does an agent actually work better when given this medium?

---

## 14. Minimal prototype boundary

A useful first prototype should remain deliberately small.

Conceptually, it only needs:

### Node

```text
id
title
summary
refs
```

### Edge

```text
node A
node B
direction
summary
```

### Runtime state

```text
focus
```

### Operations

A minimal interface might support:

```text
create node
update node
connect nodes
inspect node
list neighbors
set focus
retract / archive a node
open a ref
```

The exact names and storage format are implementation details.

The prototype should test whether agents naturally learn to:

- create useful nodes;
- preserve side branches without chasing them immediately;
- return to unresolved points;
- use summaries to decide when to inspect further;
- navigate refs instead of loading everything;
- preserve global orientation during deep local work;
- avoid repeatedly rediscovering rejected paths;
- maintain a useful graph without turning every thought into a node.

---

## 15. Evaluation through dogfooding

The first serious evaluation should use real complex work rather than synthetic benchmarks.

Good test cases include:

- software architecture design;
- coding and debugging;
- research;
- paper analysis;
- long-running project discussion.

A useful Cognitive Graph should reduce failures such as:

- losing the main thread;
- mixing unrelated concerns;
- reopening resolved issues;
- forgetting deferred issues;
- pursuing every new thought immediately;
- failing to understand why a local task matters to the whole;
- forcing a human to repeatedly restate the structure of the problem.

If the graph does not improve those behaviors, the concept should be revised before adding implementation complexity.

---

## 16. Core principles

The current concept can be summarized as follows.

### 1. Problem topology is a distinct form of state

Context, memory, transcript, and todo lists do not fully represent the structure of an ongoing problem.

### 2. Node carries meaning

A node marks a point worth returning to and has a short summary.

### 3. Edge carries connection

An edge says two points are related and may preserve direction.

### 4. Card carries explanation

The card explains what a node means, why nearby nodes matter, and where deeper material lives.

### 5. Map carries topology

The map shows the shape of the problem without trying to carry all details.

### 6. Artifacts carry detail and evidence

The graph points to source material instead of replacing it.

### 7. Incomplete structure is allowed

Placeholder nodes let an agent remember a direction without pursuing it immediately.

### 8. The graph is allowed to evolve

Nodes can be split, connected, deferred, rejected, completed, or eventually discarded.

### 9. The graph should be agent-grown

The harness supplies a medium and rules, not a predefined ontology of thought.

### 10. Persistence is contextual

Some graphs are disposable, some survive the active task, and some parts may become durable.

### 11. Local views matter more than dumping the whole graph

Agents should navigate through focus, cards, neighbors, and refs.

### 12. The same graph can coordinate multiple agents

Different agents may work on different nodes while sharing one evolving problem topology.

### 13. Schema-light, protocol-strong

The protocol should be precise while the content remains flexible.

---

## 17. Stopping point

The concept is sufficiently defined for a first experiment.

Further conceptual design is more likely to overfit imagined use cases than improve the idea.

The appropriate next step is to implement the smallest possible skill or prototype, use it on real tasks, and let actual agent behavior reveal what is missing.

The important thing to preserve is not a particular schema.

It is the central idea:

> **Give agents an external, editable map of the problem they are solving, so that complex work can remain structured without forcing the structure into a linear conversation, a fixed workflow, or a rigid ontology.**
