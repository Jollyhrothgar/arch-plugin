---
name: visualize
description: >-
  Investigate a software system and create a source-grounded, navigable HTML
  architecture explanation. Use when asked to "visualize the architecture",
  "show what talks to what", "where does this field come from", "can these
  outputs disagree", "trace this value through the system", or prepare an
  architecture explainer for a meeting, engineering handover, design decision,
  or incident write-up. Supports landscape/coordination and
  mechanism/provenance views. Do not use for generic UI mockups, metric
  dashboards, or diagrams copied from unverified descriptions.
user-invocable: true
---

# Visualize architecture

Turn a concrete architecture question into a source-grounded, navigable HTML
explanation. The diagram is not the goal. The goal is for a reader to understand
the causal structure well enough to explain it, coordinate work, or make a
decision without first reading the underlying code.

Read `references/design-patterns.md` before choosing the page structure or
visual language.

## When to use

Use this skill when the user needs to understand relationships that prose or a
flat component list hides, especially:

- which components communicate, through what interface, and with what payload;
- where one person's or team's work touches another's;
- how data changes as it crosses services, schemas, storage, or trust boundaries;
- which output is model-authored, deterministic, persisted, derived, or merely
  displayed;
- what guarantees an invariant and where disagreement can enter;
- which paths exist now, are being built, are being removed, or do not exist.

Do not use it merely because the word "architecture" appears. A prose answer is
better when the question has no meaningful spatial, causal, temporal, or
boundary structure. Use an ordinary charting or frontend workflow for metric
dashboards and product mockups.

## Inputs

Resolve these from the conversation and repository before asking the user:

- **Question:** the confusion the visualization must resolve.
- **Reader:** who will use it and what they already know.
- **Decision or action:** what the reader should be able to decide, explain, or
  coordinate afterward.
- **Scope:** repository, feature, workflow, incident, or code path, including
  source revisions and observation dates.
- **Output destination:** a user-specified path, or the default described below.

If the question or intended reader cannot be inferred, ask one focused question:
"What should the reader understand or decide after seeing this?" Do not begin by
asking the user to choose a diagram type. That choice follows from the evidence.

## Output contract

Produce one HTML file that can be opened directly in a browser. Prefer:

1. the path the user requested;
2. a repository's existing, gitignored `.local/` directory;
3. otherwise, a temporary directory and a clearly reported path.

Before using `.local/`, verify that it is ignored. Do not introduce a tracked
artifact or modify application code unless the user asked for a durable,
version-controlled document.

The page must include:

- a title and one-sentence answer to the driving question;
- a visual whose nodes and edges carry domain meaning;
- a prose explanation of the important path through the visual;
- the decisions, invariants, gaps, or touch points needed for the use case;
- evidence references for load-bearing claims;
- an evidence snapshot: the revision of each repository inspected and the
  observation time of live or external state;
- a clear distinction between verified facts, interpretations, future state,
  and mechanisms that do not exist.

## Workflow

### 1. Frame the question

Write an internal one-sentence contract:

> After viewing this page, the reader can explain or decide [specific outcome].

Reject a contract such as "understand the architecture." Name the confusion.
Examples: identify the owner of every integration seam; explain why two outputs
can disagree; decide which migration dependency must land first.

Choose the initial mode:

- **Landscape mode:** topology, ownership, communication, migration state, and
  coordination seams.
- **Mechanism mode:** data flow, transformations, provenance, invariants,
  failure modes, and missing safeguards.
- **Hybrid:** use only when both views are necessary to answer one question.
  Keep distinct visual sections rather than encoding unrelated meanings into
  one overloaded graph.

Apply this test to the one-sentence contract:

- If the answer names who or what must change together, use landscape mode.
- If the answer traces a value, field, decision, or verdict to its origin, use
  mechanism mode.
- Use hybrid only when the contract explicitly requires both a coordination
  decision and a derivation explanation. Do not default to hybrid. If neither
  test resolves the mode, ask which of those two answers the reader needs.

### 2. Gather primary evidence

Read repository instructions first. Before investigating, capture the current
revision of every repository in scope and the observation time for live or
external state. Use those revisions consistently in source references. If a
source repository changes before delivery, either re-verify against the new
revision or keep citations pinned to the captured revision and state that the
page is a snapshot.

Trace the smallest source surface that can answer the question:
implementations, schemas, tests, configuration, migration code, and generated
contracts. Use tickets, pull requests, meeting records, and design documents
for intent and ownership, but verify implementation claims against the captured
source revision.

For broad systems, delegate independent read-only searches by subsystem. Every
delegated prompt must name the scope, files or search area, expected output, and
success criteria. Keep interpretation and the final architecture model in the
main context so important reasoning is not lost in handoffs.

Maintain a compact evidence ledger while investigating:

| Claim | Evidence | Status |
| --- | --- | --- |
| What happens | `path:line`, test, schema, or canonical URL | verified |
| Why it was chosen | decision record, ticket, or attributed statement | reported |
| What remains unclear | conflicting or absent evidence | unknown |

Do not upgrade reported intent into verified runtime behavior. Re-check any
session summary or old document against current primary sources before using it.

### 3. Build the causal model before the page

Write the architecture thesis in plain language. Then identify only the elements
needed to prove it:

- actors or components;
- boundaries and ownership;
- interfaces and payloads;
- transformations and derivations;
- persistence and source-of-truth locations;
- invariants and the code or test that enforces each one;
- exceptions, bypasses, missing checks, and deliberately unsupported paths;
- current, transitional, and target states when time matters.

