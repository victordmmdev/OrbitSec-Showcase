# Public security overview

This document explains the design intent of OrbitSEC without exposing private implementation details.

## Trust boundaries

OrbitSEC treats four concerns separately:

| Boundary | Goal | Public design statement |
|---|---|---|
| User intent | Prevent hidden action | Commands stay visible and user-initiated. |
| Authorized scope | Avoid accidental out-of-scope integration | Enhanced tool capture validates declared targets and fails closed when ambiguous. |
| Investigation integrity | Avoid presenting guesses as facts | Visual observations come from validated structured events and retain provenance. |
| Local project data | Reduce exposure of sensitive lab material | Sensitive state remains local and receives layered protection at rest. |

## Why layered data protection

One mechanism should not be responsible for every security property.

- Secret storage and project storage have different trust boundaries, so access material is not stored beside protected project content.
- Independent project keys reduce coupling between investigations.
- A memory-hard derivation step is appropriate for human-selected passphrases because human credentials have less entropy than generated keys.
- Authenticated encryption is used conceptually because confidentiality alone would not reveal whether stored data had been modified.
- Provenance is kept at the application layer because encryption cannot prove that an observation came from the expected command or parser.

## Deliberately not published

The showcase excludes:

- Source code for cryptography, authentication, key storage, and protected persistence.
- Ciphertext formats, associated-data layouts, parameters, salts, nonces, wrappers, and key identifiers.
- Recovery, deletion, retry, and failure-handling internals.
- Threat-model working documents and development decisions.
- Real project databases, scan output, terminal transcripts, evidence, targets, and credentials.
- Private repository history and local development-tool configuration.

## Claims and limitations

OrbitSEC is under development. Automated tests validate selected invariants, but the project has not claimed an independent professional security audit. Public documentation describes goals and observed behavior, not a guarantee of security suitability for production environments.
