# CMPE 272 — Enterprise Software Platforms — Midterm Study Guide
**Exam date:** Tue, Oct 7, 2026 (Week 8, in class) · **Weight:** 10% of final grade
**Instructor:** Andrew Bond, SJSU

> **Scope — straight from the slides:** *"The midterm covers sessions 1 to 7."* Also: *"The full deck is posted on Canvas; exams draw on the full deck plus readings."* So study Lectures 1–7 **and** the assigned readings (Fowler, Newman).

---

## 0. Which slides/lectures to study (scope)

| Priority | Lecture (file) | Topic | Week |
|----------|----------------|-------|------|
| ⭐⭐⭐ | `Lecture-04_CMPE-272-Bond_FA26.pdf` | **Cloud-Native: microservices, SLI/SLO/SLA, sync vs async, circuit breaker** | 4 |
| ⭐⭐⭐ | `Lecture-07_CMPE-272-Bond_FA26.pdf` | **Messaging, delivery guarantees, outbox, sagas, BPMN, UML/ER** | 7 |
| ⭐⭐⭐ | `Lecture-03_CMPE-272-Bond_FA26.pdf` | **App frameworks & APIs: REST, gRPC/Protobuf, SOA, ESB, EIP** | 3 |
| ⭐⭐ | `Lecture-06_CMPE-272-Bond_FA26.pdf` | **ERP / SCM / CRM, enterprise systems, SOA** | 6 |
| ⭐⭐ | `Lecture-02a/02b-...pdf` | Enterprise infrastructure, OS, identity (SAML/OAuth/AD), storage, networking | 2 |
| ⭐⭐ | `Lecture-05_CMPE-272-Bond_FA26.pdf` | Platform engineering, SDLC, CI/CD, DORA, IDP | 5 |
| ⭐ | `Lecture-01_CMPE-272_Bond_FA26.pdf` | Intro, course overview, texts | 1 |

**Required readings referenced on the slides:** Martin Fowler *Patterns of Enterprise Application Architecture* (ch. 2), Sam Newman *Building Microservices* (ch. 3), and <http://martinfowler.com/articles/microservices.html>. Also Chris Richardson's patterns: Saga, Transactional Outbox, Idempotent Consumer (microservices.io).

> **Grading context:** Midterm 10% · Final 25% · Term project 33% · Homework 22% · In-class 10%. The exam is one of the smaller-weight items — study efficiently, prioritize the ⭐⭐⭐ decks.

---

## 1. Introduction & Overview (Lecture 1)

- **Enterprise software platforms** = technologies, architectures & practices to build/operate software **at organizational scale**: infrastructure → app frameworks → data platforms → enterprise apps → AI integration → security → operations.
- **Learning objectives worth knowing (good for conceptual Qs):** describe the enterprise stack end-to-end; evaluate frameworks/APIs; compare cloud-native options (containers, serverless, mesh) on cost/complexity/ops; explain ERP/SCM/CRM; model a business process & compose services; select a datastore for an access pattern; integrate AI/ML; assess security (identity, zero trust, supply chain); instrument for resilience/observability (SLOs, incident response).
- **Key texts:** Fowler — *Patterns of Enterprise Application Architecture*; Newman — *Building Microservices*. Recommended: *Pragmatic Programmer*, *Enterprise Integration Patterns* (Hohpe & Woolf), *Next Generation SOA* (Erl).

---

## 2. Enterprise Infrastructure (Lecture 2a + 2b)

### OS & compute (2a)
- OS dimensions: single/multi-tasking, single/multi-user, distributed (clustered/networked), templated (VM/container image), lightweight, embedded/real-time.
- **Linux:** started 1991 by Linus Torvalds. Distros: Slackware, RedHat/CentOS, Debian/Ubuntu, SUSE. **CentOS Linux 8 ended 2021** → CentOS Stream becomes the *upstream (development)* branch of RHEL.
- **systemd:** system & service manager / init system; bootstraps user space, manages processes; replaces init.d scripts with **declarative unit files**; language-agnostic API.
- **GPL (copyleft):** freedom to run/study/share/modify; derivative work must use the **same license**. Contrast with **permissive** licenses (BSD, MIT).
- **Load average** = avg system load over 1/5/15 min (processes running or waiting). Seen via `uptime`, `top`, `cat /proc/loadavg`.

