# HTML Report Format

The report is a single HTML file in the OS temp directory; the candidate index is a separate JSON file. Tailwind and Mermaid come from CDNs, so the report needs network access when it opens. Mermaid handles graph-shaped diagrams reliably; hand-built divs and inline SVG handle the editorial visuals such as mass diagrams and cross-sections. Mix the two. Do not lean on Mermaid for everything, or the report starts to look generic.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review for {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
    <style>
      /* small custom layer for things Tailwind doesn't cover cleanly:
         dashed seam lines, hand-drawn-feeling arrow heads, etc. */
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
      .deep { background: linear-gradient(135deg, #0f172a, #1e293b); }
    </style>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="coverage">...</section>
      <section id="state-machines" class="space-y-10">...</section>
      <section id="invariants">...</section>
      <section id="candidates" class="space-y-10">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## Header

Repo name, date, and a compact legend: solid box = module, dashed line = seam, red arrow = leakage, thick dark box = deep module, rounded box = state, dashed state edge = unknown transition. No introduction paragraph. The coverage section comes next. Why: the legend removes the need for a paragraph of explanation.

## Coverage section

A small table with the area, the lifecycles audited, the invariants audited, what was not audited, and why. Put it first. This is the completeness claim, and it tells the owner how far the report can be trusted.

## State machine section

One card per lifecycle entity:

- **Title**: the entity and the machine, for example "Order lifecycle".
- **Badge row**: recommendation strength (`Strong` = emerald, `Worth exploring` = amber, `Speculative` = slate), plus `implicit` or `partly explicit`.
- **States and transitions**: a compact table with state, event, guard, next state, side effects, and evidence. Cut it to the transitions that carry a decision.
- **Evidence**: the `file:line` locations behind the states and guards.
- **Before / after diagram**: a Mermaid `stateDiagram-v2` of what the code supports now, next to the explicit machine with illegal edges removed. Use dashed edges for unknown or unenforced transitions. This is the centrepiece.
- **Problem**: one sentence. The illegal state or transition the code allows.
- **Recommended form**: typed states plus a pure transition function, a transition table, a persisted status plus a check constraint, or a statechart library. Name the illegal states it removes, the illegal transitions it rejects, and the test that proves it.
- **Interface change**: one line. What the caller stops knowing when the state machine becomes explicit.
- **Score**: confidence, effort, and risk, for example `HIGH / M / LOW`.
- **Wins**: bullets, six words or fewer each. For example "Illegal states become unrepresentable", "One transition table, one test".

## Invariant section

A table with one row per invariant, using the [model audit's](MODEL-AUDIT.md) invariant columns (invariant, where assumed, current enforcement, failure mode, recommended enforcement, interface change, evidence) plus a score column. Use a small badge for the current enforcement layer (`type`, `schema`, `database constraint`, `transaction`, `runtime guard`, `test`, `documentation`, `none`). Put `none` in red. Add a `score` cell with strength, confidence, effort, and risk, for example `strong / HIGH / M / LOW`. The table is the centrepiece. Do not turn a one-line invariant into a card.

If the table has more than eight rows, group the rows by family: uniqueness, cardinality, ordering, ownership and tenancy, balance and totals, temporal, referential, idempotency, concurrency, immutability. Keep the families that have findings. Do not add an empty family. Why: an empty family is a heading with no evidence.

## Deepening candidate card

Each candidate is one `<article>`:

- **Title**: short, names the deepening, for example "Collapse the Order intake pipeline".
- **Badge row**: recommendation strength, plus a tag for the dependency category (`in-process`, `local-substitutable`, `ports & adapters`, `mock`).
- **Evidence**: `file:line` list, `font-mono text-sm`.
- **Before / after diagram**: the centrepiece. Two columns, side by side. See the patterns below.
- **Problem**: one sentence. What hurts.
- **Solution**: one sentence. What changes.
- **Interface change**: one line. What the caller stops knowing.
- **Deletion test**: one line. What happens to the complexity when the module is deleted.
- **Score**: confidence, effort, and risk, for example `HIGH / M / LOW`.
- **Wins**: bullets, six words or fewer each. For example "Tests hit one interface", "Pricing stops leaking", "Delete four shallow wrappers".
- **ADR callout** (if applicable): one line in an amber-tinted box.

No paragraphs of explanation. If the diagram needs a paragraph to be understood, redraw the diagram.

## Top recommendation section

One larger card. Candidate name, one sentence on why, and the sequence after it: what to do now, what comes next, and what waits. Anchor link to its card. Every card carries an `id` from the candidate index (`state-01`, `invariant-01`, `deepening-01`), so the anchor link resolves. Below the sequence, the before/after interface sketch: entry points, invariants, ordering, error modes, and what sits behind the seam. Make one call. The top recommendation is a verdict with the evidence behind it, not a shortlist.

## Diagram patterns

Pick the pattern that fits the candidate. Mix them. Do not make every diagram look the same. Variety is part of the point.

### Mermaid graph

Use a Mermaid `flowchart` or `graph` when the point is "X calls Y calls Z, and look at the mess." Wrap it in a Tailwind-styled card so it does not feel parachuted in. Style with classDef to colour leakage edges red and the deep module dark. Sequence diagrams work well for "before: six round-trips; after: one."

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
  <pre class="mermaid">
    flowchart LR
      A[OrderHandler] --> B[OrderValidator]
      B --> C[OrderRepo]
      C -.->|"leak"| D[PricingClient]
      classDef leak stroke:#dc2626,stroke-width:2px;
      class C,D leak
  </pre>
</div>
```

Always write edge labels in the pipe form (`-->|"..."|`, `-.->|"..."|`), since an inline label ends at its first period.

### Mermaid state diagram

Use `stateDiagram-v2` for a lifecycle. Before: the states and transitions the code supports, including the illegal ones. After: the explicit machine, with illegal transitions removed and unknown edges still dashed.

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
  <pre class="mermaid">
    stateDiagram-v2
      [*] --> Draft
      Draft --> Pending: submit
      Pending --> Paid: payment
      Pending --> Cancelled: cancel
      Paid --> Shipped: ship
      Shipped --> [*]
      Draft --> Paid: payment
  </pre>
</div>
```

The last edge is the finding: payment from `Draft` is representable today and should not be.

### Hand-built boxes and arrows

Modules as `<div>`s with borders and labels. Arrows as inline SVG `<line>` or `<path>` elements positioned absolutely over a relative container. Reach for this when you want the after diagram to feel like one thick-bordered deep module with greyed-out internals, since Mermaid will not render that with the right weight.

### Cross-section

Stack horizontal bands (`h-12 border-l-4`) to show layers a call passes through. Before: six thin layers each doing nothing. After: one thick band labelled with the consolidated responsibility.

### Mass diagram

Two rectangles per module: one for interface surface area, one for implementation. Before: the interface rectangle is nearly as tall as the implementation rectangle (shallow). After: the interface rectangle is short and the implementation rectangle is tall (deep).

### Call-graph collapse

Before: a tree of function calls rendered as nested boxes. After: the same tree collapsed into one box, with the now-internal calls shown faded inside it.

## Style guidance

- Use a single column with `py-12` between sections and `space-y-10` between cards. Use `font-serif` for headings and `font-sans` for body text. No sidebar, no dashboard chrome.
- Use emerald as the single accent, red for leakage and `none`, and amber for warnings and the Worth exploring badge. No other hues. Why: the Strong badge already uses emerald, and a second accent dilutes it.
- Keep diagrams 320px tall so before and after sit side by side without scrolling.
- Use `text-xs uppercase tracking-wider` for module labels inside diagrams, so they read as schematic, not as UI.
- The only scripts are the Tailwind CDN and the Mermaid ESM import. The report is otherwise static: no app code, no interactivity beyond Mermaid's own rendering.

## Tone

Plain English, concise, and the architectural nouns and verbs come straight from the `codebase-design` skill. Concision is not an excuse to drift. Why: the vocabulary is the report's precision.

**Use exactly:** module, interface, implementation, depth, deep, shallow, seam, adapter, leverage, locality.

**Model terms:** state, state machine, lifecycle, transition, invariant. Use these exactly in the model sections. Why: the model terms name the evidence the report is built on.

**Never substitute:** component, service, unit (for module); API, signature (for interface); boundary (for seam); layer, wrapper (for module, when you mean module).

For every sentence, name the audited module and the gain in glossary terms. If the sentence could describe another project, cut it. If the claim is a preference, cut it or attach evidence. Why: a sentence that survives a find-and-replace on the repo name is generic advice.

**Wins bullets** name the gain in glossary terms: "locality: bugs concentrate in one module", "leverage: one interface, N call sites", "interface shrinks; implementation absorbs the wrappers". Do not write "easier to maintain" or "cleaner code", because those terms are not in the glossary and do not earn their place.

No hedging, no throat-clearing, no "it's worth noting that". If a sentence could be a bullet, make it a bullet. If a bullet could be cut, cut it. If a term is not in the `codebase-design` glossary or the model terms above, reach for one that is before inventing a new one.
