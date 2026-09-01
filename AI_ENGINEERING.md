# AzureBot — AI Engineering Portfolio Guide

## Positioning

AzureBot is an **Enterprise Knowledge Control Center**: a citation-first RAG platform for answering questions over authorized enterprise documents.

The repository started from Microsoft's `azure-search-openai-demo`. The upstream implementation is attributed rather than presented as original work. The original engineering layer in this repository focuses on enterprise retrieval policy, grounding, security, evaluation, and operational architecture.

## Engineering story

### 1. Retrieval

The platform supports a full retrieval-to-generation path:

- document ingestion and processing,
- chunking and enrichment,
- embeddings,
- Azure AI Search lexical/vector/semantic retrieval,
- query rewriting,
- source extraction and citations,
- grounded Azure OpenAI generation.

### 2. Authorization

Authenticated identity is carried into the RAG pipeline. User-uploaded content is associated with the authenticated principal, and the new policy layer provides a fail-closed Azure AI Search ACL filter contract.

The design principle is simple: **authorization is a retrieval requirement, not a post-generation check.**

### 3. Prompt-injection defense

Retrieved documents are treated as untrusted data. The portfolio policy layer detects common indirect-instruction patterns and the generation prompt explicitly states that document instructions cannot override system/application policy.

This is important because RAG can turn a malicious document into an indirect attack surface.

### 4. Grounding and abstention

The generation contract requires:

- source-backed claims,
- explicit uncertainty when evidence is insufficient,
- conflict reporting when sources disagree,
- no fabricated citations,
- no disclosure of hidden prompts or credentials.

### 5. Evaluation

`evals/enterprise_regression.json` defines representative regression scenarios for grounded Q&A, abstention, indirect prompt injection, and authorization boundaries.

The target evaluation scorecard is:

| Area | Metric |
|---|---|
| Retrieval | Recall@K, Precision@K, MRR |
| Generation | Groundedness, citation correctness |
| Safety | Injection-resistance rate |
| Authorization | Unauthorized retrieval/leakage rate |
| Reliability | Abstention accuracy |
| Operations | latency, failures, token/cost signals |

### 6. Azure operations

The project retains the Azure Developer CLI, Bicep, Container Apps/Functions, Azure Storage, Azure AI Search, Azure OpenAI, Entra ID, and Application Insights foundation from the original sample.

The intended production pattern is Microsoft Entra/RBAC and managed identity rather than long-lived credentials wherever Azure supports it.

## Interview-ready explanation

**How did you make a RAG demo enterprise-oriented?**

> I treated retrieval as a security boundary. The authenticated principal is carried into the retrieval layer, the query is constrained by ACL metadata, retrieved documents are considered untrusted input, and the final model call receives a strict grounding contract. I then added a regression layer for retrieval quality, abstention, prompt injection, and authorization leakage so RAG quality can be measured instead of judged from a few demo questions.

**What was the main trade-off?**

> Application-controlled retrieval gives more control over authorization, evidence filtering, telemetry, and evaluation than delegating all retrieval behavior to a managed agent. The trade-off is more application code and more responsibility for maintaining the retrieval contract.

## Honest scope

This is a strong AI-engineering portfolio implementation, but it is not a blanket claim of production readiness or compliance. A real enterprise deployment still requires organization-specific threat modeling, network controls, retention policies, data classification, SLOs, disaster recovery, and operational validation.
