# OrbitSEC

### Visual security investigation cockpit for Kali Linux

OrbitSEC is a local-first desktop workspace that connects an interactive terminal, authorized security-tool execution, structured evidence, and a live visual investigation map.

Instead of forcing learners and ethical security practitioners to reconstruct an investigation from terminal output, scattered notes, and browser tabs, OrbitSEC turns supported tool results into traceable hosts, services, candidate advisories, and practical next-step checks—without hiding the commands that produced them.

> **Development preview:** OrbitSEC is an active portfolio and learning project. This public repository presents the product, architecture, progress, and design decisions. Sensitive implementation code, private planning, real evidence, credentials, and security internals are intentionally not distributed here.

## Product vision

Security work often becomes fragmented:

- The terminal contains the operator's actions.
- Scan output contains the raw observations.
- Notes contain interpretation and unfinished hypotheses.
- Vulnerability databases provide candidates, not confirmation.
- Diagrams quickly become disconnected from the evidence.

OrbitSEC brings those activities into a single investigation workspace while preserving the normal Kali workflow. Commands remain visible and user-controlled. Supported structured output is validated before becoming application state. Every projected fact retains a path back to its source observation.

## Current development preview

The current build demonstrates a vertical investigation flow:

1. OrbitSEC opens a local authorized workspace.
2. The operator works through an embedded terminal.
3. Supported Nmap XML can be imported or captured through a controlled integration path.
4. The parser applies input budgets and rejects malformed or unsupported data without publishing trusted partial state.
5. Validated events become hosts and services in the investigation model.
6. The same observations appear in a live 3D universe and an accessible facts panel.
7. Selecting a service reveals its fingerprint, provenance, validation focus, and candidate CVE information when available.

This creates an evidence-oriented experience rather than a decorative scan viewer.

## Features implemented

### Desktop investigation workspace

- Tauri 2 desktop shell targeting Kali Linux.
- React and TypeScript investigation interface.
- Rust boundary for local operating-system and tool integration.
- Responsive dashboard composed of visualization, observations, scope, and terminal areas.
- Reduced-motion support and keyboard-accessible observation controls.

### Interactive terminal foundation

- Embedded terminal based on xterm.js.
- Local PTY lifecycle managed by the Rust backend.
- Interactive input, output, resize, interruption, and close behavior.
- Commands remain visible and user-initiated.
- No automatic privilege escalation.
- Structured capture is separated from the human-oriented terminal presentation.

### Authorized project scope

- Each project declares allowed targets.
- Hostname, IPv4, IPv6, and network-scope inputs are validated and normalized.
- Unsupported or traversal-like scope input is rejected.
- Enhanced integrations are designed to fail closed when target intent is ambiguous.
- Authorization state remains visible in the workspace.

### Nmap evidence pipeline

- Nmap XML is the first structured security-tool adapter.
- Strict budgets cover input size, nesting depth, hosts, ports, events, and imported strings.
- Malformed XML, unsupported structures, DTD/entity attacks, and oversized input are rejected.
- Host and service observations are normalized into typed domain events.
- Events include project identity and provenance metadata.
- Invalid imports do not replace the last trusted investigation state.

### Visual investigation model

- Live 3D universe representing observed hosts and exposed services.
- Accessible text-based facts panel presenting the same underlying state.
- Synchronized service selection across the investigation interface.
- Host address, hostname, operating-system hint, ports, protocols, states, products, versions, and CPE identifiers are projected only after validation.
- Motion preferences do not change investigation meaning.

### Service assessment assistance

- Contextual validation checks for common HTTP, SSH, SMB, database, FTP, and DNS services.
- Suggestions are explicitly labeled as checks, not confirmed vulnerabilities.
- Candidate CVE lookup is available when a compatible service identifier exists.
- Candidate advisories remain distinct from verified findings.
- Observation provenance can identify scanner, observation time, artifact reference, and parser version.

### Protected local project foundation

- Independent local project boundaries.
- Protected persistence designed for sensitive investigation state.
- Operating-system-backed secret integration and a user-controlled fallback path.
- Failure-closed behavior for incorrect credentials, altered content, incompatible versions, and corrupted state.
- Local session lifecycle designed to release sensitive material when a project is closed or switched.
- Explicit project deletion and protected export behavior remain under active development and validation.

The public showcase deliberately omits protected-storage source code, formats, parameters, key-handling logic, recovery rules, and internal threat-model artifacts.

## Architecture overview

```text
┌───────────────────────────────────────────────────────────┐
│                     OrbitSEC Desktop                      │
├───────────────────────────┬───────────────────────────────┤
│ React / TypeScript        │ Rust / Tauri boundary         │
│                           │                               │
│ • Investigation UI       │ • PTY lifecycle               │
│ • 3D universe            │ • Scope enforcement           │
│ • Accessible facts       │ • Structured import           │
│ • Selection state        │ • Project lifecycle           │
└──────────────┬────────────┴──────────────┬────────────────┘
               │                           │
               v                           v
       Validated domain events      Local Kali services
               │
               ├────────> Visual projection
               ├────────> Evidence details
               └────────> Assessment context
```

### Data flow

```text
Visible user action
        ↓
Authorized-scope validation
        ↓
Supported machine-readable output
        ↓
Bounded parser and schema validation
        ↓
Provenance-bearing normalized events
        ↓
Investigation reducer
        ↓
3D view + accessible facts + assessment context
```

## Important design decisions

### Structured output over terminal scraping