### Identity Management (2a) ⭐
- **SAML** = Security Assertion Markup Language; **XML-based** standard for exchanging **authentication AND authorization** between security domains.
- **OAuth** = open standard for **access delegation** (grant apps access without sharing passwords). It is **authorization, not authentication**. **OpenID Connect (OIDC)** = authentication layer built **on top of OAuth 2.0**.
- **Identity Provider (IdP)** = trusted system that authenticates users on behalf of other sites.
- **Active Directory (AD)** = Microsoft directory service on Windows Server. Domain controller runs **AD DS** and authenticates/authorizes users & computers, enforces policy. AD roles: **AD DS** (users/computers/policies), **AD CS** (certificates), **AD FS** (federation / cross-boundary access), **AD RMS** (rights mgmt), **AD LDS** (lightweight directory).

### Filesystems & Storage (2a)
- **VFS (Virtual File System):** interface between kernel and concrete FS (ext4, etc.); introduced by Sun 1985. Three key metadata objects: **super block, dentry, inode**. An **inode** holds all info about a file *except its name* (owner, permissions, size, timestamps, data pointers).
- Optimizations: RAM >> disk; sequential IO >> random IO → use caches/write buffers, prefer sequential layout. **Page cache** (per-file, ~4KB pages) absorbs writes; dirty pages flushed every ~5–30s or on memory pressure.
- **HDD:** rotating magnetic disks (IBM, 1956). **RAID** = combine multiple disks into logical units for redundancy, performance, or both.
- **Block storage:** fixed-size blocks (vs file/object storage).

### Networking (2b) ⭐
- **OSI model** (7 layers); devices: routers, switches, bridges, hubs. Utilities: ping, telnet, ssh, ifconfig, ip. IPv4 addressing: classes, subnet, mask, VLSM.
- **QUIC:** runs over **UDP**; adds reliability/ordering/encryption; solves TCP **head-of-line blocking** via per-stream multiplexing; faster connection setup. Basis of HTTP/3.
- **Switching** = forwarding decision pre-determined / in hardware (vs routing in software).
- **VLAN** = logical partition of a flooding domain; **trunk** carries multiple VLANs via tagging. **IEEE 802.1q** tag: EtherType 0x8100, 12-bit VLAN ID, 3-bit priority, 1-bit CFI. Untagged frames → **Native VLAN**.
- **L2 forwarding:** no protocol builds the table; MAC/CAM table built by watching **source MAC**; unknown destination → **unknown-unicast flooding**.
- **Spanning Tree Protocol (STP):** builds a loop-free L2 topology, disables redundant links, allows backup links. Invented by **Radia Perlman** (DEC). **RSTP (802.1w)** = faster convergence.
- **L3 routing:** **IGP** (RIP, IGRP/EIGRP, IS-IS, OSPF) within an AS; **EGP = BGP** between ASes (eBGP learns reachability from neighbor ASes, iBGP propagates internally).
- **Billing:** **CIR** (Committed Information Rate — guaranteed, hard cap); **P95** (95th-percentile metering, allows bursting).
- **Control plane vs data plane:** control plane = *decides* where traffic goes (routing protocols, signalling, not time-critical); **data/forwarding plane** = *forwards* each packet to next hop (fast path).
- **Clos / Leaf-Spine:** modern DC topology. Leaves = top-of-rack switches, spines = core; leaves connect only to spines (not each other); non-blocking, loop-free without STP. Example: Cisco ACI (VXLAN/VTEP).
- **NAT:** inside/outside × local/global addresses; **Static NAT** (1:1), **Dynamic NAT** (pool).
- **Reading:** Fowler ch. 2, Newman ch. 3.

---

## 3. Application Frameworks & APIs (Lecture 3) ⭐⭐⭐

### Evolution
- Legacy client/server → RPC, CORBA, SOAP, Web Services → **REST** → SOA → microservices/service mesh.

