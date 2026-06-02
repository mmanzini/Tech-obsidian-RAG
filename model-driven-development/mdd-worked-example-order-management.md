# MDD — Worked Example: order-management (CIM → PIM → PSM → code)

**Source:** [Model-Driven Development Standard — Examples/order-management](../../../Resources/documents/frameworks/Model-driven-development/Examples/order-management/)

---

## Summary

One small system — a minimal order-management service — modelled top to bottom and generated to a Java/Spring + PostgreSQL platform, to show the same meaning descending through every MDD layer. The instructive moment is the gap between the PIM and the PSM: *what stays the same is the meaning; what gets added is the platform.* That gap is the whole value proposition (source: Examples/Examples.md). Concrete companion to the [[mdd-authoring-workflow|authoring workflow]] and [[mdd-overview|overview]].

## CIM — the domain in business language

No software concepts; audience is the operations team. Entities: Customer, Order, Line Item, Product, Payment, Shipment. Domain rules captured in plain language: an order has at least one line item; **an order cannot be shipped until paid in full**; the amount owed is the sum of price × quantity across line items; quantity ≥ 1; a shipped order cannot change. Open questions (cancellation, partial payments) are listed, not silently assumed (source: Examples/order-management/cim.md).

## PIM — precise, still platform-free

The same system as computational structure with **no technology named**. `Order` gets a `status: OrderStatus`, a derived `total: Money = sum(price × quantity)`, and operations with pre/postconditions. The lifecycle becomes an explicit state machine `Draft → Placed → Paid → Shipped` (Shipped terminal), and the domain rules become invariants — e.g. `Order: ship() requires status = Paid` (the central rule) and `quantity >= 1`. Types are abstract (`Money`, `Quantity`, `Identifier`). Everything platform — persistence, API, `Money` realisation, framework — is explicitly *deferred to the PSM* (source: Examples/order-management/pim.md).

## PSM — the same model, committed to a platform

Target: Java 21, PostgreSQL via JPA/Hibernate, REST/JSON, Spring Boot. The PSM only *adds* decisions on top of the unchanged PIM meaning (source: Examples/order-management/psm.md):

- `Order` → `@Entity @Table(name="orders")`; `status` → `VARCHAR(16)` enum column; derived `total` → `@Transient` getter (not stored).
- `price: Money` → **two** columns `price_amount NUMERIC(12,2)` + `price_currency CHAR(3)` — the abstract type realised.
- Composition `Order 1—1..* LineItem` → `@OneToMany(cascade=ALL, orphanRemoval=true)`.
- Operations → REST endpoints (`POST /orders/{id}/payment`, `/shipment`, …); state transitions → guarded service methods throwing `409 Conflict` on a bad source state.
- Each invariant gets an enforcement home: `quantity >= 1` → `@Min(1)` + DB `CHECK`; ship-requires-Paid → service guard; shipped-immutable → mutating endpoints reject when `SHIPPED`.

## Transformations — the rules that produce PSM and code

Two chained transformations: **PIM→PSM** (M2M, ATL-style declarative rules) then **PSM→code** (M2T, Acceleo/MTL templates) (source: Examples/order-management/transformation.md). Representative rules: `Entity → @Entity + @Table(pluralise(snake_case(name)))`; `attribute: Money → amount + currency columns`; `composition → @OneToMany cascade`; `state machine → <Name>Status enum + guarded methods`; `invariant x>=n → @Min(n) + DB CHECK`. The rules ship with input→output **test fixtures** re-run on every change (e.g. "re-running generation does not overwrite an existing `OrderService.java`").

## Generated code + the generation gap

The M2T step emits `Order.java`, `OrderStatus.java`, `OrderServiceBase.java` (regenerated every build), `<Name>Repository`, controllers, and `V1__schema.sql` — all marked `GENERATED — do not edit` (source: Examples/order-management/generated-code.md). The single human-owned file is `OrderService extends OrderServiceBase`, the **generation-gap** point where behaviour the model can't express attaches — here, calling an external `PaymentGateway` in an `onBeforeMarkPaid()` hook the base class leaves as a no-op. The generator never overwrites it.

## What the example is built to teach

- **Traceability:** every generated artifact traces to a model line — the `quantity >= 1` CHECK, the ship-requires-PAID guard, the `Money → amount+currency` split each came from a specific PIM invariant via a named rule (source: Examples/order-management/generated-code.md).
- **Change the model, regenerate:** adding a `Cancelled` state means editing the PIM state machine and re-running — not hand-patching output.
- **Tiny, explicit hand-written boundary:** only `OrderService.java`, exactly where the model legitimately couldn't express an external payment call.
- **Re-target by swapping the transformation:** point the same PIM at a different PIM→PSM transformation to generate, say, a Python/FastAPI/SQLite stack from the unchanged model — portability in action.

## Key Takeaways

- The PIM→PSM gap is the lesson: meaning is identical across the two; the PSM only adds platform decisions
- Abstract types (`Money`) realise into platform constructs (amount + currency) via a consistent transformation rule, not per-entity guesses
- Every PIM invariant must get a concrete enforcement home in the PSM, or it silently won't hold
- Generated files are owned wholesale by the generator; the generation-gap subclass is the one explicit, human-owned seam
- Traceability + regenerate-don't-patch + swap-the-transformation-to-re-target are the properties that make the model (not the code) the source of truth

## Related

- [[mdd-authoring-workflow|MDD — The Authoring Workflow]] — the guides this example instantiates
- [[mdd-overview|MDD — Principles, Abstraction Layers, and Workflow]] — the abstraction-layer theory the example descends through
- [[mdd-classic-vs-modern|MDD — Classic MDA vs Modern Practice]] — where this UML/MDA-style pipeline sits on the spectrum
