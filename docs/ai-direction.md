# AI integration direction

The proposed AI feature is an optional investigation explainer, not an autonomous offensive agent.

## Intended experience

When the user selects an observed service, the assistant may receive a deliberately bounded context containing public-safe fields such as service type, port, reported product, reported version, and candidate advisory identifiers. It can then:

- Explain what the observation means in beginner-friendly language.
- Distinguish observations, candidates, and confirmed findings.
- Suggest questions the operator can validate manually.
- Summarize synthetic or user-approved lab notes.
- Cite the OrbitSEC observation used to construct the explanation.

## Safety constraints

- No automatic terminal execution.
- No silent upload of project data or transcripts.
- No expansion beyond the project's declared authorized scope.
- No claim that a candidate advisory proves exploitation or vulnerability.
- No secrets, credentials, flags, or raw evidence included by default.
- Local inference preferred for routine development and private labs.
- Explicit user action required before any optional remote model request.

## Cost-conscious prototype

The first prototype can use deterministic sample responses and synthetic observations. A small local model can be introduced later behind a provider interface. Paid APIs remain optional and can be reserved for controlled portfolio demonstrations.

## Portfolio value

This direction demonstrates grounded AI integration, privacy boundaries, prompt-context minimization, explainability, safe human control, and evaluation thinking without turning OrbitSEC into a hidden command executor.