Every arrow must answer "what crosses this boundary, and how?" Generic arrows
such as "uses" or "calls" are a signal that the investigation is unfinished.

Before drawing, challenge the model:

- What evidence contradicts the thesis?
- Could two independently computed values diverge?
- Is a field authoritative, derived, cached, copied, or presentation-only?
- Is the apparent safeguard implemented, or merely implied by naming?
- Am I confusing intended, current, and future architecture?

### 4. Choose the visual grammar

Select visual encodings from the question, not from a preferred diagram style.
Use one primary semantic axis per visual. For example, use color for ownership
in a coordination map or for provenance in a mechanism map, but not both at
once. Encode secondary meaning with labels, line styles, grouping, or badges.

Use Mermaid when automatic graph layout helps. Use HTML and CSS directly for
small linear flows, comparisons, timelines, matrices, or report anatomy. Split
a graph when density prevents a reader from following its important path.

### 5. Outline the explanation

Create the page structure before writing HTML. A useful default is:

1. thesis and scope;
2. legend;
3. primary visual;
4. guided reading path;
5. decisions or invariants;
6. touch points, failure modes, or gaps;
7. evidence and freshness note.

Add tables, catalogs, truth tables, or secondary diagrams only when they prove a
claim the primary visual cannot. Do not reproduce production decision logic in
JavaScript to make a simulator; that creates a second source of truth. If an
interactive demonstration is essential, label it as a model and validate it
against production fixtures.

### 6. Build the HTML

Keep the result as one HTML file unless the artifact is too large or the user
asked for a maintainable site. Use semantic HTML, responsive CSS, and no build
step. State any network dependency; a page that loads Mermaid from a CDN is a
single file but is not self-contained offline. When a visual depends on a
remote library, detect load failure at runtime and show a visible message naming
what failed and which parts of the page still work.

The page must remain usable on desktop and mobile. Do not rely on color alone.
Pair colors with text labels, line styles, or shapes. Provide pan and zoom when
a diagram is larger than its viewport, and keep the explanatory prose readable
without manipulating the graph.

Favor a visual language suited to the subject. Do not mechanically reproduce
the dark cards, colors, typography, or section order of prior artifacts.

### 7. Verify claims and behavior

Run two separate passes.

**Evidence pass:**

- Re-open every source behind a load-bearing claim.
- Verify names, counts, enum values, states, and source references.
- Confirm each claimed invariant has an enforcing mechanism.
- Mark reported intent and unresolved contradictions explicitly.
- Search for placeholders, accidental secrets, local-only URLs, and stale status.

**Reader pass:**

- Determine which verification the environment supports before claiming a
  result.
- If browser automation or a headless browser is available, load the file,
  inspect console errors, and check desktop and narrow/mobile widths, the
  initial viewport, controls, overflow, and long labels.
- If no browser inspection is available, run only defensible static checks:
  parse or validate the HTML with an available tool, syntax-check inline
  JavaScript with an available runtime, search for unresolved placeholders and
  local absolute URLs, and verify that remote dependencies have visible failure
  handling. No static check substitutes for rendering; report browser
  verification as unperformed.
- Follow the visual as a skeptical reader who disagrees with the thesis.
- Confirm the one-sentence answer is understandable without reading every detail.

Allow at most three visual refinement passes. If the same readability problem
survives twice, change the representation rather than adjusting spacing again.

### 8. Deliver

Open the finished page when the environment supports it. Report:

- the artifact path;
- the question it answers;
- the selected mode and primary visual encoding;
- the evidence and browser checks performed;
- any external dependency or unverified claim;
- what will become stale and when it was observed.

Do not end with a generic offer to improve the page. Ask for a decision only if
the artifact exposed a real unresolved choice.

## Checks

The visualization is complete only when all are true:

- The page answers a specific architecture question rather than cataloging the
  repository.
- A reader can identify the important path and understand every arrow on it.
- The visual encoding has a documented meaning and does not overload color.
- Current, future, removed, and nonexistent behavior cannot be mistaken for one
  another.
- Every load-bearing claim has current evidence or an explicit uncertainty
  label.
- The prose explains why the topology matters and names the resulting decision,
  risk, or coordination point.
- The HTML renders without known errors at desktop and mobile widths when
  browser inspection is available. Otherwise, the delivery report names browser
  verification as unperformed and lists the static checks that passed.
- Every control has a visible focus style and a text-accessible name. Any
  interaction that reveals information is operable without a pointer, and the
  core explanation remains available as prose if the visual does not render.
- Volatile status is timestamped and visually separate from durable architecture.

## Stop conditions

Stop and ask one focused question when the reader or required decision remains
ambiguous after repository inspection. Stop before publishing or sharing when
the page contains secrets, customer data, private local paths, or internal
context unsuitable for its audience.

When sources conflict, do not pick the most convenient account. Show the
conflict, identify which source is authoritative if known, and state what would
resolve it. When the requested scope cannot fit in one readable causal model,
propose a bounded set of linked views before building them.

## Recovery

- If the graph is unreadable, remove irrelevant nodes, split durable and
  transitional views, or replace the graph with a sequence or matrix.
- If Mermaid repeatedly produces poor layout, switch to direct HTML/CSS or SVG;
  do not spend more than two retries fighting the layout engine.
- If source evidence is incomplete, trace backward from the output and forward
  from the input. If both fail, mark the gap rather than inventing an edge.
- If browser inspection is unavailable, follow the static branch in the reader
  pass and report rendering as unverified. Never infer visual success from a
  markup or syntax check.
- If three refinement passes fail the completion checks, stop with the current
  artifact, the failed checks, and a concrete alternative representation.
