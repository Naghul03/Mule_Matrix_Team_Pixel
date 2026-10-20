# Mule Matrix

### Trace the Flow. Expose the Network.

> **Mule Matrix** is a proposed financial-fraud intelligence platform
> that combines simulated programmable e₹-style token lineage with
> off-chain graph analytics to investigate cross-bank money-mule
> networks, trace multi-hop fund flows, and flag suspicious cash-out
> patterns.

**Project status:** Hackathon prototype / proof of concept\
**Domain:** Financial cybersecurity, fraud analytics, graph
intelligence, CBDC-inspired transaction simulation

------------------------------------------------------------------------

## Overview

Money-mule networks help criminals move and disguise proceeds from scams
and other financial crimes. Funds can pass through multiple intermediary
accounts, be split into smaller transfers, converge in common
beneficiary accounts, and eventually leave the observable digital
payment environment through a cash or crypto off-ramp.

Mule Matrix is designed to help investigators see beyond isolated
transactions. It brings together two complementary approaches:

1.  **Token-lineage simulation:** Track the provenance and transfer
    history of simulated CBDC/e₹-style programmable tokens.
2.  **Off-chain graph intelligence:** Represent accounts or wallets as
    nodes and transactions as time-stamped edges, then analyze the graph
    to surface suspicious multi-hop paths and connected mule patterns.

The system is intended to help prioritize investigations, not to make an
automated finding of criminality. A risk score is an investigative
signal, not proof of fraud.

## The Problem

### What is happening?

Fraudsters can move stolen money across multiple accounts controlled by
or associated with money mules. These transactions may be distributed
across institutions and obscured by layering, rapid transfers,
splitting, or consolidation.

### Why is it difficult?

-   **Cross-bank blind spots:** Each institution may see only its own
    portion of a transaction chain.
-   **Fund layering:** Multi-hop transfers make it harder to reconstruct
    the origin and destination of funds.
-   **Delayed identification:** Suspicious activity may be recognized
    only after funds have reached a final beneficiary or off-ramp.
-   **Fragmented evidence:** Transaction records, account relationships,
    and timing patterns may be analyzed separately instead of as one
    connected network.

### Problem statement

Fraudsters move stolen funds through multi-hop money-mule networks
across financial accounts, making fund flows difficult to trace.
Fragmented visibility and rapid cash/crypto conversion can delay
detection and allow suspicious funds to leave the observable payment
ecosystem.

## Why This Problem Matters

A transaction that appears ordinary in isolation may become suspicious
when considered alongside related transfers, timing, repeated
counterparties, account behaviour, and network structure. Linking these
signals can help financial institutions and authorized investigators
prioritize cases earlier and understand how funds move through connected
accounts.

Mule Matrix explores whether token-lineage metadata and graph-based
analysis can provide a clearer investigation trail than single-account
or single-transaction monitoring alone.

------------------------------------------------------------------------

## Proposed Solution

Mule Matrix is an **autonomous dual-engine fraud-intelligence concept**
combining programmable e₹-style token-lineage simulation with an
off-chain Graph Neural Network (GNN) / graph-analytics engine.

The prototype is designed to:

-   Ingest simulated token-transfer and transaction events.
-   Preserve a traceable history of token transfers in the prototype
    data model.
-   Build a time-aware graph of accounts, wallets, and transfers.
-   Detect suspicious multi-hop movement, possible smurfing/layering,
    and connected mule patterns.
-   Calculate risk indicators and surface explainable evidence.
-   Flag potential final cash-out or off-ramp accounts for human review.

**Important:** This is a proposed prototype mechanism. It does not imply
that the live Indian Digital Rupee (e₹) currently exposes this exact
lineage functionality, that SHA-256 alone proves provenance, or that the
prototype can freeze or intercept real funds. Real-world intervention
would require authorized institutional integration, appropriate legal
authority, and operational controls.

## How It Works

1.  **Transaction ingestion**\
    Load simulated transfers with transaction IDs, pseudonymous sender
    and receiver IDs, timestamps, amounts, and available lineage
    references.

2.  **Lineage association**\
    Associate a simulated token or token portion with its transfer
    history. If funds are split, represent the resulting portions and
    their parent transfer relationships explicitly.

3.  **Graph construction**\
    Model wallets/accounts as nodes and transfers as directed,
    time-stamped edges. Attach features such as amount, frequency, time
    gap, and transfer count.

