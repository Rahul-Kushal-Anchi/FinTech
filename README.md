FinTech City
Capstone page 1: a one-page map of how a financial product moves money, makes decisions, controls risk, and preserves financial truth.
FinTech City has five operating districts—payments, lending, fraud and identity, compliance, and data/AI—connected to a ledger at the centre. The districts are not isolated features. Together they form a decision system in which software, data, policy, action, and human accountability affect real customer outcomes.
Scope: The named products are real-world reference examples. They show what each district does; the capstone does not need to integrate every vendor.
City map
The map follows one purchase from left to right. The main road is the transaction path; the lower loop records the result and improves the next decision.
flowchart LR
    U([Customer starts purchase])

    subgraph CHECK["1 — Check permission and trust"]
        direction LR
        C["COMPLIANCE<br/>Can this activity proceed?<br/><b>ComplyAdvantage Mesh</b>"]
        F["FRAUD + IDENTITY<br/>Is the customer and activity genuine?<br/><b>Sift</b>"]
        C -->|cleared| F
    end

    Q{"2 — How will the<br/>customer pay?"}
    L["LENDING<br/>Offer and price credit<br/><b>Affirm Adaptive Checkout</b>"]
    P["3 — PAYMENTS<br/>Authorize and move value<br/><b>Stripe Payments</b>"]
    M([Merchant receives payment])

    G[("4 — LEDGER<br/>Record what is financially true<br/><b>Modern Treasury Ledgers</b>")]
    D["5 — DATA / AI<br/>Turn outcomes into better signals<br/><b>Plaid Enrich</b>"]

    U --> C
    F -->|approved| Q
    Q -->|pay now| P
    Q -->|pay later| L
    L -->|credit approved + terms| P
    P -->|confirmation / settlement| M

    P -->|payment entries| G
    L -->|loan entries| G
    G -.->|reconciled outcomes| D
    D -.->|features and alerts for the next decision| C

    C -->|stop or review| R([Manual review / decline])
    F -->|challenge, stop or review| R
    L -->|credit declined| X([Return to pay-now option])
    X --> Q
How to read the map
Check: Compliance asks whether the activity is permitted. Fraud and identity then ask whether it appears genuine. Either district can stop, challenge, or route the case to human review.
Choose: An approved customer either pays now or requests credit. Only the pay-later branch enters the lending district.
Move: Payments authorizes the selected payment method and handles confirmation or settlement with the merchant.
Record: Payments and lending post their financial effects to the ledger. The ledger records truth; it does not make the original approval decision or move money by itself.
Learn: Data/AI uses reconciled outcomes to create better features and alerts for the next transaction. The dotted line returns to the first gate to keep the map readable; in the implementation, those signals can support compliance, fraud, and lending. This is a feedback loop, not a second money path.
Legend: Solid arrows show the current transaction. Dotted arrows show the learning loop. Side exits show a decline, challenge, or manual-review path.

Districts and capstone mapping
District
Financial job
Capstone component
Real example
Payments
Move value
Payment service
Stripe Payments
Lending
Promise value
Credit service
Affirm Adaptive Checkout
Fraud + identity
Protect trust
Risk engine
Sift
Compliance
Enforce obligations
Review workflow
ComplyAdvantage Mesh
Data / AI
Improve decisions
Feature pipeline
Plaid Enrich
Ledger
Preserve truth
Double-entry store
Modern Treasury Ledgers
Payments district
The payment service accepts a payment request, applies idempotency, tracks states such as pending, authorized, succeeded, failed, and reversed, and publishes events for the rest of the city. Stripe Payments is the reference example because it supports online and in-person payment acceptance across multiple payment methods.[1]
Lending district
The credit service combines applicant data, affordability signals, model output, and lending policy to return an offer, decline, or manual-review result. Affirm Adaptive Checkout is the reference example because it presents eligible customers with pay-over-time options, including Pay in 4 and longer-term installments.[2]
Fraud and identity district
The risk engine evaluates identity, device, behaviour, and transaction signals. Its output is an action—approve, challenge, review, or block—not just a score. Sift is the reference example because its platform supports fraud decisioning and investigation across account and payment activity.[3]
Compliance district
The compliance workflow screens customers and activity against legal and internal obligations, routes uncertain cases to people, and stores evidence for later review. ComplyAdvantage Mesh is the reference example because it combines customer screening, transaction monitoring, risk signals, case handling, and audit trails.[4]
Data and AI district
The data/AI layer cleans events, builds features, runs models, monitors outcomes, and sends usable signals back to decision services. Plaid Enrich is the reference example because it converts unstructured transaction descriptions into categorized records with merchant, category, location, and counterparty context.[5]
Ledger at the centre
The ledger records the financial effect of completed or pending events as balanced, append-only entries. It answers what is owed, owned, paid, refunded, or reversed. Modern Treasury Ledgers is the reference example because it provides an immutable system of record for transactions, balances, and money movement.[6]

A $20 purchase through the city
Start: A customer asks to pay a merchant $20.
Permission: Compliance checks whether the customer and activity can proceed; uncertain cases go to review.
Risk: Fraud and identity services evaluate the account, device, behaviour, and payment context.
Choice: Lending may offer a credit option; otherwise the customer pays in full.
Movement: Payments authorizes and processes the selected method while emitting explicit state changes.
Truth: The ledger records balanced entries for the obligation and resulting movement of value.
Learning: Data/AI enriches the event, measures the outcome, and improves future fraud, credit, and compliance signals.
A declined transaction must still be explainable. A reversed transaction must remain visible in history. A retry must not create a second charge. These are core financial behaviours, not edge cases.

Capstone design contract
Every district should satisfy the same engineering contract:
Clear input: Validate the event schema, customer context, amount, currency, and request identifier.
Explainable output: Return a decision, reason code, policy or model version, confidence where relevant, and next action.
Human accountability: Route ambiguous or high-impact cases to a review queue; record who made the final decision.
Safe state changes: Use idempotency keys, explicit state machines, and compensating entries for reversals.
Auditability: Keep timestamps, source events, model and rule versions, decision evidence, actors, and overrides.
Reconciliation: Compare payment-provider states, bank movement, and internal ledger balances; investigate drift.
Data discipline: Minimize sensitive data, restrict access, monitor data quality, and avoid training on unresolved outcomes.
Minimal event flow for the build
checkout.requested
  -> compliance.checked
  -> fraud.scored
  -> credit.decided (optional)
  -> payment.authorized
  -> ledger.transaction.posted
  -> transaction.enriched
  -> monitoring.updated
Each event should carry a stable transaction_id, customer_id, timestamp, status, reason code, and correlation ID. Sensitive fields should be referenced or tokenized rather than copied into every service.

Key terms
Product: The financial job the customer wants completed, such as paying, borrowing, saving, insuring, or investing.
Transaction: A recorded change in money, ownership, obligation, or financial state.
Risk: The probability and cost of an undesirable result.
Decision system: Data, model, policy, and action joined with accountable human oversight.
Audit trail: Evidence that reconstructs what happened, when it happened, and why.
Ledger: The authoritative financial record to which the rest of the system must reconcile.
Capstone takeaway
The capstone is not a collection of six APIs. It is one controlled financial system: payments move value, lending prices time, fraud protects trust, compliance enforces permission, data/AI improves decisions, and the ledger preserves truth.

Sources
Stripe Payments
Affirm Products: Adaptive Checkout
Sift Fraud Attack Defense Suite
ComplyAdvantage: AML software capabilities
Plaid Enrich
Modern Treasury Ledgers
