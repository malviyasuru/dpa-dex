# DPA-DEX Agent Architecture — README

**Project:** DPA-DEX · Intelligent Data Exchange Platform  
**Developed by:** C-DAC Mumbai & CDPG IISc Bangalore  
**Compliance:** DPDP Act 2025 | Privacy-Preserving Compute | MCP Protocol Bus

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What Is an Agent?](#2-what-is-an-agent)
3. [Types of Agents in DPA-DEX](#3-types-of-agents-in-dpa-dex)
4. [Architecture Overview](#4-architecture-overview)
5. [Agent Catalogue — Roles & Responsibilities](#5-agent-catalogue--roles--responsibilities)
   - [Plane 2 — Intelligence & Orchestration](#plane-2--intelligence--orchestration)
   - [Specialist Agents Layer](#specialist-agents-layer)
   - [Cross-Cutting Agents](#cross-cutting-agents)
6. [Platform Services (Plane 3 — Governance & Trust)](#6-platform-services-plane-3--governance--trust)
7. [End-to-End Request Flow](#7-end-to-end-request-flow)
8. [Agent Interaction Matrix](#8-agent-interaction-matrix)
9. [MCP Bus Protocol](#9-mcp-bus-protocol)

---

## 1. Introduction

DPA-DEX (Data Partnership & Data Exchange) is an intelligent, privacy-preserving data exchange platform built to enable secure, consent-driven, and policy-compliant data sharing across organisations, sectors, and jurisdictions.

At its core, DPA-DEX uses a **multi-agent architecture** — a coordinated system of 12 autonomous AI agents operating across 5 architectural planes. Every data access request, from the moment a user submits a natural language objective to the moment results are returned, is governed by this agent network. No data moves without consent. No computation runs without a policy signal. No decision is made without an audit trail.

The platform is fully compliant with the **DPDP Act 2025** (India's Digital Personal Data Protection Act) and supports privacy-enhancing compute technologies including Trusted Execution Environments (TEE), Federated Learning (FL), Homomorphic Encryption (HE), and Secure Multi-Party Computation (MPC).

**Key figures at a glance:**

| Metric | Value |
|---|---|
| Autonomous Agents | 12 |
| Architecture Planes | 5 |
| PET Compute Modes | 4 |
| Agent Communication Bus | 1 (MCP) |
| Data Provider Types | 6 |
| Actor / User Types | 7 |

---

## 2. What Is an Agent?

In the context of DPA-DEX, an **agent** is an autonomous software entity with a well-defined role, its own decision logic, and the ability to communicate with other agents exclusively through the MCP (Multi-agent Communication Protocol) bus.

Each agent:

- **Owns a specific domain** — e.g., consent, access control, data discovery, computation.
- **Acts autonomously** — it makes decisions within its scope without requiring human intervention for routine tasks.
- **Communicates via message passing** — all inter-agent calls are mediated by the MCP bus; direct agent-to-agent calls are prohibited.
- **Emits audit events** — every significant decision is logged to the Audit Agent for compliance and traceability.
- **Can block a request** — if a constraint is violated (consent denied, policy breached, access refused), the agent issues a BLOCK signal that halts the request pipeline immediately.

Agents are distinct from **platform services** (such as Catalogue Service, Consent Service), which are infrastructure components that agents invoke rather than autonomous decision-makers.

---

## 3. Types of Agents in DPA-DEX

DPA-DEX agents fall into three broad categories based on their operational model:

### 3.1 Orchestration Agents
These agents manage the high-level reasoning, task decomposition, execution, lifecycle, and memory of the platform. They form the cognitive core of DPA-DEX and operate within **Plane 2 — Intelligence & Orchestration**.

| Agent | ID | Role Summary |
|---|---|---|
| Planner Agent | AGENT-01 | Decomposes user goals into task graphs; re-plans on failure |
| Executor Agent | AGENT-02 | Translates task graphs into concrete agent/tool invocations |
| Agents Lifecycle Manager | AGENT-03 | Manages registration, health, versioning, and scaling of all agents |
| Memory & Context Agent | AGENT-04 | Maintains session state, long-term knowledge, and RAG context |

### 3.2 Specialist Domain Agents
These agents each own a specific operational domain — data discovery, consent, access control, policy enforcement, computation, and data harmonisation. They form the **Specialist Agents Layer** and communicate exclusively via the MCP bus.

| Agent | ID | Role Summary |
|---|---|---|
| Discovery Agent | AGENT-05 | Finds matching datasets via semantic search over catalogue |
| Consent Reasoning Agent | AGENT-06 | Validates and issues consent artefacts per DPDP Act 2025 |
| Access Intelligence Agent | AGENT-07 | Issues scoped, time-bound, purpose-bound access tokens |
| Policy Governance Agent | AGENT-08 | Enforces sectoral policies and regulatory rules |
| Compute Agent | AGENT-09 | Provisions and orchestrates privacy-preserving compute jobs |
| Schema Harmonization Agent | AGENT-10 | Resolves format heterogeneity; applies data minimisation |

### 3.3 Cross-Cutting Agents
These agents run continuously across all planes. They are not invoked per-request but operate as always-on system-level agents providing observability, auditability, and responsible AI oversight.

| Agent | ID | Role Summary |
|---|---|---|
| Observability Agent | AGENT-11 | Collects telemetry; monitors SLAs; fires operational alerts |
| Audit & Transparency Agent | AGENT-12 | Writes immutable audit logs; generates compliance reports |
| AI Safety & Ethics Agent | AGENT-13 | Monitors for bias, fairness, and explainability; triggers HITL |

> **Note:** The HTML source references 12 autonomous agents. AGENT-13 (AI Safety & Ethics) is a cross-cutting agent that completes the full complement.

---

## 4. Architecture Overview

DPA-DEX is structured across **5 architecture planes**, each representing a logical layer of the platform. Agents operate within or across these planes.

```
┌─────────────────────────────────────────────────────────────┐
│  PLANE 1 — Engagement Plane                                 │
│  User interface layer; 7 actor types interact here          │
│  (Researcher, Analyst, Regulator, Data Principal, etc.)     │
├─────────────────────────────────────────────────────────────┤
│  PLANE 2 — Intelligence & Orchestration Plane               │
│  Cognitive core: Planner, Executor, Lifecycle, Memory       │
├─────────────────────────────────────────────────────────────┤
│  SPECIALIST AGENTS LAYER                                    │
│  Discovery · Consent · Access Intelligence                  │
│  Policy Governance · Compute · Schema Harmonization         │
├─────────────────────────────────────────────────────────────┤
│  PLANE 3 — Governance & Trust Plane                         │
│  Platform Services: Catalogue · Consent Service             │
│  Access Control · Resource Service · Secure Compute         │
├─────────────────────────────────────────────────────────────┤
│  PLANE 4 — Data Exchange Plane                              │
│  Data ingestion, transformation, and delivery               │
├─────────────────────────────────────────────────────────────┤
│  PLANE 5 — Data Provider Plane                              │
│  Registered Data Provider nodes (6 provider types)         │
├─────────────────────────────────────────────────────────────┤
│  CROSS-CUTTING (all planes)                                 │
│  Observability · Audit & Transparency · AI Safety & Ethics  │
└─────────────────────────────────────────────────────────────┘
                     ↕ All communications via MCP Bus ↕
```

**Key architectural principles:**
- All agent-to-agent communication is mediated exclusively by the **MCP bus**. Direct calls are prohibited.
- Every message on the bus is signed with the sending agent's private key and logged in the Audit Agent.
- A BLOCK at any stage immediately aborts the request pipeline and notifies the Planner.
- All 4 PET compute modes (TEE, FL, HE, MPC) are available; the Compute Agent selects the appropriate mode based on task sensitivity.

---

## 5. Agent Catalogue — Roles & Responsibilities

---

### Plane 2 — Intelligence & Orchestration

#### 🧠 AGENT-01 — Planner Agent  
*Alias: "The Thinker"*

The Planner is the cognitive entry point of the platform. It receives natural language objectives from users via the Engagement Plane and is responsible for translating them into a structured execution plan.

**Responsibilities:**
- Receives natural language objectives from users via the Engagement Plane
- Decomposes goals into structured task graphs using ReAct / Chain-of-Thought reasoning
- Selects appropriate downstream agents and determines sequencing strategy
- Maintains reasoning state; re-plans on partial failure or policy block
- Issues Work Orders to the Executor Agent via the MCP bus
- Collects Results/Insights from Executor and synthesises the final response

**Capabilities:** ReAct Loop · Task Graph · Chain-of-Thought · Goal Decomposition · Re-planning

**Interacts with:**
- → Executor Agent — sends Work Orders (MCP)
- → Memory & Context Agent — reads/writes session state
- ← Executor Agent — receives Results/Insights
- ← Engagement Plane — receives user objective
- → Agents Lifecycle Manager — reports health/status

---

#### ⚙️ AGENT-02 — Executor Agent  
*Alias: "The Doer"*

The Executor is the platform's operational engine. It takes structured Work Orders from the Planner and translates them into concrete invocations of downstream specialist agents and tools.

**Responsibilities:**
- Receives structured Work Orders from Planner via the MCP bus
- Resolves Work Orders into concrete tool/agent invocations from the agent registry
- Invokes Discovery, Consent, Access Intelligence, Policy Governance, and Compute agents as required
- Handles partial results, retries, and timeout management
- Packages and returns Results/Insights to Planner via MCP
- Streams intermediate outputs for long-running compute tasks

**Capabilities:** Tool Registry · MCP Client · Retry Logic · Streaming · Parallel Dispatch

**Interacts with:**
- ← Planner Agent — receives Work Orders
- → Discovery Agent — triggers dataset search
- → Consent Reasoning Agent — requests consent check
- → Access Intelligence Agent — requests access token
- → Compute Agent — triggers PET computation
- → Planner Agent — returns Results/Insights

---

#### 🔄 AGENT-03 — Agents Lifecycle Manager  
*Alias: "Manage & Evolve"*

The Lifecycle Manager is the operational backbone of the agent fleet. It ensures all agents are healthy, correctly versioned, and appropriately scaled.

**Responsibilities:**
- Manages agent registration, versioning, and deployment across the platform
- Performs health monitoring and automatic restart of failed agents
- Controls A/B routing and canary deployments for agent upgrades
- Maintains the Agent Registry (service discovery for all 12 agents)
- Enforces auto-scaling policies based on load metrics from Observability Agent

**Capabilities:** Health Checks · Agent Registry · Canary Deploy · Auto-scaling · Versioning

**Interacts with:**
- → All Agents — health pings and restart commands
- ← Observability Agent — receives load/error metrics
- → MCP Bus — updates agent endpoint registry

---

#### 🗄️ AGENT-04 — Memory & Context Agent  
*Alias: "Context Keeper"*

The Memory Agent provides the platform's cognitive continuity. It stores session state and historical knowledge so the Planner can operate with full context across multi-turn interactions.

**Responsibilities:**
- Maintains short-term session state (Redis KV store) per user conversation
- Maintains long-term knowledge base via vector database (Qdrant/Weaviate)
- Stores successful data mappings and consent patterns for orchestration efficiency
- Provides RAG (Retrieval-Augmented Generation) context to the Planner Agent
- Handles memory eviction, TTL policies, and DPDP-compliant data retention rules

**Capabilities:** RAG · Vector DB · KV Store · TTL Policies · Semantic Search

**Interacts with:**
- ↔ Planner Agent — reads/writes reasoning context (bidirectional)
- ← Consent Reasoning Agent — stores consent patterns
- ← Audit Agent — receives provenance records

---

### Specialist Agents Layer

#### 🔍 AGENT-05 — Discovery Agent  
*Alias: "Discover & Search"*

The Discovery Agent is the platform's data finder. It performs semantic search over the Catalogue Service to identify datasets that satisfy the Executor's task requirements.

**Responsibilities:**
- Queries the Catalogue Service for datasets matching task requirements
- Performs semantic search over DCAT/OGC-aligned metadata registry
- Resolves dataset provenance, lineage, and freshness metadata
- Automatically triggers downstream consent and access workflows on a match
- Returns ranked dataset candidates with access requirements to the Executor

**Capabilities:** Semantic Search · DCAT/OGC · Provenance · Auto-trigger

**Interacts with:**
- ← Executor Agent — receives search task
- → Catalogue Service (Plane 3) — queries metadata registry
- → Consent Reasoning Agent — triggers consent workflow
- → Executor Agent — returns dataset candidates

---

#### 🔐 AGENT-06 — Consent Reasoning Agent  
*Alias: "Justify & Validate"*

The Consent Reasoning Agent is the platform's consent authority. No data access proceeds without a valid consent artefact issued or confirmed by this agent. It is the primary enforcer of DPDP Act 2025 consent requirements.

**Responsibilities:**
- Generates context-aware consent justifications (purpose, computation intent, confidentiality)
- Verifies provider–requestor compatibility against registered consent artefacts
- Issues machine-readable consent receipts compliant with DPDP Act 2025
- Manages the full consent lifecycle: issue, renew, revoke, expire
- Invokes Consent Service (Plane 3) for DI/API consent record operations
- **BLOCK:** blocks data access and notifies Planner if consent cannot be obtained

**Capabilities:** DPDP Act 2025 · Consent Artefact · Purpose Binding · Revocation · OPA/Cedar

**Interacts with:**
- ← Discovery Agent — triggered on dataset match
- ← Executor Agent — receives consent check request
- → Consent Service (Plane 3) — reads/writes consent records
- → Policy Governance Agent — forwards for policy validation
- → Memory Agent — stores consent patterns
- → Audit Agent — logs consent decisions

---

#### 🛡️ AGENT-07 — Access Intelligence Agent  
*Alias: "Enforce & Resolve"*

The Access Intelligence Agent is the gatekeeper for data access. It evaluates the full context of a request and, only upon satisfying all conditions, issues a scoped, time-limited access token to the Data Exchange Plane.

**Responsibilities:**
- Evaluates task context, data sensitivity, and multi-party access policies
- Issues scoped, purpose-bound, time-limited access tokens (JWT / Verifiable Credentials)
- Enforces ABAC/RBAC rules via Access Control Service (Plane 3)
- Automatically revokes tokens on consent withdrawal or policy violation
- Implements context-aware reasoning (geo-fence, time-bound, role, purpose)
- **BLOCK:** blocks access if any policy check fails

**Capabilities:** ABAC/RBAC · JWT/VC · Geo-fence · Time-bound · Auto-revoke

**Interacts with:**
- ← Executor Agent — receives access request
- ← Consent Reasoning Agent — receives validated consent
- → Access Control Service (Plane 3) — evaluates ABAC/RBAC rules
- → Data Exchange Plane (4) — issues access token
- → Audit Agent — logs access decisions

---

#### 📋 AGENT-08 — Policy Governance Agent  
*Alias: "Validate & Enforce"*

The Policy Governance Agent is the platform's regulatory enforcement layer. It validates every data request against the applicable sectoral policy rules defined under the DPDP Act 2025.

**Responsibilities:**
- Validates data requests against participant policies and regulatory requirements
- Enforces DPDP Act 2025 sectoral rules (health, finance, agri, telecom)
- Supports dynamic policy updates without requiring a platform restart
- Runs a policy simulation sandbox for prospective access scenarios
- Propagates Policy Enforcement & Trust Signals to the Data Exchange Plane
- Generates policy compliance reports for regulators and auditors
- **BLOCK:** blocks requests if any policy rule is violated

**Capabilities:** Policy Engine · OPA/Cedar · DPDP Rules · Dynamic Updates · Simulation

**Interacts with:**
- ← Consent Reasoning Agent — receives consent for validation
- → Data Exchange Plane (4) — sends Policy Enforcement Signals
- → Audit Agent — logs policy decisions
- → AI Safety Agent — forwards AI-specific policy checks

---

#### ⚡ AGENT-09 — Compute Agent  
*Alias: "Provision & Negotiate"*

The Compute Agent orchestrates all privacy-preserving computations. It selects the appropriate PET compute mode and provisions the secure environment in which data is processed — ensuring raw data is never exposed.

**Responsibilities:**
- Dynamically selects PET compute mode (TEE, MPC, FL, or HE) based on task requirements
- Provisions secure enclave sessions (Intel TDX / NVIDIA Confidential Computing)
- Orchestrates Federated Learning jobs across distributed data provider nodes
- Manages Homomorphic Encryption key distribution and computation coordination
- Monitors compute resource usage; returns results via Secure Compute Service (Plane 3)
- Negotiates compute SLAs and timeout constraints per job

**PET Compute Modes:**

| Mode | Technology | Use Case |
|---|---|---|
| TEE | Intel TDX / NVIDIA CC | Isolated single-party execution |
| FL | PySyft / FATE / KubeRay | Distributed multi-node training |
| HE | SEAL / OpenFHE | Computation on encrypted data |
| MPC | MP-SPDZ | Secure multi-party analysis |

**Interacts with:**
- ← Executor Agent — receives compute task
- → Secure Compute Service (Plane 3) — provisions TEE/enclave
- → Data Provider Nodes (Plane 5) — dispatches FL training jobs
- → Data Exchange Plane (4) — returns computation results
- → Audit Agent — logs compute provenance

---

#### 🔧 AGENT-10 — Schema Harmonization Agent  
*Alias: "Unify & Transform"*

The Schema Harmonization Agent resolves the data format heterogeneity inherent in cross-sector data exchange. It sits within the Data Exchange Plane and prepares data for consumption before it reaches the compute layer.

**Responsibilities:**
- Automatically resolves heterogeneous data formats (JSON, XML, OGC, FHIR, CSV, Parquet)
- Translates between domain standards (FHIR for health, FIX for finance, ISO 19115 for geo)
- Applies filtering, masking, aggregation, and synthetic data generation
- Minimises unnecessary data exposure per the data minimisation principle
- Operates within the Data Exchange Plane before data reaches the compute layer

**Capabilities:** FHIR/ISO · Data Masking · Aggregation · Synthetic Data · Format Transform

**Interacts with:**
- ← Data Exchange Plane (4) — receives raw ingested data
- → Compute Agent — passes harmonised dataset
- → Access Intelligence Agent — consults masking policy

---

### Cross-Cutting Agents

These agents run continuously across all planes and are not invoked per-request. They provide always-on system-level capabilities.

#### 📊 AGENT-11 — Observability Agent  
*Alias: "Monitoring & Alerts"*

**Responsibilities:**
- Collects metrics from all agents and services via OpenTelemetry instrumentation
- Tracks performance metrics, data quality scores, and SLA adherence
- Fires alerts to the Lifecycle Manager when thresholds are breached
- Feeds Prometheus/Grafana dashboards for operations team visibility

**Interacts with:**
- ← All Agents & Services — receives telemetry metrics
- → Agents Lifecycle Manager — sends health/load alerts

---

#### 📜 AGENT-12 — Audit & Transparency Agent  
*Alias: "Immutable Logs"*

**Responsibilities:**
- Receives audit events from all agents (consent, access, policy, and compute decisions)
- Writes to an immutable, append-only audit ledger (IPFS-anchored or blockchain-notarised)
- Captures agent reasoning traces for explainability and compliance
- Generates regulatory compliance reports on demand
- Exposes an audit query API for Regulator/Auditor actors via the Engagement Plane

**Interacts with:**
- ← Consent, Access, Policy, Compute Agents — receives all decision events
- → Engagement Plane — exposes audit query API
- → Memory Agent — stores provenance records

---

#### ⚖️ AGENT-13 — AI Safety & Ethics Agent  
*Alias: "Responsible AI"*

**Responsibilities:**
- Monitors Planner and Executor agent decisions for bias, fairness, and explainability
- Runs SHAP/LIME post-hoc explainability analysis on model outputs from the Compute Plane
- Triggers Human-in-the-Loop (HITL) escalation when confidence is low or risk is high
- Enforces Responsible AI guidelines across all AI-generated recommendations
- Works with the Policy Governance Agent on AI-specific regulatory compliance

**Interacts with:**
- ← Planner Agent — monitors reasoning steps
- ← Compute Agent — receives model outputs for audit
- → Policy Governance Agent — shares AI risk signals
- → Engagement Plane — triggers HITL escalation

---

## 6. Platform Services (Plane 3 — Governance & Trust)

Plane 3 hosts infrastructure services that agents invoke. These are not autonomous agents but managed platform components essential to the governance layer.

| Service | ID | Description |
|---|---|---|
| **Catalogue Service** | SVC-01 | DCAT/OGC-compliant metadata registry; exposes semantic search API to Discovery Agent; stores dataset provenance, lineage, access requirements, and freshness. |
| **Consent Service** | SVC-02 | Stores and manages machine-readable consent artefacts (DPDP Act 2025 compliant); exposes read/write/revoke API to Consent Reasoning Agent; maintains consent audit log per data principal. |
| **Access Control Service** | SVC-03 | Hosts and evaluates ABAC/RBAC policy rules (OPA/Cedar engine); issues and validates scoped JWT / Verifiable Credential tokens; exposes token revocation endpoint. |
| **Resource Service** | SVC-04 | Hosts policy-aware connectors to registered Data Provider nodes; manages API Gateway Models and data ingestion adapters; enforces rate limiting, DLP, and data flow policies. |
| **Secure Compute Service** | SVC-05 | Provisions and attests TEE sessions; manages cryptographic key distribution for HE operations; returns computation results to the Compute Agent after enclave execution. |

---

## 7. End-to-End Request Flow

A complete data access request traverses 14 ordered steps across all planes. A BLOCK at any step immediately aborts the request and notifies the Planner Agent.

| Step | Actor / Agent | Action | Output | Protocol |
|---|---|---|---|---|
| 01 | User / Actor (Engagement Plane) | Submits high-level objective via secure channel; authenticated via OAuth 2.0 / OIDC / Verifiable Credential | Authenticated Request | HTTPS + mTLS |
| 02 | Planner Agent (AGENT-01) | Reads session context from Memory Agent; decomposes goal into task graph using ReAct/CoT; emits Work Order to Executor via MCP bus | Work Order (MCP) | MCP Protocol |
| 03 | Memory & Context Agent (AGENT-04) | Returns relevant session state, previous data mappings, and consent patterns to Planner; updates short-term session store | Context Package | Redis / Vector DB |
| 04 | Executor Agent (AGENT-02) | Receives Work Order; resolves to tool invocations; dispatches to Discovery Agent | Discovery Request | MCP Protocol |
| 05 | Discovery Agent (AGENT-05) | Queries Catalogue Service with semantic search; returns ranked dataset candidates with metadata; triggers consent workflow | Dataset Candidates | DCAT/OGC REST |
| 06 | Consent Reasoning Agent (AGENT-06) | Generates consent justification; verifies compatibility; checks Consent Service for existing artefacts; requests Data Principal consent if absent. **BLOCK if consent denied.** | Consent Artefact / BLOCK | DI/API + DPDP |
| 07 | Policy Governance Agent (AGENT-08) | Validates consent artefact against DPDP Act 2025 sectoral policies; issues Policy Enforcement & Trust Signal. **BLOCK if policy violated.** | Policy Trust Signal / BLOCK | OPA/Cedar Rules |
| 08 | Access Intelligence Agent (AGENT-07) | Evaluates task context, data sensitivity, actor attributes, and constraints; issues scoped, purpose-bound access token. **BLOCK if check fails.** | Access Token / BLOCK | JWT / W3C VC |
| 09 | Data Exchange Plane (Plane 4) | Access token verified at Resource Server/API Gateway; data ingested from Data Provider via connector; passed to Schema Harmonization Agent | Raw Data Stream | Secure APIs |
| 10 | Schema Harmonization Agent (AGENT-10) | Resolves format heterogeneity; applies data masking, aggregation, and filtering per data minimisation policy | Harmonised Dataset | Internal Bus |
| 11 | Compute Agent (AGENT-09) | Selects PET compute mode (FL/TEE/HE/MPC); provisions compute environment; dispatches jobs; waits for results within SLA window | PET Compute Job | KubeRay / TEE API |
| 12 | AI Safety & Ethics Agent (AGENT-13) | Runs SHAP/LIME explainability; checks for bias; triggers HITL escalation if confidence is low or risk flag raised; otherwise clears result | Cleared Result / HITL | SHAP/LIME API |
| 13 | Audit & Transparency Agent (AGENT-12) | Continuously receives audit events from all steps; writes immutable log entries; makes audit trail available to regulators | Immutable Audit Log | Append-only Ledger |
| 14 | Executor → Planner → User | Executor packages results; Planner synthesises final response with reasoning explanation; Memory Agent updated; response delivered to user | Final Response | MCP + HTTPS |

---

## 8. Agent Interaction Matrix

The table below shows which agents communicate with which across the MCP bus. All communications are logged by the Audit Agent.

**Legend:** ▲ = Sends message to · ◆ = Bidirectional (read/write) · · = No direct interaction

| FROM ↓ / TO → | Planner | Executor | Lifecycle | Memory | Discovery | Consent | Access Intel | Policy Gov | Compute | Schema Harm | Observability | Audit | AI Safety |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Planner (01)** | — | ▲ | ▲ | ◆ | · | · | · | · | · | · | · | ▲ | · |
| **Executor (02)** | ▲ | — | · | · | ▲ | ▲ | ▲ | · | ▲ | · | · | ▲ | · |
| **Lifecycle (03)** | · | ▲ | — | ▲ | ▲ | ▲ | ▲ | ▲ | ▲ | ▲ | · | ▲ | ▲ |
| **Memory (04)** | ▲ | · | · | — | · | · | · | · | · | · | · | · | · |
| **Discovery (05)** | · | ▲ | · | · | — | ▲ | · | · | · | · | · | ▲ | · |
| **Consent (06)** | · | ▲ | · | ▲ | · | — | · | ▲ | · | · | · | ▲ | · |
| **Access Intel (07)** | · | ▲ | · | · | · | · | — | · | · | · | · | ▲ | · |
| **Policy Gov (08)** | · | ▲ | · | · | · | · | · | — | · | · | · | ▲ | ▲ |
| **Compute (09)** | · | ▲ | · | · | · | · | · | · | — | · | · | ▲ | ▲ |
| **Schema Harm (10)** | · | · | · | · | · | · | ▲ | · | ▲ | — | · | ▲ | · |
| **Observability (11)** | · | · | ▲ | · | · | · | · | · | · | · | — | · | · |
| **Audit (12)** | · | · | · | ▲ | · | · | · | · | · | · | · | — | · |
| **AI Safety (13)** | · | · | · | · | · | · | · | ▲ | · | · | · | ▲ | — |

> **Note:** All agent communications are mediated by the MCP bus. The Audit Agent receives events from all agents (not shown per row to avoid clutter). The Lifecycle Manager monitors all agents.

---

## 9. MCP Bus Protocol

The **MCP (Multi-agent Communication Protocol) Bus** is the single inter-agent communication backbone of DPA-DEX. All 12 agents communicate exclusively through this bus — direct agent-to-agent calls are prohibited. This ensures auditability, decoupling, and the ability to intercept, rate-limit, or block any message flow for compliance purposes.

### Message Types

| Message Type | From | To | Description | Priority |
|---|---|---|---|---|
| WORK_ORDER | Planner (01) | Executor (02) | Structured task graph: task_id, objective, sub-tasks, tool_refs, constraints, timeout_ms, session_token | HIGH |
| RESULT | Executor (02) | Planner (01) | task_id, status, result_payload, reasoning_trace, data_lineage_ref, audit_ref | HIGH |
| DISCOVERY_REQ | Executor (02) | Discovery (05) | semantic_query, domain_filters, sensitivity_threshold, freshness_requirement, requestor_id | MEDIUM |
| DISCOVERY_RESP | Discovery (05) | Executor (02) | dataset_candidates[]: {dataset_id, provider_id, metadata_ref, access_requirements, consent_status} | MEDIUM |
| CONSENT_CHECK | Executor (02) / Discovery (05) | Consent (06) | dataset_id, requestor_id, purpose, computation_intent, data_principal_id, session_token | CRITICAL |
| CONSENT_RESULT | Consent (06) | Executor (02) | consent_artefact or BLOCK; artefact includes consent_id, expiry, purpose_binding, revocation_endpoint | CRITICAL |
| POLICY_VALIDATE | Consent (06) | Policy Gov (08) | consent_artefact, dataset_id, computation_type, actor_role, jurisdiction, sector_code | CRITICAL |
| POLICY_SIGNAL | Policy Gov (08) | Executor (02) / Data Plane | ALLOW or BLOCK: policy_id, rule_ref, expiry; ALLOW includes enforcement_conditions, audit_ref | CRITICAL |
| ACCESS_REQ | Executor (02) | Access Intel (07) | dataset_id, consent_artefact_id, policy_signal_id, requestor_context{role, geo, timestamp, purpose} | CRITICAL |
| ACCESS_TOKEN | Access Intel (07) | Executor (02) / Data Plane | scoped_jwt or vc_credential: {scope, expiry, purpose_binding, revocation_uri} or BLOCK | CRITICAL |
| COMPUTE_REQ | Executor (02) | Compute (09) | compute_mode{TEE/FL/HE/MPC}, dataset_ref, access_token, algorithm_ref, output_schema, sla_ms | MEDIUM |
| COMPUTE_RESULT | Compute (09) | Executor (02) | result_payload, compute_provenance_ref, enclave_attestation, model_outputs, data_lineage | MEDIUM |
| AUDIT_EVENT | All Agents | Audit (12) | event_type, agent_id, timestamp, payload_hash, decision{ALLOW/BLOCK/HITL}, reasoning_trace_ref | ASYNC |
| HEALTH_PING | Lifecycle (03) | All Agents | agent_id, check_type{liveness/readiness}, timeout_ms | LOW |
| METRICS_PUSH | All Agents | Observability (11) | agent_id, metric_type, value, timestamp, labels{plane, version, instance} | ASYNC |
| SAFETY_CHECK | AI Safety (13) | Policy Gov (08) | model_output_ref, bias_score, explainability_ref, risk_level{LOW/MED/HIGH}, hitl_required: bool | MEDIUM |

### Bus Architecture Properties

**Transport**
- Kafka / NATS for async event streaming
- gRPC for synchronous request-response
- mTLS on all inter-agent channels

**Delivery Guarantees**
- Exactly-once delivery for CRITICAL messages
- At-least-once for ASYNC audit events
- Message schema versioned (protobuf)

**Security**
- All messages signed with the sending agent's private key
- Payload hashes stored in the Audit Agent
- Dead-letter queue for compliance review

**Observability**
- OpenTelemetry trace propagation across all message hops
- Distributed trace ID in every message header
- Prometheus metrics per message type / agent pair

---

*End of DPA-DEX Agent Architecture README*  
*Document generated from the DPA-DEX Agent Architecture Map — C-DAC Mumbai & CDPG IISc Bangalore*