4.  **Pattern analysis**\
    Use graph rules, behavioural features, and---where implemented and
    evaluated---GNN-based methods to identify suspicious paths and
    connected clusters.

5.  **Risk scoring**\
    Combine explainable indicators into a risk score or priority tier.
    Keep the underlying reasons visible to the investigator.

6.  **Alert and investigation view**\
    Display suspicious paths, related accounts, lineage history,
    evidence, and review status in a dashboard.

7.  **Human review**\
    An authorized reviewer evaluates the evidence before any
    consequential action. The prototype should not automatically accuse,
    block, or freeze an account.

## Core Modules

### 1. Transaction and Token-Lineage Ingestion

-   Accepts synthetic transaction data.
-   Validates required fields and timestamps.
-   Associates transfers with simulated token-lineage references.
-   Uses pseudonymous identifiers in demonstration data.
-   Records data-quality issues instead of silently discarding them.

### 2. Token-Lineage Engine

-   Stores a transfer history for simulated e₹-style tokens.
-   Links a transfer to its parent lineage record where applicable.
-   Represents split transfers as linked child portions, with amounts
    that reconcile to the parent within defined rules.
-   Allows investigators to query upstream provenance and downstream
    movement.
-   Can use SHA-256 hashes as integrity fingerprints for canonicalized
    records.

**Hashing note:** A hash can help detect whether data changed relative
to a trusted stored hash. It does not, by itself, prove that the
original data was true, that a transfer was authorized, or that the
token is an actual CBDC instrument. Integrity controls need a trusted
process for generating, storing, and verifying hashes.

### 3. Graph Construction and Network Analytics

-   Creates a directed graph from transaction records.
-   Connects accounts through time-stamped transfers.
-   Supports multi-hop path exploration.
-   Identifies repeated counterparties, rapid pass-through activity,
    unusual fan-in/fan-out, and possible common beneficiaries.
-   Helps distinguish a single suspicious transaction from a potentially
    coordinated network.

### 4. Behavioural and GNN-Based Detection

Potential features include:

-   Number of incoming and outgoing transfers in a time window.
-   Transfer velocity and time between receipt and onward transfer.
-   Inflow/outflow ratio and pass-through ratio.
-   Number of unique counterparties.
-   Fan-in and fan-out degree.
-   Repeated transfer patterns and amount fragmentation.
-   Graph neighbourhood and multi-hop connectivity.
-   Proximity to previously reported or seeded suspicious entities in
    the synthetic scenario.

A GNN is a proposed advanced detection option, not a requirement for the
first working version. Begin with transparent graph rules and a baseline
model; add a GNN only if the available data and evaluation justify its
complexity.

### 5. Risk Scoring and Alert Management

-   Converts detection signals into a priority score or tier.
-   Explains which signals contributed to the score.
-   Supports review states such as `New`, `Under Review`, `Escalated`,
    `Cleared`, and `Closed`.
-   Records reviewer decisions for evaluation and future tuning.
-   Separates an alert from a confirmed fraud finding.

### 6. Investigation Dashboard

A dashboard can show:

-   Transaction timeline.
-   Multi-hop fund-flow graph.
-   Token-lineage view.
-   Suspicious account cluster.
-   Risk score and reasons.
-   Potential consolidation and off-ramp points.
-   Alert status and investigator notes.

### 7. Pre-Conversion Alert Concept

The prototype can flag a suspicious flow before a **simulated**
conversion or off-ramp event. A real pre-conversion alert or hold would
require the payment/off-ramp provider to integrate with the system and
authorize the action. Without such integration, the system can only
produce an alert or recommendation.

------------------------------------------------------------------------

## Example Transaction Flow

A demonstration scenario may use the following flow:

**Victim / Silent Ring → Bank A Mule → Multi-Account Mule Network →
Simulated e₹ Token Lineage → Off-Ramp (Cash/Crypto) → Suspected Final
Beneficiary**

This is a synthetic investigation scenario, not a claim about a real
case or a guaranteed detection outcome.

### Example stages

1.  A synthetic victim account sends funds to an intermediary account.
2.  The intermediary quickly distributes the funds to multiple accounts.
3.  The funds move through several hops and some flows converge on a
    common beneficiary.
4.  The lineage engine reconstructs the simulated transfer history.
5.  Graph analytics flags suspicious structural and behavioural
    patterns.
6.  The dashboard shows the route, evidence, and risk factors.
7.  A human reviewer decides whether further investigation is warranted.

------------------------------------------------------------------------

## Technical Approach

