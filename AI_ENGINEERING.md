# Azure RAG Engineering Portfolio Guide

## Positioning

`AzureBot` is based on Microsoft's `azure-search-openai-demo` and is being used as an AI Engineering portfolio workspace.

The repository demonstrates an end-to-end Azure RAG architecture involving Azure OpenAI, Azure AI Search, document ingestion, cloud deployment, identity, and observability. The upstream sample is the foundation; original portfolio value must come from modifications, experiments, evaluation, testing, and hardening added in this repository.

## Current repository assessment

The initial repository commit is effectively an import of the upstream Azure sample. For that reason, it would be misleading to present the existing implementation as entirely original engineering work.

That is not a reason to discard the project. It is a reason to make the fork visibly and technically yours.

## What makes this valuable for AI Engineering

### 1. RAG systems

Demonstrates the complete retrieval-to-generation path:

- document ingestion
- indexing
- vector/semantic retrieval
- prompt construction
- grounded generation
- citations

### 2. Azure architecture

The stack provides practical exposure to:

- Azure OpenAI
- Azure AI Search
- Azure Storage
- Azure Container Apps / App Service
- Azure Functions
- Azure AI Document Intelligence
- Microsoft Entra ID
- Application Insights / Azure Monitor
- Bicep
- Azure Developer CLI

### 3. Production engineering

A strong portfolio version should demonstrate more than a successful chat response. It should show:

- authentication and authorization
- secret management
- prompt-injection defenses
- evaluation and regression testing
- observability
- latency and cost measurement
- CI/CD
- infrastructure as code
- operational documentation

## Recommended transformation

### Phase 1 — Make the use case yours

Replace the generic fictional-company scenario with a domain that demonstrates useful enterprise AI engineering, such as:

- government policy assistant
- engineering-document assistant
- enterprise knowledge assistant
- procurement-document intelligence
- environmental-document research assistant

Use synthetic or public documents unless you have permission to use private data.

### Phase 2 — Build measurable RAG evaluation

Create a small evaluation set containing:

- question
- expected source document
- expected evidence
- acceptable answer characteristics

Measure retrieval quality and grounded answer quality. Keep evaluation results in the repository.

### Phase 3 — Harden the agent

Add and test:

- prompt-injection defenses
- source filtering
- authorization-aware retrieval
- input validation
- safe error handling
- sensitive-data-safe logging
- rate limiting where appropriate

### Phase 4 — Engineering quality

Add:

- typed Python interfaces
- unit tests
- integration tests
- RAG regression tests
- linting
- formatting
- type checking
- dependency/security scanning
- GitHub Actions

### Phase 5 — Azure operations

Document and, where practical, implement:

- managed identity
- Key Vault
- Application Insights
- Azure Monitor
- Bicep deployment
- `azd` deployment
- cost controls
- rollback/recovery procedures

## Recruiter-facing project statement

> Built and extended an Azure-based RAG platform using Python, Azure OpenAI and Azure AI Search, focusing on document ingestion, retrieval quality, grounded generation, enterprise identity, observability, evaluation, and cloud deployment.

Use this wording only after the corresponding engineering work has actually been implemented and verified.

## Important attribution rule

This repository is based on Microsoft's Azure Samples `azure-search-openai-demo`. Do not claim Microsoft's upstream implementation as original work. Preserve the upstream license and attribution while clearly documenting your own changes.

## Target outcome

The finished portfolio version should answer five interview questions convincingly:

1. **How did you build the RAG pipeline?**
2. **How did you measure whether retrieval and answers were good?**
3. **How did you secure enterprise data and model/tool access?**
4. **How did you deploy and observe the system on Azure?**
5. **What engineering trade-offs did you make around quality, latency and cost?**
