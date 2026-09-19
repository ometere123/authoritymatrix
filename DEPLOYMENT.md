# Deployment

## Target

AuthorityMatrix is locked to GenLayer Studionet:

- chain ID: 61999
- RPC: https://studio.genlayer.com/api
- explorer: https://explorer-studio.genlayer.com/

Do not deploy this repository using the development-preview network.

## Preflight

    python scripts/preflight.py
    python -m pip install -r requirements-test.txt
    pytest tests/direct/ -v -s

## Deploy

    genlayer network set studionet
    genlayer network info

Confirm chain ID 61999 before continuing.

    genlayer deploy --contract contracts/authoritymatrix.py --rpc https://studio.genlayer.com/api

Record the finalized contract address below only after verifying it in the Studionet explorer.

### AuthorityMatrix

- address: PENDING_LIVE_DEPLOYMENT
- explorer: PENDING_LIVE_DEPLOYMENT
- deployment transaction: PENDING_LIVE_DEPLOYMENT
- finalized: PENDING_LIVE_DEPLOYMENT

## Optional consumer proof

After deploying AuthorityMatrix, deploy AuthorityGate with:

1. the live AuthorityMatrix address;
2. a 32-byte context hash representing the intended consumer domain.

Then prove one negative and one positive path:

- an action lacking a required authority approval must be rejected by AuthorityGate.execute;
- the same exact action, after all required thresholds and the distinct-signer floor are satisfied, must execute once;
- a second execution with the same action hash must fail as replay.

### AuthorityGate

- address: PENDING_LIVE_DEPLOYMENT
- explorer: PENDING_LIVE_DEPLOYMENT
- deployment transaction: PENDING_LIVE_DEPLOYMENT
- finalized: PENDING_LIVE_DEPLOYMENT
