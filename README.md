# AzureBot — Enterprise RAG Engineering Platform

AzureBot is an enterprise-oriented Retrieval-Augmented Generation (RAG) platform built on Azure. It combines Microsoft Entra identity, Azure AI Search, Azure OpenAI, document ingestion, authorization-aware retrieval, citation-first generation, evaluation, and observability.

> **Portfolio disclosure:** this repository started from Microsoft's `azure-search-openai-demo`. The upstream license and attribution are retained. The portfolio work focuses on the enterprise RAG controls, evaluation methodology, security hardening, and architecture documented in this repository.

## What I built

The project is designed around a simple enterprise requirement: **an AI assistant must answer from authorized evidence, not from whatever the model happens to know.**

### RAG pipeline

```text
Documents
   |
   v
Blob / document processing
   |
   v
Chunking + enrichment + embeddings
   |
   v
Azure AI Search
   |  hybrid/vector/semantic retrieval
   v
Authorization + evidence policy
   |
   v
Grounded Azure OpenAI prompt
   |
   v
Cited answer / abstention
```

### Enterprise controls

- Microsoft Entra authentication.
- Authorization-aware document retrieval.
- Fail-closed ACL filter construction.
- Citation-first grounded generation.
- Indirect prompt-injection defense for retrieved content.
- No-guessing / evidence-insufficient behavior.
- Conflict-aware source handling.
- Application Insights / OpenTelemetry integration.
- Infrastructure-as-code and Azure Developer CLI deployment foundation.
- Automated tests and evaluation workflows.

## Evaluation

RAG quality is treated as an engineering problem, not a demo feature.

The regression suite covers:

- retrieval relevance (Recall@K, Precision@K, MRR),
- groundedness and citation correctness,
- abstention when evidence is insufficient,
- indirect prompt injection,
- authorization leakage.

See `evals/enterprise_regression.json` and `docs/ENTERPRISE_RAG.md`.

## Security model

Retrieved content is explicitly treated as **untrusted data**. Document instructions cannot override application policies. Missing identity fails closed, and material claims are expected to map to retrieved evidence.

For production Azure deployments, prefer Microsoft Entra ID/RBAC and managed identities over long-lived API keys. Azure's current RAG guidance also recommends retrieval-time access control and treating retrieved content as untrusted input. citeturn0search0turn0search6

## Why this is an AI Engineering project

This repository demonstrates engineering across the complete RAG lifecycle:

1. **Ingestion** — documents become searchable, enriched evidence.
2. **Retrieval** — lexical, vector, and semantic retrieval can be compared.
3. **Grounding** — generation is constrained by retrieved evidence.
4. **Security** — identity, ACLs, prompt-injection defenses, and safe boundaries.
5. **Evaluation** — regression cases and measurable retrieval/answer quality.
6. **Operations** — Azure deployment, telemetry, latency and token/cost signals.

Microsoft's current Azure RAG guidance emphasizes the same lifecycle: chunking, embeddings, retrieval, evaluation, and secure access control. citeturn0search1turn0search10

## Repository guide

- `docs/ENTERPRISE_RAG.md` — architecture and engineering decisions.
- `AI_ENGINEERING.md` — portfolio/interview framing and transformation notes.
- `SECURITY.md` — security posture and reporting guidance.
- `evals/` — synthetic regression corpus and evaluation scenarios.
- `app/backend/core/rag_policy.py` — testable RAG security policy primitives.
- `app/backend/approaches/` — retrieval and generation orchestration.
- `infra/` — Azure infrastructure as code.

## Deployment

The project retains the Azure Developer CLI (`azd`) deployment foundation and Azure services used by the original sample. Deployment requires an Azure subscription and appropriate permissions.

Do not deploy the project with production data until your organization's identity, network, logging, retention, data residency, and authorization requirements have been reviewed.

## Current scope

This is a serious AI-engineering portfolio project, not a claim of universal production readiness. Enterprise readiness depends on the target organization's threat model, compliance requirements, network topology, data classification, SLOs, and operational controls.
