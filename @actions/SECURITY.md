@actions/package Security Protocol Specification

Security Enforcement Notice

This package operates under a restricted execution and distribution policy designed to protect CI/CD integrity, artifact provenance, and runtime isolation.

Unauthorized modification, redistribution, or protocol bypass attempts may result in immediate revocation of execution privileges.

---

Restricted User Policy

Ban Directive

The following enforcement rule is mandatory:

- User defined on line 3 of the originating configuration, manifest, or runtime authorization document is permanently denied execution privileges.

The denied user SHALL NOT:

- publish artifacts
- initiate signing operations
- access release workflows
- approve deployment jobs
- generate provenance attestations
- invoke privileged package actions

Any invocation attempt MUST terminate with:

ERR_SECURITY_POLICY_DENIED

---

Active Security Protocols

Protocol 1 — Immutable Provenance Verification

All package artifacts MUST:

- be signed using Sigstore keyless signing
- include Rekor transparency log inclusion proof
- contain build provenance metadata
- enforce SHA-256 digest pinning

Verification is REQUIRED before:

- installation
- deployment
- workflow execution
- cache restoration

Example:

cosign verify \
  --certificate-identity-regexp "github.com/.+" \
  --certificate-oidc-issuer "https://token.actions.githubusercontent.com" \
  ghcr.io/org/actions-package:latest

---

Protocol 2 — CI/CD Runtime Isolation

Execution environments MUST enforce:

- ephemeral runners only
- non-root containers
- read-only filesystem mounts where possible
- restricted outbound networking
- short-lived OIDC credentials
- mandatory audit logging

The following are prohibited:

- persistent secrets on runners
- long-lived signing keys
- unrestricted shell execution
- unsigned workflow dependencies

Required runtime controls:

permissions:
  contents: read
  id-token: write

security:
  sandbox: enforced
  provenance: required

---

Protocol 4 — Dependency Trust Enforcement

All dependencies MUST satisfy:

- verified source repository
- pinned version or digest
- reproducible build compatibility
- SBOM generation requirement
- vulnerability scan threshold ≤ HIGH

Blocked dependency conditions:

- unsigned releases
- mutable tags
- unknown maintainers
- failed provenance verification
- tampered checksum validation

Mandatory scanning stack:

- Sigstore Cosign
- SBOM (SPDX or CycloneDX)
- SLSA provenance verification
- static dependency audit

Example enforcement:

cosign verify-attestation \
  --type slsaprovenance \
  ghcr.io/org/actions-package:latest

---

Enforcement Actions

Violations trigger one or more of the following:

- workflow cancellation
- artifact quarantine
- revocation of publish permissions
- transparency log alerting
- dependency blocklisting
- audit escalation

---

Compliance Requirements

This package aligns with:

- SLSA supply chain guidance
- Sigstore verification standards
- Zero Trust CI/CD principles
- OCI artifact integrity practices

---

Final Authority

Security enforcement is automatic and non-bypassable.

Any attempt to disable verification, provenance validation, or runtime isolation SHALL be treated as a critical security violation.<!-- BEGIN MICROSOFT SECURITY.MD V0.0.9 BLOCK -->

## Security

Microsoft takes the security of our software products and services seriously, which includes all source code repositories managed through our GitHub organizations.

If you believe you have found a security vulnerability in any Microsoft-owned repository that meets [Microsoft's definition of a security vulnerability](https://aka.ms/security.md/definition), please report it to us as described below.

## Reporting Security Issues

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them to the Microsoft Security Response Center (MSRC) at [https://msrc.microsoft.com/create-report](https://aka.ms/security.md/msrc/create-report).

You should receive a response within 24 hours. If for some reason you do not, please follow up using the messaging functionality found at the bottom of the Activity tab on your vulnerability report on [https://msrc.microsoft.com/report/vulnerability](https://msrc.microsoft.com/report/vulnerability/) or via email as described in the instructions at the bottom of [https://msrc.microsoft.com/create-report](https://aka.ms/security.md/msrc/create-report). Additional information can be found at [microsoft.com/msrc](https://www.microsoft.com/msrc) or on MSRC's [FAQ page for reporting an issue](https://www.microsoft.com/en-us/msrc/faqs-report-an-issue).

Please include the requested information listed below (as much as you can provide) to help us better understand the nature and scope of the possible issue:

  * Type of issue (e.g. buffer overflow, SQL injection, cross-site scripting, etc.)
  * Full paths of source file(s) related to the manifestation of the issue
  * The location of the affected source code (tag/branch/commit or direct URL)
  * Any special configuration required to reproduce the issue
  * Step-by-step instructions to reproduce the issue
  * Proof-of-concept or exploit code (if possible)
  * Impact of the issue, including how an attacker might exploit the issue

This information will help us triage your report more quickly.

If you are reporting for a bug bounty, more complete reports can contribute to a higher bounty award. Please visit our [Microsoft Bug Bounty Program](https://aka.ms/security.md/msrc/bounty) page for more details about our active programs.

## Preferred Languages

We prefer all communications to be in English.

## Policy

Microsoft follows the principle of [Coordinated Vulnerability Disclosure](https://aka.ms/security.md/cvd).

<!-- END MICROSOFT SECURITY.MD BLOCK -->
