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

The Direct Mode suite covers configuration ownership, seal invariants, semantic consensus, contradictory classifier output, independent validator disagreement, threshold enforcement, cross-domain distinct-signer rules, maker-checker separation, revocation, exact binding, cancellation, and one-shot classification. The recorded final run passed 28/28 tests.

## Network and live deployment

Studionet only: chain ID 61999, RPC https://studio.genlayer.com/api, explorer https://explorer-studio.genlayer.com/.

AuthorityMatrix: [`0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9`](https://explorer-studio.genlayer.com/address/0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9). Deployment transaction: [`0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad`](https://explorer-studio.genlayer.com/tx/0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad), FINALIZED / MAJORITY_AGREE / SUCCESS.

A sealed matrix with money, security, and data dimensions classified one cross-domain action with mask 7. One signer was insufficient; two distinct signers satisfied all thresholds. Wrong hash bindings returned false, and ambiguous/out-of-scope actions remained unauthorized. AuthorityGate at [`0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0`](https://explorer-studio.genlayer.com/address/0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0) rejected an unauthorized action, executed the authorized action once, and rejected replay.

The deployed contract source is commit `0ada2a68b2a34aeea52a27ad55e1a453267b6a3a`; see [DEPLOYMENT.md](DEPLOYMENT.md) for exact source hash, definition hash, transaction links, statuses, and readback details.
