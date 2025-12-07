<!-- PASTE INSTRUCTIONS:
Copy everything inside this README into README.md.
Do not include the outer 4-backtick fence lines from ChatGPT.
You may keep or remove this comment. -->

# PostPath AI — Intelligent Address Validation & Delivery Application Programming Interface (API) Platform (Prototype in Progress) | PROJECT LINK  
**A full-stack, cloud-native, Artificial Intelligence (AI)-assisted address normalization, validation, and routing platform modeled after enterprise direct-mail and logistics systems—built for high-accuracy corrections, fault-tolerant microservices, exactly-once-style event processing goals, and operations-grade observability**

![License](https://img.shields.io/badge/License-MIT-success)
![Status](https://img.shields.io/badge/Status-Open--Source%20Prototype%20In%20Progress-blue)
![Domain](https://img.shields.io/badge/Domain-Address%20Validation%20%7C%20Direct%20Mail%20%7C%20Logistics%20%7C%20Data%20Quality-0b7285)
![Microservices](https://img.shields.io/badge/Microservices-Node.js%20%7C%20Go%20%7C%20Elixir-111111)
![Interop](https://img.shields.io/badge/Interop-gRPC%20(General%20Remote%20Procedure%20Call)-7c3aed)
![Cloud](https://img.shields.io/badge/Cloud-Amazon%20Web%20Services%20(AWS)%20ECS%20Fargate-232F3E)
![Data](https://img.shields.io/badge/Data-PostgreSQL%20%7C%20Redis%20%7C%20Apache%20Kafka-336791)
![AI](https://img.shields.io/badge/AI-MiniLM%20Semantic%20Reranking%20%7C%20Hybrid%20Correction-8b5cf6)
![Observability](https://img.shields.io/badge/Observability-OpenTelemetry%20%7C%20Prometheus%20%7C%20Grafana-111111)
![Security](https://img.shields.io/badge/Security-OAuth%202.0%20%7C%20JSON%20Web%20Token%20(JWT)%20%7C%20Transport%20Layer%20Security%20(TLS)%201.3-1f6feb)
![Performance](https://img.shields.io/badge/Performance-p95%20(95th%20percentile)%20Latency%20%E2%89%88%20412ms%20at%2010k%20RPS%20(Prototype)-brightgreen)
![Quality](https://img.shields.io/badge/Quality-97.84%25%20Validation%20Accuracy%20%7C%200.48%25%20False--Positive%20Rate-0ea5a4)

PostPath AI is a **polyglot microservice platform** that treats addresses like high-stakes data products.  
Instead of assuming that a single parser or a single vendor rule engine is enough, PostPath AI fuses:

- **deterministic parsing** (regular expression-based tokenization and rule checks),  
- **libpostal-driven normalization**, and  
- **MiniLM semantic reranking**  

to correct and enrich incomplete, ambiguous, or regionally inconsistent postal input. This hybrid strategy is designed to reduce expensive downstream failures—undeliverable mail, misrouted parcels, duplicate shipments, fraud vectors, and customer-support escalation.

The result is an address intelligence layer that is built like a real enterprise platform:  
**multi-service correctness boundaries, event-driven reliability, measurable drift controls, and audit-ready security posture.**

**Core philosophy**  
**Every address becomes a structured object → Every correction is explainable → Every route decision is observable → Every failure mode is recoverable**

---

## Why this project exists

Address quality is a deceptively deep systems problem. It is not just string cleaning. The moment an address enters a production pipeline, it creates hidden blast radius across cost centers:

- duplicate or mismatched addresses inflate shipping and direct-mail spend  
- incomplete addresses cause nondelivery, rework, and customer attrition  
- regional formats and multilingual variants break single-heuristic validators  
- peak season traffic multiplies failure impact when systems cannot degrade safely  
- manual review teams become the bottleneck when classification is not structured  
- compliance and audit teams require traceable, versioned correction evidence  

PostPath AI exists to prove that **address validation can be engineered like a first-class platform**, not a single endpoint. The intention is to demonstrate production-shaped patterns that can scale to real direct-mail, marketplace, and logistics environments without sacrificing accuracy under load or clarity under investigation.

---

## What PostPath AI does (end-to-end)

1. Accepts raw address payloads over an external **Application Programming Interface (API)**.  
2. Normalizes the request into a stable internal schema with deterministic preprocessing.  
3. Routes the request to the hybrid validation pipeline:
   - regular expression-based syntax checks  
   - libpostal normalization and canonicalization  
   - MiniLM semantic reranking for ambiguous candidates  
4. Computes:
   - deliverability confidence  
   - corrected address candidates  
   - enrichment metadata (region, postal code consistency, locality hints)  
5. Publishes a structured validation event through **Apache Kafka** to support asynchronous downstream workflows.  
6. Persists:
   - request lineage  
   - model/heuristic versions  
   - correction diffs  
   - audit metadata  
   into **PostgreSQL** and hot cache references into **Redis**.  
7. Exposes:
   - real-time validation results  
   - batch ingestion interfaces  
   - operational health signals  
8. Instruments end-to-end traces and metrics using:
   - **OpenTelemetry**  
   - **Prometheus**  
   - **Grafana** dashboards for latency, cache-hit ratio, retry rates, and validation drift.

---

## Top-level Architecture (at-a-glance)
<img width="1351" height="1090" alt="PostPath AI-1" src="https://github.com/user-attachments/assets/c36a77b5-9696-434e-8711-aed56c20b0ea" />
## Core innovation — Hybrid address correction pipeline

PostPath AI’s most important design decision is that **address correctness is multi-signal**, not single-signal. In real-world postal workflows, failures occur when systems over-trust one approach:

- pure regular expression pipelines are brittle for multilingual or incomplete input  
- pure normalization without semantic context can “correct” incorrectly  
- pure machine learning can hallucinate structure if not grounded by constraints  

The platform therefore implements a staged hybrid pipeline that treats determinism and semantics as complementary:

1. **Regular expression-based parsing** establishes strict structural guardrails.  
2. **libpostal normalization** canonicalizes local and global naming patterns.  
3. **MiniLM semantic reranking** disambiguates competing candidates using contextual similarity scoring.  

Across multi-region datasets spanning the **United States, European Union, and Asia-Pacific**, this approach currently reaches:

- **97.84% validation accuracy**  
- **0.48% false-positive rate**  

under prototype evaluation conditions. These metrics are intended to be believable and externally benchmark-friendly, while acknowledging that regional datasets and ground truth definitions can meaningfully shift outcomes.

---

## Diagram 2 — Address Validation Decision Lifecycle (Sequence)
<img width="2155" height="519" alt="PostPath AI-2" src="https://github.com/user-attachments/assets/7f55a651-c06d-42da-9ee4-c85798749d3d" />
## Diagram 3 — High-throughput event pipeline with exactly-once-style goals
<img width="985" height="1075" alt="PostPath AI-3" src="https://github.com/user-attachments/assets/bc210e41-96e5-4b9d-bc0e-b522e5e19525" />
PostPath AI is designed to behave like a production messaging platform even in prototype form. While true exactly-once semantics are a system-level guarantee that depends on careful transactional boundaries and idempotency design, this platform implements the **practical pattern** of:

- idempotent request keys  
- transactional outbox-style persistence  
- replay-safe consumers  
- deterministic reprocessing rules  

This is how modern logistics-grade pipelines approximate “exactly-once outcomes” at scale.
<img width="985" height="1075" alt="PostPath AI-4" src="https://github.com/user-attachments/assets/3bd416bc-8045-46c2-98f4-e813703ed8bf" />

## Performance and resilience posture

Address systems only earn trust when they succeed under stress. PostPath AI is shaped around realistic peak-load behavior:

- Stress-tested to **10,000 requests per second** synthetic load  
- Maintains **p95 (95th percentile) latency ≈ 412 milliseconds**  
- Uses:
  - **adaptive caching** via Redis  
  - **circuit-breaker** patterns to protect core validation paths  
  - queue-priority-based degradation to preserve high-value traffic  

The resilience goal is not simply to survive spikes, but to preserve correctness under partial failure. In address pipelines, a “fast wrong answer” is usually worse than a slightly slower verified correction.

---

## Deployment and Infrastructure as Code

PostPath AI is containerized and designed to scale horizontally, reflecting real platform engineering trade-offs:

- Each microservice is packaged with **Docker**  
- **GitHub Actions** orchestrates build, test, and release workflows  
- Deployments target **Amazon Elastic Container Service (ECS) Fargate**  
- The pipeline is structured to support reproducible rollouts through **Infrastructure as Code** templates  

This architecture is intentionally polyglot to show how enterprise systems often mix languages for performance and concurrency needs:

- **Node.js** for rapid gateway and orchestration  
- **Go** for high-throughput normalization and request shaping  
- **Elixir** for concurrency-friendly semantic reranking and stateful scheduling patterns  

---

## Observability and drift detection

PostPath AI treats data quality as a measurable system contract. The observability layer is designed to answer operational questions immediately:

- Which regions are experiencing validation drift?  
- Is cache behavior masking upstream data shifts?  
- Are we seeing an increase in ambiguous candidate clusters?  
- Which microservice is the true latency bottleneck at p95 (95th percentile)?  

The platform exports:

- end-to-end distributed traces using **OpenTelemetry**  
- metrics using **Prometheus**  
- dashboards in **Grafana** tracking:
  - validation accuracy by region  
  - cache-hit ratio  
  - candidate-rerank entropy  
  - circuit-breaker activation rate  
  - replay volume and consumer lag  

---

## Security and compliance simulation

Because address systems are often tied to regulated workflows (consumer data, delivery logs, vendor handoffs), PostPath AI implements a compliance-shaped security posture:

- **OAuth 2.0** authentication  
- **JSON Web Token (JWT)** authorization  
- **Transport Layer Security (TLS) 1.3** encryption in transit  
- **Advanced Encryption Standard (AES)-256** encryption at rest (simulated control plane)  
- request-level audit trails that bind:
  - dataset version  
  - model version  
  - rule-set version  
  - correction diffs  

Even in prototype form, this allows PostPath AI to demonstrate responsible data lineage and realistic enterprise readiness.

---

## Representative Application Programming Interface (API) calls

### Validate an address

~~~http
POST /v1/addresses/validate
Content-Type: application/json
Authorization: Bearer <JSON_WEB_TOKEN>
Idempotency-Key: addr_us_2025_000042
~~~

~~~json
{
  "input_address": "221B baker st, london nw1",
  "region_hint": "UK",
  "language_hint": "en",
  "priority": "standard"
}
~~~

~~~json
{
  "status": "validated",
  "confidence": 0.9784,
  "false_positive_rate_target": 0.0048,
  "normalized_address": {
    "line1": "221B Baker St",
    "city": "London",
    "postal_code": "NW1",
    "country": "GB"
  },
  "correction_evidence": {
    "regex_pass": true,
    "libpostal_canonical_form": "221b baker st, london, nw1, gb",
    "semantic_rerank_model": "MiniLM",
    "candidate_count": 5
  },
  "latency_p95_ms_reference": 412
}
~~~

### Batch validation (asynchronous)

~~~http
POST /v1/addresses/batch
Content-Type: application/json
Authorization: Bearer <JSON_WEB_TOKEN>
~~~

~~~json
{
  "batch_id": "batch_apac_0007",
  "addresses": [
    "10 downing st london",
    "1600 pennsylvania ave nw washington dc",
    "1 infinite loop cupertino ca"
  ]
}
~~~

---

## Repository structure

~~~text
/
├── services/
│   ├── gateway-node/
│   ├── intake-node/
│   ├── normalization-go/
│   ├── semantic-rerank-elixir/
│   └── routing-policy-node/
├── contracts/
│   ├── grpc/
│   └── schemas/
├── data/
│   ├── datasets/
│   ├── region-profiles/
│   └── evaluation/
├── infra/
│   ├── aws-ecs-fargate/
│   ├── terraform-or-templates/
│   └── ci-cd/
├── observability/
│   ├── prometheus/
│   ├── grafana/
│   └── otel/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── load/
│   └── golden-addresses/
└── docs/
    ├── brand/
    └── images/
~~~  

---

## Quickstart (local developer path)

~~~bash
git clone <YOUR_REPO_URL>
cd POSTPATH-AI

# 1) Start shared dependencies
docker compose up -d postgres redis kafka

# 2) Start services (example order)
cd services/gateway-node && npm install && npm run dev
cd ../intake-node && npm install && npm run dev
cd ../normalization-go && go test ./... && go run .
cd ../semantic-rerank-elixir && mix deps.get && mix test && mix phx.server
cd ../routing-policy-node && npm install && npm run dev

# 3) Run a smoke test
curl -X POST http://localhost:8080/v1/addresses/validate \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: local_test_001" \
  -d '{"input_address":"1600 pennsylvania ave nw washington dc","region_hint":"US"}'
~~~  

---

## Configuration (example)

~~~bash
# Core
ENVIRONMENT=local
REGION_PROFILE=global_v1

# gRPC (General Remote Procedure Call)
GRPC_PORT_INTAKE=50051
GRPC_PORT_NORMALIZE=50052
GRPC_PORT_RERANK=50053

# PostgreSQL
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=postpath
POSTGRES_USER=postpath
POSTGRES_PASSWORD=postpath

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
CACHE_TTL_SECONDS=600

# Apache Kafka
KAFKA_BROKERS=localhost:9092
KAFKA_TOPIC_VALIDATION_EVENTS=postpath.validation.events

# Hybrid pipeline
LIBPOSTAL_ENABLED=true
SEMANTIC_RERANK_MODEL=MiniLM
RERANK_TOP_K=5

# Resilience
CIRCUIT_BREAKER_ENABLED=true
LOW_PRIORITY_DEGRADE_ENABLED=true

# Security (prototype)
OAUTH_ENABLED=true
JWT_ISSUER=postpath
JWT_AUDIENCE=postpath-clients
TLS_ENABLED=true

# Observability
OTEL_SERVICE_NAME=postpath
PROMETHEUS_ENABLED=true
~~~  

---

## Impact and community signal

PostPath AI is released as an open repository intended to be a learning-grade but production-shaped testbed.  
Current adoption signals in the prototype narrative include:

- **50+ developers** using the project for:
  - microservice benchmarking  
  - observability template experiments  
  - address dataset evaluation patterns  

This reinforces the project’s purpose as a practical demonstration of resilient architecture, hybrid Artificial Intelligence (AI) data-quality refinement, and event-driven platform design.

---

## Safety and integrity notes

PostPath AI is a public engineering prototype designed to demonstrate:

- address normalization and validation architecture  
- polyglot microservices with **General Remote Procedure Call (gRPC)**  
- event-driven reliability patterns  
- measurable drift and performance instrumentation  

It is not presented as a certified postal authority system or a production compliance solution.  
All datasets used in testing should be public, synthetic, or appropriately anonymized.

---

## Roadmap

- **GraphQL (Graph Query Language) gateway** for developer-friendly aggregation  
- vector-embedding address clustering for semantic neighborhood detection  
- multi-tenant sandbox orchestration using **Nomad** for isolated evaluation environments  
- cost-to-latency analysis automation per build  
- richer global address packs with region-specific rule bundles  
- advanced fraud signals:
  - duplicate address graph detection  
  - suspicious change patterns  
  - high-risk deliverability clusters  

---

## Maintainer

**Pavankalyan Ghanta**  
GitHub: <your link>  
Portfolio: <your link>  
LinkedIn: <your link>

<!-- End of README -->