### REST (Representational State Transfer) — Roy Fielding ⭐
- **6 architectural constraints:**
  1. **Client-server** (uniform interface separates them)
  2. **Stateless** (no client context stored on server between requests)
  3. **Cacheable**
  4. **Layered system**
  5. **Uniform interface**
  6. **Code on demand** (optional)
- APIs that follow these = **RESTful APIs**.
- Properties aimed at: scalability, simplicity, modifiability, visibility, portability, reliability.
- **RAML** = RESTful API Modeling Language (YAML-based description; analogous to **WSDL** in SOAP).

### gRPC & Protocol Buffers ⭐
- **Protocol Buffers (Protobuf):** flexible/efficient mechanism for serializing structured data ("like XML, but smaller, faster, simpler"); 3–10× smaller, 20–100× faster. `.proto` file + compiler generates data-access classes (Java/C++/Python). Fields have tags: `required string name = 1;`.
- **gRPC** = Google RPC; uses Protobuf as its **IDL** and HTTP/2 transport. Four call types: **unary, server streaming, client streaming, bidirectional streaming**. HTTP/2 gives multiplexing, bidirectional streaming, header compression.

### SOA, ESB & Messaging ⭐
- **Enterprise Service Bus (ESB):** middleware that distributes work among components; uniform message movement; **subscription model**; promotes **loose coupling**. Advantages: interoperability, scalability, flexibility, system integration.
- **Message bus / message queue:** shared messaging infrastructure; two processes exchange info via a common queue (sender places a message, another process reads it).
- **Enterprise Integration Patterns (EIP)** — book by **Gregor Hohpe & Bobby Woolf**; a *pattern language* for messaging. Apache **Camel** implements EIP. 60+ patterns incl.:
  - **Message Channel** — how apps connect.
  - **Message Router** — direct a message based on conditions.
  - **Message Translator** — convert formats between systems.
  - **Message Endpoint** — how an app sends/receives on a channel.
- Platforms: IBM WebSphere MQ, webMethods, BizTalk, RabbitMQ, Oracle Service Bus, ServiceMix.

### UI frameworks
- jQuery (DOM via CSS selectors), React (view layer, component-based, Facebook), Angular (MVW), Node.js (V8, event-driven non-blocking I/O, npm), Bootstrap/Foundation (responsive), Vue, etc.

---

## 4. Cloud-Native Services (Lecture 4) ⭐⭐⭐ (highest priority)

### Microservices (Newman / Fowler) ⭐
- **Definition (Lewis & Fowler, 2014):** a single application as a **suite of small services**, each in its **own process**, communicating via **lightweight mechanisms** (often HTTP API), built around **business capabilities**, **independently deployable**, with minimal centralized management, potentially polyglot.
- Characteristics: small & focused (Single Responsibility), **autonomous** (comms via network/APIs), **technology heterogeneity**, ease of deployment, composability, optimized for replaceability.
- Fowler's 9 traits: componentization via services; organized around business capabilities; products not projects; **smart endpoints & dumb pipes**; decentralized governance; decentralized data management; infrastructure automation; **design for failure**; evolutionary design.
- Benefits: strong module boundaries, independent deployment, technology diversity.
- Costs ("microservice premium"): **distribution** (remote calls slow/fail), **eventual consistency**, **operational complexity** (need mature ops). Only worth it for complex systems.
- **Modulith** (2026 update): modularity *within* a monolith — monolith's simple deployment + microservice-like modularity.

### SLI / SLO / SLA (Google SRE) ⭐ likely exam Q
- **SLI (Indicator):** a quantitative *measure* of service level (latency, error rate, throughput, availability).
- **SLO (Objective):** a *target value/range* for an SLI (e.g., avg latency < 120 ms; "five nines" = 99.999% uptime ≈ 5.26 min downtime/year).
- **SLA (Agreement):** a *contract* with users, with consequences/penalties for breaching an SLO.
- **Error budget** = allowed unreliability (1 − SLO).