### A. Data ingestion and validation

Use a consistent transaction schema. Validate IDs, timestamps, amounts,
currency, and sender/receiver fields. Handle duplicate records, missing
fields, inconsistent amounts, and out-of-order events explicitly.

### B. Token-lineage representation

For a prototype, a lineage record can include:

-   `lineage_id`
-   `token_reference` or pseudonymous token/portion ID
-   `parent_lineage_id`
-   `transaction_id`
-   `sender_id`
-   `receiver_id`
-   `amount`
-   `timestamp`
-   `record_hash`
-   `source_type` (synthetic/imported)
-   `integrity_status`

If partial transfers are supported, define clear accounting rules for
how a parent amount is divided among child portions and how remaining
balances are represented. Do not imply that ordinary bank records always
provide enough information to trace fungible money at an
individual-token level.

### C. Graph model

A simple graph model is:

-   **Node:** pseudonymous account or wallet.
-   **Directed edge:** transaction from sender to receiver.
-   **Edge attributes:** transaction ID, amount, timestamp,
    channel/type, lineage reference.
-   **Optional node attributes:** account age band, activity summary,
    seeded/report status, and calculated behavioural features.

The graph can be queried to trace paths, identify clusters, and
calculate structural indicators. Use bounded path depth and time windows
to keep queries manageable.

### D. Detection strategy

A practical progression is:

1.  **Rule-based baseline:** fast pass-through, unusual fan-in/fan-out,
    fragmented transfers, multi-hop chains, and suspicious convergence.
2.  **Classical ML baseline:** train a model on labelled synthetic
    scenarios or carefully defined proxy labels.
3.  **Graph analytics:** connected components, path searches,
    centrality, community patterns, and temporal motifs where useful.
4.  **GNN experiment:** use only if enough suitable graph-labelled data
    is available and the result beats simpler baselines on held-out
    data.

A model should be evaluated against a baseline and tested on scenarios
not used during development. Synthetic labels can encode assumptions and
do not guarantee real-world performance.

### E. Alert evidence

Every alert should include a human-readable reason, for example:

-   High transfer velocity after receiving funds.
-   Several outgoing transfers shortly after a large incoming transfer.
-   Unusual multi-hop path or repeated network motif.
-   Multiple suspicious paths converging on a common destination.
-   A lineage record linking the transfer to a previously flagged
    synthetic flow.

These are indicators for review, not proof that an account is controlled
by a criminal.

------------------------------------------------------------------------

## Technology Stack

The intended open-source stack discussed for the prototype is:

  -----------------------------------------------------------------------
  Layer                   Technology              Role
  ----------------------- ----------------------- -----------------------
  Frontend                React                   Investigation dashboard
                                                  and graph visualization

  Backend                 Python + FastAPI        APIs, validation,
                                                  orchestration, alerts

  Graph database          Neo4j                   Account relationships,
                                                  transfer paths, graph
                                                  queries

  Data processing         Python                  Feature extraction,
                                                  validation, scoring

  ML / graph ML           Scikit-learn and/or a   Baselines and
                          graph ML framework, if  experimental models
                          selected                

  Data input              CSV/JSON synthetic      Reproducible prototype
                          datasets                scenarios

  Deployment              Local development first Low-cost testing
                                                  without paid cloud
                                                  services
  -----------------------------------------------------------------------

The exact libraries and versions should be recorded in the repository's
dependency files. Only list a component as implemented once it exists in
the codebase.

## System Architecture

``` text
Synthetic Transaction / Token Events
                |
                v
       Ingestion & Validation
                |
                v
       Token-Lineage Service
                |
                v
       Graph Builder / Neo4j
                |
                v
 Behavioural Features + Graph Analytics
                |
       +--------+---------+
       |                  |
       v                  v
 Risk Scoring        Path / Cluster Analysis
       |                  |
       +--------+---------+
                |
                v
       Alert & Evidence API
                |
                v
       React Investigation Dashboard
                |
                v
       Human Review / Case Outcome
```

The lineage engine and graph engine provide complementary views: lineage
follows the modeled transfer history, while graph analytics identifies
relationships and patterns across accounts.

## Data Model

A prototype may use the following logical entities:

### Transaction

  Field                Description
  -------------------- -----------------------------------------------
  `transaction_id`     Unique synthetic transaction ID
  `sender_id`          Pseudonymous sender account/wallet
  `receiver_id`        Pseudonymous receiver account/wallet
  `amount`             Transfer amount in the dataset's defined unit
  `timestamp`          Event time in a documented format
  `transaction_type`   Transfer, split, conversion simulation, etc.
  `lineage_id`         Link to lineage record, if available

