# Enterprise RAG Engineering

## Product

AzureBot is now positioned as an **Enterprise Knowledge Control Center**: a secure, citation-first RAG platform for organizations that need answers over internal policies, procedures, operational manuals, and controlled documents.

The goal is not to reproduce a generic chatbot. The engineering focus is the boundary between enterprise identity, retrieval, evidence, model generation, evaluation, and operations.

## Architecture

```text
Microsoft Entra ID
       |
       v
Web UI -> Quart API -> RAG Orchestrator
                         |
             +-----------+-----------+
             |                       |
             v                       v
      Azure AI Search          Policy Layer
      hybrid/vector/semantic   ACL + injection defense
             |                       |
             +-----------+-----------+
                         v
                  Grounded prompt
                         |
                         v
                  Azure OpenAI
                         |
                         v
                 cited response

Blob Storage / Document Intelligence -> ingestion -> chunking -> embeddings -> Search
Application Insights / OpenTelemetry -> traces, latency, failures, token/cost signals
```

## Engineering decisions

1. **Application-controlled retrieval**: authorization and retrieval policy remain in the application layer so tenant/user filters are explicit and testable.
2. **Hybrid retrieval first**: combine lexical and vector signals; use semantic ranking when available.
3. **Evidence before generation**: the model receives retrieved evidence and a strict grounding contract rather than unrestricted knowledge-base completion.
4. **Fail closed**: missing identity must not result in an unrestricted document query.
5. **Documents are data, not instructions**: retrieved text is treated as untrusted input and screened for common indirect prompt-injection patterns.
6. **Citations are part of the contract**: material claims must map back to source documents/pages.
7. **Observability is part of quality**: retrieval latency, model latency, failures, and token usage should be measurable without logging sensitive document content.

## Original engineering added to the upstream foundation

- Enterprise grounding contract and prompt hardening.
- Provider-agnostic RAG security policy helpers.
- Fail-closed authorization filter construction.
- Indirect prompt-injection detection for retrieved documents.
- Regression tests for the policy layer.
- A portfolio-specific architecture and evaluation methodology.

## Evaluation plan

The repository uses a separate evaluation layer so retrieval and generation quality can be tested without requiring an Azure deployment for every unit test.

Required metrics:

- Recall@K / Precision@K for retrieval.
- MRR for ranking quality.
- Groundedness and citation correctness for generated answers.
- Abstention accuracy for questions not answerable from the corpus.
- Injection-resistance rate for adversarial documents.
- Authorization leakage rate: unauthorized evidence retrieved must be zero.

Every material prompt/retrieval change should be evaluated against the same regression set before release.

## Attribution

The deployment foundation and substantial application components originate from Microsoft's `azure-search-openai-demo`. This repository does not claim the upstream implementation as original work. Original engineering is documented separately and the upstream license is retained.