### Synchronous vs Asynchronous ⭐
- **Sync:** caller waits for the operation to complete. **Async:** caller doesn't wait (may not care if/when it completes).
- Two collaboration styles: **request/response** vs **event-based**.
- **Sync patterns:** decentralized sync; orchestrated sync sequential (central orchestrator holds all requests → single point of failure, burdens orchestrator); orchestrated sync parallel (faster, higher throughput, more complex). All better for **read-heavy** systems.
- **Async patterns:** well-suited to distributed systems; central **message bus** gives consistent delivery semantics. **Choreographed async events** (each service listens to the bus, context in event payload, scales for **write-heavy**); orchestrated async sequential; **hybrid** (orchestration for explicit flow + choreography for implicit).
- **Trade-offs:** async is harder to follow and more complex, but a natural fit for write-heavy systems; needs a **sync wrapper** (stateful entry point) for synchronous reads.

### Orchestration vs Choreography ⭐
- **Orchestration:** a central brain tells each service what to do (explicit flow).
- **Choreography:** services react to events, no central coordinator (implicit flow).
- Example (MusicCorp customer creation): loyalty points record + welcome pack + welcome email — can be done either way.

### Failure modes & guards (the deck groups failures into 3 kinds)
- **Latency / cascade failure:** symptoms = rising p95/p99 latency, timeout spikes, pools pinned, circuit breakers opening. Guards = strict per-hop timeouts, retry budgets w/ exponential backoff + jitter, circuit breakers + graceful fallbacks, small caches for hot reads.
- **Saturation / backpressure (resource exhaustion):** symptoms = CPU/mem/connections pegged, backlog growth, 429/503s, GC pauses (often from traffic spikes or retry storms). Guards = **bulkheads** (per-dependency pools/quotas), bounded queues & concurrency limits, load shedding / rate limiting, autoscale on queue depth/lag, isolate by cell/AZ.
- **Contract & state correctness (schema drift, duplicates, ordering):** symptoms = parse/validation errors, silent truncation, 4xx bursts after a deploy, double-processing. Guards = versioned schemas (JSON/Proto) with backward compatibility, schema registry, sagas/compensations, reconciliation jobs.

### Circuit Breaker (Michael Nygard, *Release It!*) ⭐
- Remote calls can fail or hang until a timeout; many callers on an unresponsive supplier can exhaust resources → **cascading failures**.
- **Basic idea:** wrap a protected call in a circuit-breaker object that **monitors for failures**; once failures hit a **threshold** the breaker **trips** and further calls return an error immediately (without making the protected call). Add a **monitor/alert** when it trips, and a **reset mechanism** once calls succeed again.
- Related guards: **Timeouts, Bulkhead, Isolation**. **Antifragile** org = embracing failure to improve resilience (Chaos Monkey).

### Idempotency ⭐
- **Idempotent** = a client can make the same call repeatedly and get the **same result/effect** as making it once.
- Idempotent ≠ stateless: the server may keep state, but repeating the call leaves that state exactly as one call left it.
- HTTP verbs: **GET, PUT, DELETE** are defined idempotent; **POST is NOT** unless you give it an **idempotency key**. (Verbs are only idempotent if the service *implements* them that way.)
- Why it matters for scaling: with replicas, requests may hit any node → same outcome; prevents unintended side effects from retries/duplicates; enables better caching; simplifies error handling (safe to retry transient errors).

### Scaling
- **Vertical scaling** = bigger machines. **Horizontal scaling** = many small machines.
- Current practice: **containers on a managed orchestrator** (Kubernetes, ECS, Cloud Run), serverless for spiky/event-driven pieces.
- Affinity / anti-affinity (host, AZ, region); worker-based (Spark, Flink, Ray).
- **Scaling databases:** reads → **caching + read replicas**; writes → **sharding** (hash on primary key picks the node; Cassandra replicates across a ring for resiliency).
- **CQRS** = separate read and write models: **commands** update data (task-based, e.g. "Book hotel room", may be queued/async); **queries** never modify data (return a DTO with no domain logic).

### Caching
- **Proxy caching** — proxy between client & server (reverse proxy or **CDN**).
- **Server-side caching** — server handles it (Memcached, ElastiCache).