### Lineage record

  -----------------------------------------------------------------------
  Field                               Description
  ----------------------------------- -----------------------------------
  `lineage_id`                        Unique lineage record ID

  `parent_lineage_id`                 Parent record for split/derived
                                      portions, if applicable

  `transaction_id`                    Related transfer

  `token_reference`                   Pseudonymous simulated token or
                                      portion

  `record_hash`                       Optional integrity fingerprint

  `integrity_status`                  Result of verification against
                                      trusted record
  -----------------------------------------------------------------------

### Alert

  Field              Description
  ------------------ --------------------------------------------------
  `alert_id`         Unique alert ID
  `entity_id`        Account, wallet, path, or cluster being reviewed
  `risk_score`       Model or rule-based score
  `risk_tier`        Configured priority category
  `evidence`         Reasons and linked transactions
  `status`           Review workflow status
  `review_outcome`   Human-reviewed outcome, if available

------------------------------------------------------------------------

## Detection and Risk Scoring

A transparent prototype can calculate a risk score from normalized
features, for example:

-   Transfer velocity.
-   Short dwell time before onward transfer.
-   Unusual fan-in/fan-out.
-   Multi-hop path structure.
-   Fragmentation or rapid movement of amounts.
-   Convergence on a common beneficiary.
-   Link to a seeded or previously reported synthetic entity.

Weights and thresholds should be calibrated using the prototype dataset
and documented clearly. Avoid treating any single feature as decisive.
Do not hard-code a claim such as "100% interception" or "zero false
positives."

### Reported and unreported networks

-   **Reported / seeded cases:** Evaluate whether the system can trace
    known synthetic suspicious entities and their connected transfers.
-   **Unreported / unknown cases:** Evaluate whether unsupervised graph
    and behavioural patterns can surface previously unlabelled synthetic
    networks for review.

"Unreported" means not labelled in the test scenario; it does not mean
the system can identify all previously unknown real-world criminal
networks.

------------------------------------------------------------------------

## Privacy, Security, and Responsible Use

Mule Matrix should be designed around privacy and controlled access:

-   Use synthetic or appropriately pseudonymized data for development
    and demonstrations.
-   Minimize collection of personally identifying information.
-   Apply role-based access controls to investigation views and APIs.
-   Protect secrets and credentials with environment variables or a
    secret manager.
-   Validate and sanitize inputs; avoid exposing sensitive data in logs.
-   Maintain audit logs for access, alert changes, and reviewer actions.
-   Define retention limits and deletion processes for demonstration
    data.
-   Show evidence and uncertainty alongside risk scores.
-   Require human review before consequential decisions.
-   Test for false positives and potential bias before any real-world
    use.

A hash of an account identifier is not automatically anonymous. If it is
deterministic or can be guessed from a small identifier space, it may
still be linkable. Use keyed pseudonymization and access controls where
appropriate, and do not publish secrets or real customer records.

------------------------------------------------------------------------

## Feasibility

### Technical feasibility

The first version is feasible as a local prototype using open-source
components, synthetic transaction data, a graph database, and a web
dashboard. The highest-value initial scope is:

1.  Synthetic transaction ingestion.
2.  Multi-hop graph visualization and path tracing.
3.  Lineage records for simulated token transfers.
4.  Explainable rule-based risk scoring.
5.  Alert dashboard with human-review status.

GNN training and more complex lineage accounting can follow once the
baseline works and the data model is validated.

### Running cost

-   Python/FastAPI, React, and Neo4j have open-source options.
-   Local development avoids mandatory paid APIs or cloud hosting.
-   Main costs for a future production system would include
    infrastructure, integration, monitoring, compliance, security, and
    specialist staffing.

### Risks and fallback

-   **False positives:** Use human review, evidence-based explanations,
    and threshold evaluation.
-   **Data gaps:** Display incomplete lineage and confidence
    limitations.
-   **Graph scale:** Bound path depth and time windows; optimize
    queries.
-   **Model complexity:** Retain a rule-based baseline if a GNN does not
    provide measurable improvement.
-   **Integration limits:** Demonstrate with synthetic events instead of
    implying access to real bank or CBDC systems.

------------------------------------------------------------------------

## Business Analysis

### Mission

