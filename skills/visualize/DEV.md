# arch:visualize - development state

Working notes for whoever picks this up next. Not part of the skill; the skill
does not read this file.

## Where it is

First draft of a question-driven architecture visualization skill. It supports
two modes:

- landscape mode for topology, ownership, communication, migration, and
  coordination seams;
- mechanism mode for transformations, provenance, invariants, failure modes,
  and missing safeguards.

The skill produces a single HTML technical brief, selects its visual grammar
from the user's question, verifies load-bearing claims against primary sources,
and requires a separate browser-based reader pass. Detailed mode selection and
HTML guidance live in `references/design-patterns.md`.

The draft is based on two artifacts from earlier sessions that worked: a
dependency-graph explanation, written after a simpler lane diagram failed to
explain relationships and decisions, and a provenance-centered explanation of
how a report is generated and whether its outputs can disagree.

Both source artifacts are gitignored working files, not fixtures in this repo.
The reusable lessons are folded into the skill rather than depending on those
paths at runtime.

## Where it should be

The skill should reliably turn a real architectural confusion into an artifact
that Mike can use for technical reasoning or a cross-team conversation without
correcting the agent's causal model first.

Before treating it as mature:

1. Run it on one landscape question outside the Vetting Dashboard domain.
2. Run it on one mechanism question outside vendor risk.
3. Compare both outputs against the completion checks and record which
   instructions were ignored or ambiguous.
4. Add reusable HTML code only when repeated use proves a stable pattern. Do not
   freeze the two originating pages into a universal template.
5. Re-evaluate whether the skill belongs in the shared registry once its visual
   language and verification workflow generalize beyond Mike's use.

## Known limits

None established yet. The draft has not completed an end-to-end run on a new
architecture question.

## Open questions

- Will agents select landscape versus mechanism mode correctly without an
  explicit user choice, now that the draft has a discriminating rule?
- Is browser verification portable across Claude Code and OpenCode when browser
  automation tools differ or are unavailable?
- Does the one-file default remain practical for large catalogs, or should a
  size threshold permit a data sidecar?
- Should a later version bundle a tested pan/zoom shell, or would that encourage
  visual sameness and template-first reasoning?
- Does the skill's broad trigger overlap too much with `write-doc` or general
  frontend work after installation?
- Is the evidence ledger useful enough to persist beside the HTML, or should it
  remain temporary investigation state?

## Log

**2026-08-31** - Initial draft created from the two prior architecture HTML
sessions. The design deliberately keeps visual choice adaptive, separates
volatile status from durable architecture, and prohibits unvalidated replicas
of production decision logic.

**2026-08-31** - OpenCode `github-copilot/claude-opus-5` agent
`4d66277b-23eb-4670-bce2-802f59b41c21` completed a read-only adversarial audit
and rated the draft "needs targeted fixes," not structural rework. Applied the
agreed must-haves: a landscape/mechanism discriminator and hybrid gate,
capability-aware browser verification, captured source revisions, mechanism
routing phrases, remote-library failure handling, and minimum accessible
interaction. Deferred persistent evidence state, check-list restructuring,
iteration-budget wording, reusable HTML scaffolding, and other polish until two
new-domain runs produce evidence that they are needed.
