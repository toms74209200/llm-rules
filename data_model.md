# Data Model Design

## Core Principle

**Events must not be updated. Do not modify past facts.**

When designing databases, classify all entities into two categories:

### Event
Records of "things that happened" in business operations. An order was placed, a shipment occurred, an approval was made.
These are facts from the past and cannot be rewritten later.
INSERT only. UPDATE/DELETE prohibited.

### Resource
"Things" whose state changes over time. Employees, products, customers.
Employees get promoted, product prices change, customer addresses change when they move.
UPDATE allowed if managing only current state. Use temporal management if history is required.

## Concept Separation: Simple vs Complex

### What is Complexity?
From Rich Hickey's "Simple Made Easy":
- Simple: State having a single responsibility, a single concept
- Complex: State where multiple responsibilities or concepts are intertwined and mixed

Complex things are difficult to understand and difficult to combine.
The goal of data modeling is to decompose complex entities into simple entities.

### Detecting Concept Mixing

Consider an "Order entity" with the following attributes:
- Order number, customer code, product code (order concept)
- Shipment number, delivery company code, shipment date (shipment concept)

This has two concepts—"order" and "shipment"—mixed in one entity.

**What's the problem?**
- "Want to ship multiple orders together" → Create multiple order records with same shipment number?
- "Want to ship one order in multiple deliveries" → The above method cannot express this

**Why does it lose flexibility?**
Because it implicitly fixes the relationship between order and shipment to "1-to-1".
When concepts are mixed, you get a rigid model that cannot adapt to business requirement changes.

### Decomposing Concepts

**First Stage: Separate concepts**
Make "order" and "shipment" separate entities.
If the shipment entity holds order numbers, multiple orders can be grouped into one shipment.

**Second Stage: Make relationships independent**
But the above still cannot handle "shipping one order in multiple deliveries".
Extract the relationship itself between order and shipment as an independent entity (junction table).

**Result:**
- Order entity (order concept only)
- Shipment entity (shipment concept only)
- Order-Shipment mapping entity (relationship concept only)

Each has a single concept. The number of entities increases, but each is simple and easy to understand,
and can flexibly respond to requirement changes.

### Essential Complexity vs Accidental Complexity

From the "Out of the tar pit" paper:
- Accidental complexity: Arises from design mistakes. Concept mixing is this. Must be eliminated.
- Essential complexity: Complexity inherent to the business itself. Increased number of entities reflects this.

Forcing reduction of entity count causes concept mixing and creates accidental complexity.
The goal should be eliminating accidental complexity and focusing on essential complexity.

## The Anti-Pattern of Deletion Flags

### The Fundamental Problem with Deletion Flags

Deletion flags (is_deleted) make data state ambiguous.

**Example: Employee Management System**
Cannot physically delete retired employee data (tied to past approval records).
But don't want to display them in employee lists.
→ Set is_deleted = true?

**What's wrong with this?**

- Unclear state: The contradictory state of "exists but doesn't exist"
- Business rules are hidden: The meaning of the flag scatters across code, not expressed at DB level
- Query complexity: Every query needs `WHERE is_deleted = false`
- Risk of misuse: Bugs frequently occur from mistakenly referencing/updating deleted data

**A more serious problem: What is "deletion"?**
Is a retired employee "deleted"? No, there's a clear business state of "retired".
The deletion flag hides this essential business concept behind implementation convenience.

### Explicit State with Subtypes

**Paradigm shift:**
Not "hiding state with deletion flags" but "expressing state as types with subtypes".

The employee entity actually contains two different business concepts:
- Active employee: Has department, can be approval authority, subject to payroll
- Retired employee: Has retirement date, reference-only for past data, cannot be approval authority

These aren't just "different states" but different types with fundamentally different business rules applied.

**Expressing with subtypes:**
- Supertype "Employee" has common attributes (employee ID, name)
- Subtype "Active Employee" has specific attributes (department ID, position)
- Subtype "Retired Employee" has specific attributes (retirement date, retirement reason)

**What improves?**

1. Type safety: Retired employees cannot sneak into logic that only handles active employees
2. Business rules made explicit: "Approval authority selected from active employee table" can be expressed with DB constraints
3. Intent clarification: Not "deletion" but the business concept of "retirement" is correctly modeled
4. Query simplification: Just select the appropriate table for the use case

Deletion flags are technical "hiding", subtypes are business "revelation".
Data models should accurately reflect business, not be distorted by technical convenience.

## Aggregate State: Derived from Events

The current state of resources (aggregates) is derived from events.
There are multiple implementation methods; choose according to requirements.

### Rebuild from Events Each Time
Read all events and apply them sequentially to get current state.
- Advantages: Simple implementation, always up-to-date
- Disadvantages: Slow when many events
- Use cases: Aggregates with few events, when strict consistency is needed

### Snapshots
Periodically save aggregate state and only apply events after that.
- Advantages: Fast rebuilding
- Disadvantages: Complexity of snapshot management
- Use cases: Aggregates with many events, when performance is critical

### Materialized Views / Projections
Read-only tables updated asynchronously when events occur.
- Advantages: Very fast reads, can create multiple purpose-specific views
- Disadvantages: Eventual consistency (slight delay), synchronization mechanism needed
- Use cases: High read frequency, complex search conditions needed

### Temporal Management Tables
Tables that record state changes as chronological generations.
- Advantages: Can directly retrieve state at any past point in time
- Disadvantages: Slightly complex queries
- Use cases: When frequently referencing specific past states like price history

**Important principle:**
Aggregate state is a derivative from the "truth of events".
Regardless of implementation choice, state can be reproduced if events are correctly recorded.
Conversely, if event recording is insufficient, the past cannot be recovered with any implementation.

## Design Checklist

Ask yourself the following before completing design:

### Entity Classification
   - [ ] Have all entities been classified as Event/Resource?
   - [ ] Is UPDATE/DELETE prohibited for events?
   - [ ] Are all business-critical occurrences recorded as events without omission?

### Concept Purity
   - [ ] Does each entity express only a single concept?
   - [ ] Are multiple concepts mixed?
   - [ ] Are many-to-many relationships explicitly expressed with junction tables?

### State Explicitness
   - [ ] Are deletion flags avoided?
   - [ ] Are states with different business treatments expressed as subtypes?
   - [ ] Does the data "type" accurately reflect business rules?

### Aggregate State Management
   - [ ] Can aggregate state be derived from events?
   - [ ] Is an appropriate implementation method (rebuild/snapshot/view, etc.) chosen according to requirements?
   - [ ] If past state is needed, can it be reproduced from events?
