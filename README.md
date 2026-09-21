# AuthorityMatrix

AuthorityMatrix is a reusable GenLayer Intelligent Contract for semantic separation of duties.

A normal multisig answers one question: did enough keys approve? AuthorityMatrix answers a harder question first: which independent authority domains does this exact action touch?

The authority matrix is configured deterministically. Each dimension has its own approvers and threshold. A proposed action stores a canonical cryptographic binding over the exact action hash, frozen-description hash, consumer context hash, and immutable sealed matrix hash. Consumers must derive the expected description from their canonical executable payload and verify the description/action binding before execution. GenLayer consensus only classifies semantic jurisdiction. It never chooses approvers, thresholds, or whether missing approvals can be bypassed.

There is **no frontend**. This repository is intentionally a standalone Intelligent Contract primitive.

## Network lock

AuthorityMatrix targets **Studionet only**:

- network alias: studionet
- RPC: https://studio.genlayer.com/api
- chain ID: **61999**
- explorer: https://explorer-studio.genlayer.com/

The repository does not target 61997 and does not use the development-preview network.

## Why this primitive exists

An organisation may require different people to approve different parts of one action:

- finance approves spending and value at risk;
- security approves privileged access and attack-surface changes;
- data/privacy approves sensitive information handling;
- legal approves contractual or external obligations.

A flat 3-of-5 multisig cannot express that cleanly.

Example action:

    Pay 40 GEN for an external penetration test that uses customer-derived test fixtures.

A sealed matrix might derive:

    MONEY       threshold 1
    SECURITY    threshold 1
    DATA        threshold 1

The action does not become authorised until every required dimension reaches its deterministic threshold and the matrix-wide minimum number of distinct signers is also satisfied.

## Core invariants

1. The matrix is immutable after sealing.
2. Consensus selects jurisdiction, not permission.
3. OUT_OF_SCOPE never means approved.
4. AMBIGUOUS fails closed.
5. Authorisation is tied to an exact 32-byte action hash.
6. Consumers also pin a separate 32-byte context hash.
7. Consumers pin the sealed matrix definition hash.
8. Thresholds and approver membership are deterministic.
9. A matrix-wide distinct-signer floor prevents one cross-functional signer from satisfying a configured multi-person control alone.
10. A matrix may forbid the proposer from approving their own action.
11. An exact action hash can only be registered once.
12. Approvals may be revoked while pending, but not after authorisation.

## Contract lifecycle

    create_matrix
        |
    add_dimension x N
        |
    add_approver x N
        |
    seal_matrix
        |
    open_action(matrix_id, context_hash, action_hash, description)
        |
        +-- stores binding_hash = keccak(canonical(context_hash, matrix_hash, action_hash, description_hash))
        |
    classify_action
        |
        +-- IN_SCOPE ------> collect required approvals ------> AUTHORISED
        +-- OUT_OF_SCOPE --> terminal, not authorised
        +-- AMBIGUOUS -----> terminal, not authorised

## Consensus design

The leader produces only a bounded semantic result:

    {
      "status": "IN_SCOPE",
      "dimensions": ["money", "security", "data"],
      "reason": "..."
    }

Every validator independently repeats the jurisdiction task over the sealed matrix and frozen action description.

The custom validator accepts only when the validator independently agrees on:

- classification state; and
- the exact dimension bitmask.

Rationale text is diagnostic only and is not consensus-critical.

This is deliberately stricter than a format-only validator. A leader cannot omit a material authority dimension and still be accepted merely because its JSON is valid.

## Consumer interface

Consumers call:

    is_authorized_for(
        action_id,
        expected_context_hash,
        expected_action_hash,
        expected_description_hash,
        expected_binding_hash,
        expected_matrix_hash,
    )

The included AuthorityGate recomputes the binding hash and asks AuthorityMatrix to verify the same context, action, description, binding, and matrix commitments. In a production integration, the consumer must derive the expected description hash from the exact payload it will execute; it must not accept the description/hash pair as independent caller claims.

The context hash should commit to the intended consumer domain, for example:

    chain=61999 | consumer=<address> | version=1 | operation-family=treasury-execute

Hash that canonical value off-chain and use the resulting 32-byte digest.

## Repository layout

    contracts/
      authoritymatrix.py
      authority_gate.py

    tests/direct/
      test_authoritymatrix.py

    scripts/
      preflight.py
      deploy_studionet.sh

    docs/
      ARCHITECTURE.md
      SECURITY.md

    SUBMISSION.md
    DEPLOYMENT.md
    gltest.config.yaml

## Local checks

    python scripts/preflight.py
    python -m pip install -r requirements-test.txt
    pytest tests/direct/ -v -s

Optional linter:

    python -m pip install -r requirements.txt
    genvm-linter contracts/authoritymatrix.py
    genvm-linter contracts/authority_gate.py

## Deploy to Studionet

Use:

    bash scripts/deploy_studionet.sh

or:

    genlayer network set studionet
    genlayer network info
    genlayer deploy --contract contracts/authoritymatrix.py --rpc https://studio.genlayer.com/api

Before signing any transaction, confirm chain ID 61999.

## Live deployment and reviewer evidence

AuthorityMatrix is deployed on Studionet (61999) at [`0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9`](https://explorer-studio.genlayer.com/address/0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9). The deployment transaction [`0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad`](https://explorer-studio.genlayer.com/tx/0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad) finalized with MAJORITY_AGREE / SUCCESS.

A live three-domain matrix (money, security, data) was sealed. A multi-domain action required mask 7; one distinct approver remained insufficient, while a second approver moved it to AUTHORIZED. Wrong action/context/matrix hashes returned false. Ambiguous and out-of-scope actions both failed closed. The AuthorityGate reference consumer is deployed at [`0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0`](https://explorer-studio.genlayer.com/address/0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0); live evidence shows rejection before authorization, exact authorized execution, and replay rejection.

See [DEPLOYMENT.md](DEPLOYMENT.md) for transaction-by-transaction evidence, readbacks, matrix definition hash, consumer proof, and verification record.

## What AuthorityMatrix is not

AuthorityMatrix is not a DAO frontend, treasury, escrow, generic policy oracle, mandate system, or LLM multisig replacement.

It does one job: map an exact natural-language action to the sealed authority domains that must approve it, then enforce those approvals deterministically.
