# Security

## Scope

AzureBot is an enterprise RAG portfolio project. It processes potentially sensitive enterprise documents, so security is treated as part of the retrieval and generation design rather than as a separate checklist.

## Implemented controls

- Microsoft Entra authentication foundation.
- Authorization metadata propagated into retrieval.
- User-uploaded documents are associated with the authenticated principal.
- Fail-closed ACL filter helper in `app/backend/core/rag_policy.py`.
- Retrieved content is treated as untrusted data.
- Common indirect prompt-injection patterns are detected and neutralized by the policy helper.
- Grounded generation explicitly forbids disclosure of prompts, credentials, tokens, and unauthorized data.
- Citation-first response contract.
- OpenTelemetry / Application Insights integration in the application foundation.
- Dependency and CI workflows inherited from and extended around the Azure sample foundation.

## Security properties to verify before production

The repository does not claim that these controls alone establish compliance or production readiness. A deployment should additionally verify:

- tenant isolation and document-level authorization with realistic identities,
- network isolation/private endpoints where required,
- managed identities and Microsoft Entra RBAC instead of long-lived API keys,
- Key Vault and secret rotation,
- data retention/deletion requirements,
- audit logging without sensitive document content,
- rate limits and abuse controls,
- backup and disaster recovery,
- vulnerability/dependency scanning,
- adversarial RAG and authorization testing,
- cost and quota protections.

## Threat model

Primary threats include indirect prompt injection in documents, unauthorized retrieval, sensitive-data leakage, prompt disclosure, malicious uploads, denial of service, and configuration/credential compromise.

The application should assume that any retrieved document may contain attacker-controlled text. Azure's current guidance explicitly recommends retrieval-time access control and treating retrieved content as untrusted input. It also recommends Microsoft Entra ID/RBAC and managed identities for production access patterns.

## Reporting

For vulnerabilities specific to this portfolio repository, open a private security report through GitHub rather than publishing exploit details in a public issue. Do not include real customer data, credentials, access tokens, or other secrets in a report.

## Attribution

The repository began from Microsoft's `azure-search-openai-demo`. Microsoft security-policy text that was specific to Microsoft's own repositories has been replaced with security guidance appropriate to this portfolio repository. The upstream license remains in `LICENSE`.