Deliver explainable, graph-based fraud intelligence that helps
authorized financial teams investigate suspicious fund flows more
effectively.

### Vision

Support proactive identification and investigation of cross-institution
money-mule networks while respecting privacy, due process, and
institutional controls.

### Target users

-   Banks and financial institutions.
-   Payment providers and fintechs.
-   Fraud and financial-crime investigation teams.
-   Authorized financial intelligence and law-enforcement units, subject
    to applicable legal authority.

### Customer problem

Single-institution monitoring may not reveal a full multi-hop route when
relevant transactions are distributed across accounts or institutions.
Investigation teams need tools to connect evidence, reconstruct fund
movement, and prioritize suspicious cases.

### Value proposition

-   **Network visibility:** Connects related transfers instead of
    examining each transaction in isolation.
-   **Traceability:** Presents modeled token lineage and transaction
    paths.
-   **Prioritization:** Ranks cases for review using explainable risk
    indicators.
-   **Investigation efficiency:** Brings transaction paths, evidence,
    and alert context into one view.
-   **Extensibility:** Can be evaluated with synthetic data and later
    explored in controlled, authorized pilots.

These are expected value propositions to validate through testing and
customer discovery, not proven business outcomes.

### Market and competition

Mule Matrix sits within the broader financial-crime analytics,
transaction monitoring, fraud detection, and graph-intelligence space.

Potential alternatives include:

-   Existing bank transaction-monitoring systems.
-   Rules-based anti-fraud platforms.
-   Commercial graph analytics and financial-crime investigation tools.
-   Manual case investigation and cross-team information sharing.

**Potential differentiator:** A prototype that combines simulated token
lineage with multi-hop graph analysis and an explainable investigation
workflow. Its advantage must be validated against existing systems; the
project should not claim that competitors lack these capabilities
without research.

### Business model

A possible future model is **B2B SaaS or enterprise licensing** for
institutions, subject to customer validation and regulatory
requirements.

Potential pricing approaches:

-   Tiered subscription based on transaction volume and features.
-   Enterprise licensing for larger institutions.
-   Paid pilot or sandbox evaluation.
-   Optional deployment and integration services.

No pricing or revenue projections should be presented as validated until
cost modelling and customer interviews are completed.

### Financial plan

**Potential initial investment areas** - Development and testing
infrastructure. - Data preparation and synthetic scenario design. -
Model and graph evaluation. - Security and compliance review. - Pilot
integration and monitoring.

**Potential operating costs** - Compute, storage, and observability. -
Engineering and data-science effort. - Security maintenance and incident
response. - Compliance, legal review, and partner integration.

**Potential revenue** - Subscription fees. - Enterprise licenses. - Paid
pilots and integration services.

**Scale strategy** Start with a reproducible prototype, validate
performance on synthetic scenarios, conduct controlled evaluations with
appropriate partners, and only then assess wider deployment.
Cross-border expansion would require separate legal, privacy, and
operational analysis.

------------------------------------------------------------------------

## Impact and Benefits

### What measurably improves?

Mule Matrix is designed to:

-   Surface suspicious mule-network patterns earlier in a simulated
    transaction flow.
-   Trace multi-hop transfers and identify possible consolidation or
    off-ramp points.
-   Reveal connections between wallets and common beneficiaries.
-   Give investigators a clearer evidence trail for review.

### Who could use it?

Banks, payment providers, fintechs, and authorized investigators may use
this type of tooling to support proactive financial-fraud
investigations.

### Benefits

**Faster and smarter investigations** - Combines transfer history and
graph relationships in one investigation view. - Helps investigators
prioritize suspicious paths and connected accounts.

**Reduced fraud impact** - Can flag suspicious simulated flows before a
modeled off-ramp event. - May support earlier intervention where a
participating institution has an authorized alert or control workflow.

**Stronger network visibility** - Connects activity across multiple hops
rather than focusing only on isolated transactions. - Highlights
possible mule clusters and fund convergence.

**Measurable evaluation** Track precision, recall, false-positive rate,
path reconstruction accuracy, lineage coverage, alert latency, and
investigator review time. Report actual values only after testing.

------------------------------------------------------------------------

