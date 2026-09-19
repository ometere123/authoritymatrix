# AuthorityMatrix submission notes

## One-line purpose

AuthorityMatrix is a reusable semantic separation-of-duties primitive that determines which independent authority domains govern an exact action, then enforces each domain's approval threshold deterministically.

## Why GenLayer is necessary

A normal smart contract can count signatures but cannot reliably determine that a natural-language action simultaneously touches finance, security, and data authority when the wording is not reducible to fixed selectors.

AuthorityMatrix uses GenLayer only for that semantic jurisdiction boundary. Validators independently derive the exact required-dimension mask. Approval thresholds, signers, immutable matrix definitions, action/context binding, and authorization are deterministic.

## Non-trivial consensus

The validator is not format-only. A leader result is accepted only when an independent validator agrees on both:

1. IN_SCOPE / OUT_OF_SCOPE / AMBIGUOUS
2. the exact authority bitmask

A well-formed leader response that omits security while the validator independently finds money + security is rejected.

## Reusability

Any consumer contract can pin a matrix hash and call is_authorized_for before performing a protected action.

The included AuthorityGate demonstrates the consumer surface without turning the submission into a full app. There is no frontend.

## Important design choices

- matrices are immutable after sealing;
- out-of-scope never auto-authorizes;
- ambiguous classification fails closed;
- exact action and consumer context hashes are mandatory;
- matrix-wide distinct-signer floors prevent one cross-functional approver from satisfying a configured multi-person control alone;
- proposer self-approval can be disabled;
- approvals may be revoked only before authorization.

## Tests

The direct-mode suite covers configuration ownership, seal invariants, semantic consensus, contradictory classifier output, independent validator disagreement, threshold enforcement, cross-domain distinct-signer rules, maker-checker separation, revocation, exact binding, cancellation, and one-shot classification.

## Network

Studionet only: chain ID 61999, RPC https://studio.genlayer.com/api.
