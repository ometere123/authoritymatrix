# Architecture

## Design goal

AuthorityMatrix separates semantic jurisdiction from authorization mechanics.

GenLayer consensus answers only:

Which sealed authority dimensions materially govern this exact action description?

All authority is already present on-chain before that question is asked.

## Matrix

A matrix contains:

- owner during draft configuration;
- human-readable purpose;
- independent authority dimensions;
- one threshold per dimension;
- approver membership per dimension;
- matrix-wide minimum distinct signer count;
- optional maker-checker rule;
- canonical sealed definition hash.

A matrix becomes immutable when sealed.

## Dimension

A dimension is a bounded authority domain such as money, security, data, or legal.

The semantic description explains when that authority should be involved. Thresholds and approvers are deterministic state.

## Action

An action binds:

- sealed matrix ID and definition hash;
- proposer;
- consumer context hash;
- exact executable action hash;
- hash of the frozen natural-language description;
- consensus-derived required-dimension bitmask;
- deterministic approval state.

## Consensus boundary

The model cannot:

- add or remove approvers;
- change thresholds;
- authorize an action;
- weaken the global distinct-signer floor;
- change the action hash;
- change the consumer context;
- change the matrix definition.

It returns only a bounded jurisdiction classification.

The custom validator independently repeats that task and requires exact agreement on classification state and bitmask.

## Fail-closed states

OUT_OF_SCOPE is not an authorization shortcut. It means the matrix is not the correct authority system for the action.

AMBIGUOUS is terminal and non-authorizing. Callers must create a new exact action commitment with a clearer description rather than asking the contract to guess.

## Consumer binding

The consumer-facing view requires:

    action_id
    context_hash
    action_hash
    matrix_hash

This prevents a valid approval from being casually reused for another payload, another matrix version, or another consumer context.

## Why the matrix is immutable

Mutable authority definitions create a policy-substitution problem: an administrator could alter thresholds or approvers after an action was opened.

AuthorityMatrix avoids that class of ambiguity entirely. Updates require a new sealed matrix. Consumers decide which exact matrix hash they trust.