## Risks and Mitigations

  -----------------------------------------------------------------------
  Risk                                Mitigation
  ----------------------------------- -----------------------------------
  False positives                     Human review, explainable reasons,
                                      threshold calibration

  Incomplete transaction data         Show missing links and uncertainty
                                      explicitly

  Misuse of risk scores               Treat alerts as leads, not proof of
                                      guilt

  Privacy leakage                     Synthetic/pseudonymized data,
                                      access control, data minimization

  Misleading lineage claims           Document assumptions; distinguish
                                      simulated lineage from actual CBDC
                                      capabilities

  Graph performance issues            Bound path depth, index data,
                                      profile queries

  GNN overfitting                     Compare with simple baselines and
                                      use held-out scenarios

  Lack of real-world integration      Clearly label as prototype; test in
                                      an authorized sandbox before
                                      deployment
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Evaluation Plan

Evaluate the prototype with synthetic scenarios containing both ordinary
and suspicious patterns.

### Suggested scenarios

-   Normal transfers between unrelated accounts.
-   Rapid pass-through transfers.
-   Funds split across several accounts.
-   Multiple paths converging on a common beneficiary.
-   Long multi-hop chains.
-   Benign high-volume accounts that should not automatically be
    flagged.
-   Missing or inconsistent lineage metadata.
-   Transfers that cross the configured time-window boundary.

### Suggested metrics

  -----------------------------------------------------------------------
  Metric                              What it measures
  ----------------------------------- -----------------------------------
  Precision                           How many flagged cases are actually
                                      suspicious in the labelled test
                                      data

  Recall                              How many seeded suspicious cases
                                      are detected

  False-positive rate                 How often benign cases are
                                      incorrectly flagged

  Path reconstruction accuracy        Whether the system reproduces
                                      expected synthetic transfer paths

  Lineage coverage                    Proportion of eligible simulated
                                      transfers with traceable lineage
                                      records

  Alert latency                       Time from ingestion to alert
                                      creation

  Review efficiency                   Time required to understand and
                                      assess a case in the dashboard
  -----------------------------------------------------------------------

Compare graph/ML methods with a simple rule-based baseline. Document the
dataset, assumptions, test split, and limitations so results are
reproducible.

------------------------------------------------------------------------

## Scope and Limitations

### In scope

-   Simulated CBDC/e₹-style programmable tokens with lineage metadata.
-   Synthetic transaction generation or ingestion.
-   Reported/seeded and unlabelled synthetic mule-network scenarios.
-   Behavioural features, graph analytics, and experimental GNN
    detection.
-   Explainable risk scores, alerting, and investigation visualization.
-   Local, low-cost prototype development.

### Out of scope for the prototype

-   Direct integration with live RBI or bank systems.
-   Access to real customer or banking records without authorization.
-   Automatic freezing or seizure of real funds.
-   Guaranteed identification of a real-world "ganglord."
-   Claims of complete traceability for actual e₹ tokens.
-   Production-scale cross-bank intervention without partner integration
    and legal authorization.

### Key limitations

The accuracy of detection depends on data quality, feature design, and
evaluation. Synthetic datasets cannot fully reproduce real-world fraud
behaviour. Token-level tracing is meaningful only where the modeled
transfer system preserves sufficient lineage information. Actual
deployment requires technical validation, institutional cooperation,
privacy/security assessment, and regulatory review.

------------------------------------------------------------------------

## Roadmap

### Phase 1 --- Working baseline

-   Define synthetic transaction schema.
-   Ingest and validate data.
-   Build a transaction graph.
-   Trace multi-hop paths.
-   Create a basic investigation dashboard.

### Phase 2 --- Lineage and alerts

-   Implement simulated token lineage and split-transfer accounting.
-   Add integrity fingerprints and verification.
-   Implement transparent risk rules and evidence-based alerts.
-   Add human-review workflow.

### Phase 3 --- Model evaluation

-   Create reproducible benign and suspicious scenarios.
-   Evaluate precision, recall, false positives, and path
    reconstruction.
-   Compare rules with classical ML and graph-based approaches.
-   Test GNN only if suitable labelled graph data is available.

### Phase 4 --- Controlled pilot exploration

-   Seek an authorized sandbox or research partner.
-   Review data protection, security, legal authority, and operational
    integration.
-   Benchmark against a conventional graph-based baseline.
-   Assess scalability and business value before production claims.

------------------------------------------------------------------------

## Project Summary

**Mule Matrix --- Trace the Flow. Expose the Network.**

Mule Matrix explores how simulated e₹ token lineage and graph
intelligence can work together to reconstruct multi-hop fund flows,
surface hidden money-mule networks, and prioritize suspicious cash-out
patterns for human investigation. Its prototype-first, open-source
approach is designed for transparent evaluation before any real-world
integration or deployment.
