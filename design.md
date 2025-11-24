# Design Principles

## What Design Is NOT

Design is NOT merely creating deliverables or following methodologies:

- Defining interfaces
- Organizing folder structures
- Drawing class diagrams
- Showing dependencies
- Test-Driven Development
- Filling in EARS notation
- Writing in Gherkin syntax

## What Design Actually IS

Design is the **intellectual activity of thinking through problems and making deliberate decisions about structure**.

### Understanding the Problem

- Identify the real problem to solve
- Clarify constraints and requirements
- Anticipate future changes

### Finding Appropriate Abstractions

- Model complex reality into concepts
- Determine responsibility boundaries
- Decide what to hide and expose

### Creating Change-Resilient Structures

- Identify stability vs. volatility
- Localize change impact
- Prepare for new requirements

### Making Intent Explicit

- Document reasoning behind decisions
- Explain assumptions
- Communicate why this structure over alternatives

## Design Principles

- Design is the intellectual process of understanding problems and making decisions. Artifacts are outcomes of this thinking, not substitutes for it.
- Business meaning should be visible in the structure itself. Hidden logic obscures intent and scatters knowledge across the codebase.
- Systems must accommodate future evolution. Stability comes from identifying what changes and what remains constant, not from resisting change.
- Separation is valuable only when concerns truly differ in purpose or volatility. Unnecessary separation creates complexity without benefit.
- Components should clearly state their expectations and guarantees. Implicit contracts create fragility and prevent reuse.
- Type systems can express constraints and intent at compile-time. This shifts error detection earlier and makes assumptions visible.

Design is explaining WHY, not documenting WHAT. If you skip reasoning and alternatives, you're not designing.
