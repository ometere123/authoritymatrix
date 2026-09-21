# Security model

## Threat model

AuthorityMatrix assumes action descriptions may be adversarial and treats them as data inside the classification prompt.

The contract is designed against:

- leader omission of a material authority domain;
- unknown or invented authority dimensions;
- malformed model output;
- automatic authorization of out-of-scope actions;
- mutable threshold or approver substitution after sealing;
- duplicate approval counting;
- same-signer cross-domain collapse when a distinct-signer floor is configured;
- proposer self-approval when maker-checker separation is enabled;
- action-hash replay inside the registry;
- consumer use under a mismatched matrix, context, action commitment, or classified-description commitment.

## Consensus safety

The custom validator does not validate response shape alone. It independently derives semantic jurisdiction and requires exact agreement on the required bitmask.

Diagnostic rationale is intentionally ignored by the equivalence check because wording differences should not affect authorization.

## Deterministic safety

Threshold satisfaction, approver membership, duplicate prevention, distinct-signer counts, maker-checker rules, hash bindings, and final authorization are deterministic.

## Known limits

AuthorityMatrix stores a canonical binding hash over the action hash, frozen description hash, consumer context hash, and sealed matrix definition hash. AuthorityGate independently recomputes the binding and AuthorityMatrix verifies the exact stored pair. A consumer must derive its expected description hash from the exact canonical executable payload, not accept unrelated prose or a description hash from an untrusted caller. This cryptographically prevents pair substitution after classification; it does not prove that arbitrary prose truthfully describes arbitrary payload bytes.

The context hash is also only as strong as the canonical context encoded by the integrator. It should include chain identity, consumer identity/version, and operation family where appropriate.

The contract does not discover omitted facts. If a proposer describes an executable payload dishonestly, the semantic classifier only sees the submitted frozen description. A secure integration therefore displays or derives the description from the canonical payload rather than accepting arbitrary prose from an untrusted agent.

The primitive does not replace wallet signatures, role-based access control, or consumer-side replay protection. AuthorityGate demonstrates one minimal replay-safe consumer pattern.