### Load balancers (AWS, know ALB vs NLB vs GWLB)
- **Classic/ELB** (2009, legacy) — like an Nginx/HAProxy instance; routes only by port.
- **ALB (Application LB)** — **Layer 7 (HTTP)**; rich routing by host, path, query string, method, headers, source IP, port; can target many ports, and route to **Lambda**.
- **NLB (Network LB)** — Layer 4; listeners → target groups → instances/containers/IPs with health checks.
- **GWLB (Gateway LB)** — Layer 3; deploy/scale inline network appliances (firewalls, IDS/IPS); uses GENEVE over UDP 6081.

### Service discovery & registries
- Plain **DNS** isn't suited to microservice churn unless kept current (Kubernetes does this via **CoreDNS + Services**).
- Dynamic registries: **Zookeeper**, Consul, etc. (ephemeral topology, failure detection).

### Containers & orchestration (one-slide summary) ⭐
- A **container** = a process with its own filesystem **image** and resource limits (namespaces, cgroups). The **image** is the unit you build/test/ship.
- An **orchestrator** (Kubernetes, ECS, Cloud Run, Nomad) places containers, restarts them, scales replicas, routes traffic, rolls versions out/back.
- **Kubernetes objects to know:** Pod, Deployment, Service, Ingress, ConfigMap, Secret, Namespace. You declare **desired state in YAML**; controllers **reconcile** the cluster to it.
- **Managed control planes:** EKS (AWS), GKE (Google), AKS (Azure). What stays yours: images, resource requests/limits, health probes, autoscaling, cost. **Sizing rule: request what you use.**

### Service mesh ⭐
- A **proxy beside every service** (**sidecar**) intercepts every service-to-service call. Proxies: **Envoy, Linkerd**; meshes: **Istio, Linkerd, Consul**. App code unchanged.
- Gives you: **traffic policy in config** (timeouts, retries, circuit breaking, canary/traffic splitting, fault injection); **security by default** (mutual TLS between services, per-workload identity, authz at the proxy); **observability for free** (uniform latency/error/throughput metrics, traces, access logs per hop).
- Since 2024 the sidecar is **optional** — ambient / sidecar-less modes (Istio ambient, Cilium) move the proxy to the node to cut cost.
- **Worth it:** dozens of services in several languages, mTLS/audit requirements, progressive delivery, a platform team to own it. **Not worth it:** a few services in one language (use a library like Resilience4j/Polly), a modulith, a small team. **Costs:** per-pod CPU/memory (~1 ms/hop), a control plane to upgrade, a second place policy lives, harder debugging.

### Event-driven collaboration checklist (the deck's 5-point recipe)
1. **Contract:** CloudEvents 1.0 attributes on every message (`id, source, type, subject, time, datacontenttype, dataschema`). JSON (easy to debug) or Protobuf/Avro (smaller/faster/typed). Backward-compatible schema changes only; store in a schema registry with a BACKWARD compatibility policy.
2. **Delivery:** at-least-once + **idempotent handlers**; enforce once-only effects (upsert / ProcessedEvents table / compare-and-set); one ordering key per aggregate (Kafka key, SQS FIFO group, Pub/Sub ordering key).
3. **Reliability:** retries with **exponential backoff + full jitter**; classify transient (retry) vs permanent (no retry); **DLQ** after retry budget exhausted (include failure metadata); safety valves (concurrency limits, circuit breakers, poison-pill detection).
4. **State / Outbox:** write an **outbox row** in the same transaction as the business row; a poller or **CDC (Debezium)** publishes NEW rows and marks them SENT → no lost events, no phantom events.
5. **Observability:** propagate **W3C traceparent** + a business `correlationId`; ship publish/consume rate, end-to-end latency (p50/p95/p99), consumer lag, retry counts, DLQ depth, schema-validation failures; alert on sustained lag, DLQ depth > 0, stuck consumers.
- Best practice: **"dumb pipes, smart endpoints"** — keep middleware dumb; prefer open contracts (HTTP + CloudEvents, or a broker: Kafka/RabbitMQ/SQS/Pub-Sub).