Terminal text is designed for humans and can change with color, localization, tool versions, and interactive behavior. OrbitSEC prefers documented machine-readable formats as the authoritative source for investigation facts.

### One fact model, multiple views

The 3D universe and accessible panel project the same validated state. The visual experience therefore does not create a second, conflicting source of truth.

### Provenance before automation

A useful investigation tool must explain where a fact came from. OrbitSEC retains operation, artifact, parser, project, and observation identity rather than reducing everything to an untraceable host card.

### Suggestions are not findings

A reported software version or candidate CVE is a reason to investigate, not proof of a vulnerability. The interface preserves this distinction in its labels and interaction model.

### Local-first privacy

Lab projects may contain sensitive targets, commands, findings, and notes. OrbitSEC avoids hosted storage and telemetry in its initial architecture. Remote export or future AI providers must remain explicit user choices.

## Protection model

OrbitSEC separates protection responsibilities instead of relying on one mechanism for every security property:

| Layer | Responsibility | Reason |
|---|---|---|
| Authorized scope | Constrain enhanced tool integrations | Reduces accidental interaction outside the declared lab boundary. |
| Structured validation | Reject malformed or excessive input | Prevents untrusted tool output from becoming trusted application state. |
| Provenance | Preserve the origin of observations | Keeps visual facts explainable and reviewable. |
| Project separation | Isolate investigation state | Limits coupling between independent labs and projects. |
| Operating-system secret boundary | Separate access material from project content | Avoids storing all protection material beside the data it protects. |
| Memory-hard credential processing | Handle human-selected credentials more defensibly | Human passphrases have different properties from generated key material. |
| Authenticated protection at rest | Preserve confidentiality and detect modification | Stored data must fail closed when altered or opened under the wrong context. |
| Session lifecycle | Minimize unnecessary sensitive state in memory | Project access should end when the corresponding session ends. |

Only the purpose of each layer is public. This repository does not disclose internal formats, parameters, keys, wrappers, identifiers, recovery behavior, or sensitive implementation code.

## Technology stack

| Area | Technology | Role |
|---|---|---|
| Desktop | Tauri 2 | Controlled native desktop boundary |
| Backend | Rust | PTY, validation, parsing, persistence, and OS integration |
| Frontend | React + TypeScript | Investigation state and user experience |
| Build | Vite | Frontend development and production bundle |
| Terminal | xterm.js + portable-pty | Interactive local shell experience |
| Visualization | Three.js + React Three Fiber | Live 3D investigation universe |
| Structured parsing | quick-xml | Bounded Nmap XML processing |
| Local persistence | SQLite | Contained project state |
| Testing | Vitest, Testing Library, Rust tests | Domain, interface, parser, and lifecycle verification |

## Verification snapshot

The development snapshot used to prepare this showcase passed:

- **15 frontend tests** across domain behavior and interface components.
- **42 Rust unit tests** covering parser, scope, command, session, authentication-boundary, and project behavior.
- **8 Rust integration tests** covering protected repository lifecycle and failure cases.
- **50 Rust tests total**, with no failures.
- Sensitive-artifact Git audit.
- Git diff whitespace validation.

Test coverage includes malformed and oversized Nmap input, missing provenance, scope validation, project path containment, stale state, altered persisted content, concurrent operations, session transitions, and accessible UI behavior.

These checks describe the current development snapshot. They are not a claim of a completed independent security audit or production certification.

## Safe demonstration scenario

Public demonstrations use synthetic fixtures only:

1. Start a disposable authorized lab project.
2. Import a synthetic Nmap XML fixture containing fictional or designated demonstration targets.
3. Show host and service projection in the 3D universe.
4. Select a service from the accessible facts panel.
5. Review provenance, validation suggestions, and candidate-advisory labeling.
6. Demonstrate that malformed or oversized input is rejected without replacing trusted state.

Real targets, flags, credentials, transcripts, project databases, and evidence are never included in this repository.

## AI integration direction

The planned OrbitSEC Copilot is an optional explanation layer, not an autonomous offensive agent. It is intended to:

- Explain selected observations in beginner-friendly language.
- Summarize user-approved lab notes.
- Suggest validation questions grounded in structured facts.
- Distinguish observation, hypothesis, candidate advisory, and confirmed finding.
- Cite the OrbitSEC context used for an explanation.
- Prefer local inference for private labs and cost-conscious development.

It will not silently execute terminal commands, expand project scope, upload raw evidence by default, or treat model output as trusted investigation state.

Read the detailed [AI integration direction](docs/ai-direction.md).

## Roadmap

The public roadmap is maintained in [ROADMAP.md](ROADMAP.md). The next milestones focus on completing the protected project lifecycle, strengthening terminal capture, connecting visual facts back to originating commands, and prototyping a local-first investigation explainer.

## Repository policy

This showcase follows an explicit publication allowlist. It contains only:

- Public product documentation.
- Public roadmap and release notes.
- Sanitized diagrams.
- Synthetic screenshots or videos added after review.

It never contains:

- OrbitSEC application source code.
- Private Git history or GSD planning artifacts.
- Cryptographic, authentication, secret-storage, or persistence implementation.
- Real scans, evidence, transcripts, targets, flags, credentials, or databases.
- Local `.env`, editor, agent, or automation configuration.

## Responsible use

OrbitSEC is intended for CTF environments, personal laboratories, education, and systems where the operator has explicit authorization. The project does not encourage or automate unauthorized access.

## Project status

OrbitSEC is under active development as a cybersecurity, desktop engineering, visualization, and applied-AI portfolio project. Interfaces and behavior may change before the first distributable release.
