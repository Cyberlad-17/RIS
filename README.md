# RIS — Reliability Intelligence System

> Agentic AI + RAG-based platform for automated alert triage, runbook-driven remediation, and self-healing infrastructure across cloud-native Kubernetes environments.

---

## Overview

RIS (Reliability Intelligence System) is an intelligent incident automation platform built at **Comcast India Engineering Center**. It eliminates manual on-call toil by combining a multi-agent AI orchestration framework with a RAG-based runbook search engine — enabling autonomous resolution of recurring Kubernetes failures while keeping humans in control of high-risk operations through a Slack-gated approval workflow.

---

## Problem Statement

Modern cloud-native environments generate thousands of alerts. Without intelligent triage and automated remediation:

- On-call engineers spend hours diagnosing incidents manually
- Alert storms cause fatigue and missed critical signals
- Repetitive failures get resolved the same way every time — by hand
- Risky remediation steps run without review or audit trail

RIS solves all four.

---

## Architecture

```
Alerting Tool (Prometheus / Grafana)
        │
        ▼
 Webhook Ingestion
 [Critical-only filter]
        │
        ▼
   Redis Layer
 [Rate control + Deduplication]
        │
        ▼
  LLM Enrichment
 [Glean LLM API + Live cluster context]
        │
        ▼
 Semantic Runbook Search
 [YAML / Markdown / Database]
        │
        ▼
  ┌─────────────────────────────┐
  │     Dual-Path Resolution    │
  ├──────────────┬──────────────┤
  │ Autonomous   │  Approval-   │
  │ Safe Path    │  Gated Path  │
  │              │              │
  │ Pod restarts │ DB restarts  │
  │ Cache flush  │ Node drain   │
  │ K8s jobs     │ Scale change │
  └──────────────┴──────────────┘
        │
        ▼
 Slack Traceability + Audit Trail
```

---

## How It Works

### 1. Webhook Ingestion
Alerting tools send event payloads to the RIS backend. A critical-only filter eliminates low-priority noise before any processing begins — ensuring LLM and remediation resources are spent only on alerts with real reliability impact.

### 2. Redis Rate Control
Redis applies rate limiting and deduplicates burst alerts before passing context downstream — preventing alert storms from triggering multiple redundant remediation flows simultaneously.

### 3. LLM Context Enrichment
The Glean LLM API ingests the alert payload alongside live Kubernetes cluster state — pod health, recent deployments, resource metrics — and produces a structured incident summary enriched with operational context.

### 4. Semantic Runbook Search
RIS performs a semantic search across runbooks stored as YAML, Markdown, and structured database entries — matching the enriched incident context to the most relevant response guide automatically.

### 5. Dual-Path Resolution Engine

**Autonomous Safe Path**
- Executes non-destructive remediation without human intervention
- Actions: pod restarts, cache flushes, Kubernetes job triggers, Ansible playbook runs
- Logs every action to Slack and the backend datastore with a full execution trace

**Approval-Gated Path**
- Flags operations with high blast radius before any execution
- Pushes a structured decision request to Slack with full alert and runbook context
- Waits for explicit on-call engineer approval before proceeding
- Records the complete decision trail — who approved, when, and what ran

---

## Key Capabilities

| Capability | Description |
|---|---|
| Critical-only processing | Filters high-priority alerts with real reliability impact |
| Runbook mapping | Semantic match to YAML, Markdown, or DB-backed response guides |
| Safe auto-remediation | Autonomous execution of non-destructive operations |
| Slack traceability | Full action logs, approval requests, and outcomes in Slack |
| Controlled escalation | Human-in-the-loop gate for risky operations |
| Audit trail | Every decision logged with actor, timestamp, and outcome |

---

## Tech Stack

| Category | Technologies |
|---|---|
| Language | Python |
| AI / LLM | Glean LLM API, RAG Pipeline |
| Message Queue | Redis |
| Container Orchestration | Kubernetes, Amazon EKS |
| CI/CD | Jenkins CI, ArgoCD |
| Notification & Approval | Slack API |
| Automation | Ansible Playbooks |
| Observability | Prometheus, Grafana |
| Runbook Format | YAML, Markdown, Structured DB |

---

## Operational Impact

- **35% reduction** in Mean-Time-To-Resolution (MTTR) across P1/P2 production incidents
- **30% improvement** in average incident turnaround time
- **Zero SLA breaches** across all incidents processed through the RIS pipeline
- **100% audit coverage** — every remediation action logged with full execution trace

---

## Project Page

View the full project documentation at the live project site.

---

## Built At

**Comcast India Engineering Center**
2025 – 2026

---

## Author

**Sanjeev Nagarajan**
DevOps Engineer | AWS Certified Solutions Architect – Associate
[LinkedIn](https://www.linkedin.com/in/sanjeevnagarajan)