### Service design (tail slides)
- **Service Boundary Checklist / Modeling Services:** worry about what happens *between* services; model around APIs/events, datastore, integration.
- **Integration rules:** avoid breaking changes; keep APIs technology-agnostic; hide internal implementation detail; **avoid a shared database** (exceptions: read-only, strangler-fig).
- **Tailored Service Template:** a default set of decisions (web framework, logging, monitoring, build, packaging, deployment) per stack — lightweight governance that encourages collaborative evolution.

---

## 5. Platform Engineering & Delivery Lifecycle (Lecture 5)

### SDLC
- **Systems Development Life Cycle** = clearly defined, distinct work phases; a **contract between stakeholders and IT** on the process. Purpose: consistency, repeatability, clear deliverables, early issue identification.
- Stages: ID & Assess → Requirements → Design → Coding & Unit Test → Prod rollout.
- **Major milestones:** Concept Commit → Execute Commit → Design Review → Readiness Review → Post-Project Assessment.

### Platform Engineering (FA26 focus) ⭐
- Shift: DevOps asked every team to own its pipeline → at scale = 100 snowflake pipelines → **platform engineering** builds a **paved road** so stream-aligned teams just ship.
- Vocabulary: **IDP** (Internal Developer Platform), **golden path**, **paved road**, **platform-as-product**, **DevEx**.
- **IDP** = self-service portal + service catalog + templates + on-demand environments. Tools: **Backstage** (Spotify→CNCF), Port, Cortex.
- **Golden path:** scaffold → repo → CI/CD → deploy → observability, pre-wired to policy.

