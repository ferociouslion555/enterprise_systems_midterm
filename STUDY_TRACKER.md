# CMPE 272 Midterm — Study Tracker ✅
**Target: Oct 7, 2026 (Week 8, in class)** — tick each box (`[ ]` → `[x]`) as you finish.

> Scope confirmed on the slides: **"The midterm covers sessions 1 to 7"** + assigned readings (Fowler ch. 2, Newman ch. 3, the Fowler microservices article).
> Suggested order: do the ⭐⭐⭐ lectures (4, 7, 3) first — they're the richest and most exam-likely.

---

## 📅 Day-by-day plan (6 days)

### Day 1 — Lecture 4: Cloud-Native ⭐⭐⭐ (start here)
- [ ] Read `Lecture-04_CMPE-272-Bond_FA26.pdf` + Newman ch. 3 + martinfowler.com/articles/microservices.html
- [ ] Microservices: definition (Lewis & Fowler), characteristics, Fowler's 9 traits
- [ ] Benefits vs the "microservice premium" (distribution, eventual consistency, ops complexity)
- [ ] **SLI vs SLO vs SLA** + error budget (write a one-line example of each)
- [ ] Sync vs async; orchestration vs choreography
- [ ] Circuit breaker, bulkheads, vertical vs horizontal scaling

### Day 2 — Lecture 7: Messaging & Service Composition ⭐⭐⭐
- [ ] Read `Lecture-07_CMPE-272-Bond_FA26.pdf` + microservices.io patterns (Saga, Outbox, Idempotent Consumer)
- [ ] UML: class/use-case/activity/sequence diagrams
- [ ] **Association vs aggregation vs composition** + multiplicity
- [ ] ER diagram: cardinality vs participation; PK/FK
- [ ] BPMN: pools/lanes/events; **gateway types** (exclusive/inclusive/parallel)
- [ ] Queue vs topic
- [ ] **Delivery guarantees:** at-most / at-least / exactly once
- [ ] Idempotency keys; **dual-write problem + outbox pattern**
- [ ] Retries/backoff/jitter + DLQ
- [ ] **Sagas** + compensating actions; Camunda vs Temporal vs Step Functions

### Day 3 — Lecture 3: App Frameworks & APIs ⭐⭐⭐
- [ ] Read `Lecture-03_CMPE-272-Bond_FA26.pdf` + Fowler ch. 2
- [ ] **REST: the 6 constraints** (client-server, stateless, cacheable, layered, uniform interface, code-on-demand)
- [ ] gRPC + Protocol Buffers; 4 streaming modes; why smaller/faster than JSON/XML
- [ ] SOA; ESB (what it does, loose coupling)
- [ ] Enterprise Integration Patterns: channel/router/translator/endpoint (Hohpe & Woolf, Camel)
- [ ] UI frameworks skim (jQuery, React, Angular, Node.js)

### Day 4 — Lecture 6: ERP / SCM / CRM ⭐⭐
- [ ] Read `Lecture-06_CMPE-272-Bond_FA26.pdf`
- [ ] Functional structure + silo effect; Enterprise Systems (end-to-end)
- [ ] Core business processes (buy/make/sell/plan/store/...)
- [ ] Client-server 3-tier; SOA + 4 properties of a service
- [ ] **ERP:** single DB, intra-company, components/modules; the Big Three (SAP/Salesforce/MS)
- [ ] Clean core; why ERP replacement is hard (Hershey/Lidl, strangler-fig)
- [ ] **CRM** (SFA is part of CRM); SCM; "CRM drives what SCM produces"

### Day 5 — Lectures 2a/2b + 5: Infrastructure & Platform Engineering ⭐⭐
- [ ] Read `Lecture-02a` + `Lecture-02b` + `Lecture-05`
- [ ] OS/Linux/systemd; GPL vs BSD/MIT; load average
- [ ] **Identity: SAML vs OAuth vs OIDC** (authn vs authz); Active Directory roles
- [ ] Storage: VFS/inode, RAID, block storage, page cache
- [ ] Networking: QUIC, VLAN/802.1q, STP, BGP vs IGP, control vs data plane, leaf-spine, NAT
- [ ] SDLC stages & milestones
- [ ] Platform engineering: IDP, golden path, paved road; **DORA 4 metrics**; policy-as-code

### Day 6 — Review + Lecture 1 + practice
- [ ] Skim `Lecture-01` (course overview, texts, objectives)
- [ ] Do all 15 likely-question types in the study guide (section 8)
- [ ] Re-skim every deck's headers for anything missed
- [ ] Light review the day before — rest well 💤

---

## 🧠 Must-be-able-to-define/contrast
- [ ] REST's 6 constraints
- [ ] SLI vs SLO vs SLA
- [ ] At-most / at-least / exactly-once delivery
- [ ] Idempotency + outbox (dual-write problem)
- [ ] Queue vs topic
- [ ] Orchestration vs choreography
- [ ] Saga + compensating actions
- [ ] BPMN gateways (exclusive/inclusive/parallel)
- [ ] UML association vs aggregation vs composition
- [ ] ERP vs CRM vs SCM; SFA ⊂ CRM
- [ ] SAML vs OAuth vs OIDC
- [ ] gRPC/Protobuf vs JSON/XML
- [ ] Microservices benefits vs costs
- [ ] DORA's 4 metrics
- [ ] Control plane vs data plane

## 🗺️ Must-be-able-to-place/choose
- [ ] Camunda vs Temporal vs Step Functions (pick one for a scenario)
- [ ] When to call vs when to queue
- [ ] Sync vs async architecture for read-heavy vs write-heavy

## 📖 Readings (don't skip — "exams draw on the full deck plus readings")
- [ ] Fowler — *Patterns of Enterprise Application Architecture*, ch. 2
- [ ] Newman — *Building Microservices*, ch. 3
- [ ] Fowler — microservices article (martinfowler.com/articles/microservices.html)
- [ ] Richardson — microservices.io patterns (Saga, Outbox, Idempotent Consumer)
