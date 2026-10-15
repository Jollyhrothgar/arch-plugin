# Architecture visualization design patterns

Use this reference after framing the driving question. It helps select a page
structure and visual language; it is not a template that every artifact must
follow.

## Core test

An architecture visualization earns its complexity when it makes a relationship
or distinction easier to reason about than prose alone. It fails when it merely
places known nouns into boxes.

A successful page lets the intended reader answer three questions:

1. What is the important path through this system?
2. Which boundaries, transformations, or decisions make that path behave as it
   does?
3. What should I conclude or do because of that structure?

## Landscape mode

Use landscape mode for coordination across components, teams, tickets, or a
migration. The architecture is socio-technical: ownership and sequencing can be
as important as runtime topology.

### Good visual encodings

| Concern | Encoding |
| --- | --- |
| Ownership | One color per owner or team, repeated in the legend and node label |
| Existing versus future | Solid versus dashed border or separate current/target views |
| Communication | Directed edge labeled with protocol, payload, or operation |
| Migration | Old and new paths shown together, with the retired path visibly marked |
| Coordination | Numbered touch points tied to specific edges or boundaries |
| Decisions | Numbered decisions in a narrative below the visual |
| Volatile progress | Separate timestamped panel, never blended into durable topology |

### Useful page composition

1. One-sentence orientation: what the system does and why this slice matters.
2. Legend defining ownership and lifecycle encodings.
3. Current or current-to-target topology.
4. Guided path through the diagram.
5. Decisions that explain why the topology has this shape.
6. Cross-team touch points and what must be agreed at each one.
7. Optional progress panel with an explicit observation timestamp.

### Common failure

A row of components connected by unlabeled arrows looks orderly but does not
explain the system. Replace "A -> B" with the actual interface and payload. If
the relationship cannot be named, return to investigation.

## Mechanism mode

Use mechanism mode for a narrow technical question about derivation,
consistency, provenance, safety, or failure.

### Good visual encodings

| Concern | Encoding |
| --- | --- |
| Provenance | Color by model-authored, deterministic, configured, persisted, or presentation-only |
| Trust boundary | Strong boundary line and an explicit checked/unchecked crossing label |
| Transformation | Edge label naming the function, mapping, schema, or derivation |
| Invariant | Nearby statement naming both the rule and its enforcement mechanism |
| Divergence | Branch the values at the exact point where independent computation begins |
| Missing safeguard | Visible gap or callout saying what does not exist |
| Exhaustive behavior | Truth table or decision matrix sourced from production logic |

### Useful page composition

1. Direct answer to the mechanism question.
2. Provenance or trust legend.
3. End-to-end data-flow diagram.
4. Anatomy of the important output or record.
5. Decision table, rule catalog, or field mapping when needed for proof.
6. Ranked disagreement modes, unchecked crossings, or gaps.
7. Explicit "what does not exist" section to prevent imagined safeguards.
8. Evidence index with source revision.

### Common failure

Showing two outputs originate from one broad component does not prove they
cannot diverge. Trace both values to their common input, then identify every
independent field, transformation, default, and fallback after that point.

## Hybrid mode

Use a hybrid only when the reader must understand both the implementation and
the coordination consequence. Keep the modes separate:

- one visual for runtime or data mechanics;
- one small map or table for ownership and sequencing;
- one narrative joining them through the decision the reader must make.

Do not color the same graph by both ownership and provenance. If both meanings
matter, use two diagrams or put one meaning in explicit text labels.

## Evidence language

Use these categories consistently:

- **Verified:** directly observed in current code, tests, schema, configuration,
  or tool output.
- **Reported:** stated in a ticket, pull request, meeting record, or prior
  session, but not independently confirmed as current behavior.
- **Interpretation:** a conclusion drawn from multiple verified facts. State the
  reasoning, not just the conclusion.
- **Unknown:** evidence is absent or conflicts.
- **Nonexistent:** source inspection establishes that the expected mechanism is
  not implemented. Name the search boundary used to support this stronger claim.

Avoid decorative citations. Put a source reference next to the claim it proves.
For code, prefer `path:line` plus a git revision or as-of date. For decisions,
use a canonical URL and attribute the statement.

## HTML implementation

### Dependency policy

Prefer plain HTML, CSS, and JavaScript with no build step. A single HTML file is
easy to open and share, but it is self-contained only when every dependency is
embedded. If Mermaid or another library comes from a CDN, state that the page
requires network access. Detect a failed remote load and replace the broken
visual with a visible explanation of what failed and which content remains
usable.

### Responsive behavior

- Keep prose in a readable column even when the diagram is wide.
- Put wide diagrams in a bounded viewport rather than widening the whole page.
- Add mouse, trackpad, and touch-compatible pan/zoom for dense graphs.
- Provide visible Fit and Reset controls; do not make gestures the only option.
- At narrow widths, stack cards and allow tables to scroll horizontally.
- Respect `prefers-reduced-motion` if transitions or animation are present.

### Accessibility

- Use semantic headings and tables.
- Maintain sufficient contrast in light and dark themes.
- Repeat color meaning in text, shape, border style, or labels.
- Give controls accessible names and keyboard focus styles.
- Make interactions that reveal information operable without a pointer.
- Keep the core explanation available as prose if the graph fails to render.

### Interaction restraint

Interaction should improve navigation or evidence access. Useful examples are
pan/zoom, filtering a large verified catalog, collapsing secondary evidence, or
linking a callout to its source. Avoid interaction that simulates production
logic unless simulation itself is the user's question and the model is tested
against production fixtures.

## Verification heuristics

### Evidence pass

- Search the rendered text for every number, enum, ticket, state, and negation.
- Re-read the source around each referenced line; matching text alone can miss
  control flow or scope.
- For "always," "never," and "cannot diverge," identify the enforcement
  mechanism and test the counterexample mentally.
- For "does not exist," document the directories, symbols, or interfaces
  searched.
- Treat old session summaries as leads until current source confirms them.

### Reader pass

- Hide the prose and ask whether the visual's important path is identifiable.
- Hide the visual and ask whether the prose still explains the conclusion.
- Read only the title, thesis, legend, and callouts. The conclusion should still
  be correct.
- Check whether a reasonable dissenting engineer could tell evidence from
  interpretation.
- Check whether stale status could be mistaken for durable architecture.

## Lessons from the two originating cases

The coordination case began as a lane-based infrastructure diagram. It became
useful only after every edge named the communication mechanism and the page
added the decisions and cross-team touch points that explained why the graph had
that shape. Its progress panel was useful temporarily and then became stale.

The mechanism case used provenance as its visual spine. It traced model-authored
fields into deterministic report rendering, added a truth table and catalog for
proof, and named absent consistency checks. It rejected a JavaScript simulator
because duplicating production decision logic would create another source of
truth.

Together they establish the governing rule: choose the visual semantics from
the unresolved question, then surround the visual with enough evidence and
narrative for the reader to act on the answer.