### DORA metrics ⭐ likely Q
- Four keys: **Deployment frequency, Lead time for changes, Change-failure rate, Time to restore (MTTR)**.
- Elite vs low: deploy on demand vs monthly; restore in <1 hr vs >1 week.
- Use as **team health signals**, not individual performance (Goodhart's law). **SPACE** framework complements DORA.

### Governance through code
- **Policy-as-code:** OPA/Rego, Kyverno — rules enforced in the pipeline, not review meetings.
- Production-readiness scorecards as code; compliance evidence (SBOMs, provenance, audit trails) generated by the platform.

---

## 6. Enterprise Applications: ERP, SCM, CRM (Lecture 6) ⭐⭐

### Enterprise systems foundations
- **Functional structure:** org divided into departments (sales/marketing, R&D, finance/accounting, HR, IS), each doing closely related activities.
- **Silo effect:** people perform their step in isolation without understanding what comes before/after → hard to coordinate across functions.
- **Enterprise Systems (ES):** support **end-to-end** business processes that span the org and multiple geographies.
- Core business processes: procurement (buy), production (make), fulfillment (sell), lifecycle data mgmt (design), material planning (plan), inventory/warehouse (store), asset mgmt/customer service (service), HCM (people), project mgmt, financial accounting (FI, external), management/controlling accounting (CO, internal).

### Architectures
- **Client-server 3-tier:** Presentation (how you interact) / Application (what it does) / Data (where work is stored).
- **SOA:** extends client-server; integrates apps into composite/mash-up applications. **Four properties of a service:** (1) represents a business activity with a specified outcome, (2) self-contained, (3) a black box to consumers, (4) may consist of other underlying services.

### ERP (Enterprise Resource Planning) ⭐
- Integrates planning, manufacturing, sales/marketing into **one management system**; combines departmental databases into a **single database** accessible to all; **automates** business-process tasks.
- Focus: **intra-company processes** (within the org); integrates functional & cross-functional processes.
- **Components/modules:** Finance (general ledger, A/R, A/P), HR (admin, self-service), Manufacturing & Logistics (production planning, materials mgmt, order entry, warehouse mgmt), plus collaboration, content mgmt, BI, identity mgmt.
- **The Big Three today:** **SAP S/4HANA** (in-memory ERP, RISE with SAP; SAP ECC maintenance ends 2027), **Salesforce** (CRM-grown platform: Data Cloud, Agentforce, AppExchange), **Microsoft Dynamics 365 + Power Platform**. Also Oracle Fusion/NetSuite, Workday.
- **Clean core:** keep vendor core vanilla; extend via platform layers (SAP BTP, Salesforce Platform, Power Platform) using events/APIs/side-by-side apps. Avoids **customization debt** that makes upgrades take years.
- **Why replacing ERP is hard:** data gravity, "the processes ARE the org chart," big-bang vs phased cutover (**Hershey, Lidl** failure cases). Prefer **strangler-fig** over rip-and-replace.

### CRM (Customer Relationship Management) ⭐
- Technology to manage the customer base; match customer needs with offerings; track what customers purchased; a **philosophy** for keeping clients happy & returning (not just software).
- **SFA (Sales Force Automation)** = a primary *component* of CRM.
- Benefits: consolidate customer data in one system, improve productivity, reach more prospects, close more sales.

### SCM (Supply Chain Management)
- Manages flow upstream (suppliers) → downstream (customers). **CRM drives what SCM will produce.**

---

## 7. Business Processes, Messaging & Service Composition (Lecture 7) ⭐⭐⭐

### Part I — Modeling: UML & ER ⭐
- **UML** = Unified Modeling Language; general-purpose modeling language; by **Booch, Jacobson, Rumbaugh** (Rational), adopted by **OMG in 1997**, an ISO standard.
- **Diagram types:** Class, Use Case, Activity, Sequence (and State, etc.).
  - **Class diagram:** classes with name/attributes/operations + relationships. Most-used type.
  - **Use case:** actors + system functions.
  - **Activity:** workflow (sometimes alternative to state machine).
  - **Sequence:** object interactions over time; objects vertical, interactions as arrows; `frame` box = if/loop. Language-agnostic, above code level, good for teams.
- **Multiplicity / cardinality:** 0..1, 1, 0..*, 1..*, 5, m..n ; `*` = unlimited.
- **Association vs Aggregation vs Composition (★ know the difference):**
  - **Association** — general link (arrow). Deleting one may or may not affect the other. (teacher–students)
  - **Aggregation** — "has-a", **weak**; empty diamond; parts can exist **independently**; deleting whole doesn't delete part. (car–wheel)
  - **Composition** — "owns-a", **strong**; filled diamond; parts **cannot exist without** the whole; deleting whole deletes parts. (folder–file)
- **ER diagram:** documents data structure. **Cardinality** = *maximum* times an instance relates to another entity; **Participation** = *minimum* times. **Primary key** (unique identifier) / **Foreign key**.

### Part II — BPMN ⭐
- **BPMN** = Business Process Model and Notation; maintained by **OMG** (since 2005), also **ISO 19510**; latest **BPMN 2.0.2**.
- **Pools & Lanes:** a **lane** is a sub-partition within a pool, typically an organizational **role** (developer, analyst, manager).
- Elements: tasks/activities, **events** (start/intermediate/end), **gateways**, sequence flows.
- **Gateway types (★):**
  - **Exclusive (XOR):** only **one** outgoing path (a decision).
  - **Inclusive (OR):** one *or more* paths based on conditions.
  - **Parallel (AND):** **all** paths taken simultaneously, no conditions.
  - **Complex:** combination of logical operators.
  - **Event-based:** path depends on which event occurs first (e.g., message vs timer).
- **Camunda 8:** process orchestration platform; draw BPMN → **Zeebe engine** runs each instance. Your services are **workers** (subscribe to a task type, receive jobs over gRPC/REST). Camunda 7 CE ended Oct 14 2025.

### Part III — Messaging & Delivery Guarantees ⭐⭐⭐ (very likely exam Qs)
- **Queue vs call:** if the caller needs the answer to continue → **call**; if the work just needs to happen eventually → **queue**. A queue decouples in time and absorbs bursts.
- **Queue vs Topic:**
  - **Queue (point-to-point):** each message → **one** consumer; multiple consumers share the work.
  - **Topic (publish/subscribe):** each message → **every** subscriber; publisher doesn't know who listens.
  - Common pattern: a topic fans out to **one durable queue per consuming service**.
- **Delivery guarantees (★ memorize):**
  - **At most once:** send and forget; messages can be **lost**. (OK for metrics.)
  - **At least once:** redeliver until acked; messages can **arrive twice**. **Default** for SQS standard, SNS, Kafka. *Assume this for every consumer.*
  - **Exactly once:** none lost, none duplicated — a **contract you build**, not a switch. Kafka transactions / SQS FIFO give exactly-once *within their own boundary*.
  - **Rule:** ack **after** the side effect is committed, never before (acking early turns at-least-once into at-most-once).
- **Idempotency:** doing it twice = same effect as once. Each message carries a **key** (order id / UUID); consumer records the key **in the same DB transaction** as the side effect; on redelivery it finds the key and **skips**. "Duplicates are normal traffic, not an incident."
- **Ordering:** guaranteed **within a partition** (Kafka) / message group (SQS FIFO), not across them. A **hot key = hot partition**.
- **Retries & DLQ:** retry with **exponential backoff + jitter** (2s, 4s, 8s + random); after N failures move to a **dead-letter queue (DLQ)**; alert on DLQ depth; fix consumer then **replay** (safe because consumer is idempotent).
- **Dual-write problem & Outbox pattern (★):** writing to a DB *and* a broker in two steps can fail between them. **Outbox:** write the event into an **outbox table in the same local transaction** as the business row (both or neither). A **relay** publishes outbox rows to the broker (polling, or **CDC** / Debezium reading the DB log).
- **Amazon SQS:** a queue. Standard = at-least-once, best-effort ordering, near-unlimited throughput; FIFO = ordered + exactly-once within a message group, lower throughput. Features: **visibility timeout, DLQ, long polling**.

### Part IV — Service Composition: Sagas & Engines ⭐
- **Saga:** one business transaction = **many local transactions**, one per service, each commits on its own. **No rollback across services.** On failure, run **compensating actions** for already-committed steps **in reverse order** (a refund is a *new* transaction, not an undo).
  - Compensation is a **business decision** (cancel? refund minus fee? ship anyway?).
  - Some steps **can't be compensated** (email sent, payment settled) → put them **last** / make them the **pivot step**.
  - **Orchestrated saga** keeps state in the orchestrator; **choreographed saga** keeps state in the events. Either way: needs an **id and a timeout**.
- **Workflow engines (★ place on one map):**
  - **Camunda** — BPMN diagram + human tasks; use when a **person must act** mid-flow.
  - **Temporal** — flow is **ordinary code** (Go/Java/Python/TS/.NET); server records event history and **replays after crash** (workflow can sleep a month and resume). No diagram.
  - **AWS Step Functions** — flow is a **JSON state machine**; serverless, priced per state transition, wired to 200+ AWS services; weak outside AWS.
  - All provide: durable state between steps, retries with policy, visibility into every instance.
  - **No engine** for pure fan-out — don't put a workflow engine in front of a topic.
  - **Stacked retries** warning: 3 layers × 3 retries = 27 attempts; retry at **one** layer, set deadline at the top.
- Patterns source: **Chris Richardson, microservices.io** — Saga, Transactional Outbox, Idempotent Consumer.

---

## 8. Likely exam question types (based on slide cues & "what you should be able to do")
1. **REST:** list/explain the 6 architectural constraints.
2. **SLI vs SLO vs SLA** — define and give an example of each.
3. **Microservices:** definition, benefits, and the "premium"/costs (distribution, eventual consistency, operational complexity).
4. **Delivery guarantees:** at-most / at-least / exactly once — what each forces you to build; why "assume at-least-once."
5. **Idempotency / outbox:** explain the dual-write problem and how the outbox pattern fixes it.
6. **Queue vs Topic** — when to use each.
7. **Orchestration vs Choreography** — define, contrast, pick one for a scenario.
8. **Saga** — design one with compensating actions; why no cross-service rollback.
9. **BPMN gateways** — exclusive vs inclusive vs parallel.
10. **UML relationships** — association vs aggregation vs composition; multiplicity.
11. **ERP/SCM/CRM** — what each is, ERP = intra-company + single database; SFA is part of CRM.
12. **Identity** — SAML vs OAuth vs OIDC (authn vs authz).
13. **Infrastructure** — STP purpose, control plane vs data plane, leaf-spine, QUIC vs TCP.
14. **gRPC/Protobuf** — why it's smaller/faster than JSON/XML; the 4 streaming modes.
15. **DORA metrics** — name the four; platform engineering vocabulary (IDP, golden path).
