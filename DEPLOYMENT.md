# Deployment

## Target

AuthorityMatrix is locked to GenLayer Studionet:

- Network: Studionet
- Chain ID: 61999
- RPC: https://studio.genlayer.com/api
- Explorer: https://explorer-studio.genlayer.com/

## Reproduction checks

```bash
python scripts/preflight.py
python -m pip install -r requirements-test.txt
pytest tests/direct/ -v -s
```

Deployment used GenLayer CLI 0.39.1. Before deployment, the RPC `eth_chainId` was checked as `0xf22f` (decimal 61999).

## AuthorityMatrix — live deployment

- Address: [`0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9`](https://explorer-studio.genlayer.com/address/0x7145EDB4B3d1D56000A0a3ab713B15Eb1b1B25a9)
- Deployment transaction: [`0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad`](https://explorer-studio.genlayer.com/tx/0xe82d5d19e583268a4cccff0649493e6023a122eba51987fa6ff8af37cd0a73ad)
- Result: FINALIZED / MAJORITY_AGREE / SUCCESS
- Deployed source commit: `0ada2a68b2a34aeea52a27ad55e1a453267b6a3a`
- Contract source SHA-256: `264621541013D91EFD2070168FFC9D99D640EBF6484D44F84C2810EA94AC58E5`
- Source size: 37,185 bytes
- Deployer public address: `0x0d5540e0aD4B92Aa0ad4e5F1b8cD645ee1E363E7`

## Live matrix

Matrix ID `1` is sealed and read back from chain.

- Name: `AuthorityMatrix live review`
- Purpose: independent multi-domain review for high-impact actions affecting funds, security, and customer data.
- Owner/proposer: `0x0d5540e0aD4B92Aa0ad4e5F1b8cD645ee1E363E7`
- Minimum distinct approvers: 2
- Proposer approval: disabled
- Definition hash: `7bdb3c4afa7eb4ba8ed8e82b1484815e1771775bb9ea261431dbe0237ff7340c`
- Final readback: sealed; three dimensions; two approvers assigned per dimension.

Dimensions, each with threshold 1:

| Dimension | ID | Scope |
|---|---:|---|
| Money | 1 | Spending, transfers, amounts, and financial exposure |
| Security | 2 | Privileged access, production systems, and security controls |
| Data | 3 | Customer personal data, confidential records, and disclosure or movement |

Distinct approvers:

- Party A: `0xac3ac69dc0bde389256dd6748c75817ead9286d9`
- Party B: `0xa7eeae0e93793e3146cb14b0700251b8b0ebadfb`

## Live Studionet lifecycle evidence

Every transaction below was checked against the Studionet RPC and returned FINALIZED. Links point to the Studionet explorer.

### Matrix configuration

| Action | Transaction | Evidence |
|---|---|---|
| Create matrix 1 | [`0x59f48d7fe592a31721bb01f5d8e9611fafcae12ab6ae145d02787d2fc71a3aea`](https://explorer-studio.genlayer.com/tx/0x59f48d7fe592a31721bb01f5d8e9611fafcae12ab6ae145d02787d2fc71a3aea) | Matrix created |
| Add money dimension | [`0xc9b9c6fc6c237e766d3644e101a8dc0e9785b2f04c347ad6e1683f02619492a2`](https://explorer-studio.genlayer.com/tx/0xc9b9c6fc6c237e766d3644e101a8dc0e9785b2f04c347ad6e1683f02619492a2) | Dimension 1 |
| Seal matrix | [`0x50de1d9032f7cbe23a7f38e4f2cd6f07a0143cfe28583c8bd963626d5b821a44`](https://explorer-studio.genlayer.com/tx/0x50de1d9032f7cbe23a7f38e4f2cd6f07a0143cfe28583c8bd963626d5b821a44) | Readback: sealed, 3 dimensions |

The transcript retained the matrix-creation, money-dimension, and seal transaction IDs, but not the individual security/data dimension-add transaction IDs. Those writes are evidenced by the sealed on-chain readback and are not assigned invented hashes here.

Approver additions (Party A / Party B):

- Money: [`0xd5ca230744d5057fa7b314a82627881f74103481b6d9bd9e4f89f1da18e77dd4`](https://explorer-studio.genlayer.com/tx/0xd5ca230744d5057fa7b314a82627881f74103481b6d9bd9e4f89f1da18e77dd4), [`0x898f8c4133ff40c540c65720b5bc17dc5fa645440f59fec91121367c1e932084`](https://explorer-studio.genlayer.com/tx/0x898f8c4133ff40c540c65720b5bc17dc5fa645440f59fec91121367c1e932084)
- Security: [`0xf6e00935b4d1c628fe31a77482481154e10911e7189c2dbe03697755b865d2c9`](https://explorer-studio.genlayer.com/tx/0xf6e00935b4d1c628fe31a77482481154e10911e7189c2dbe03697755b865d2c9), [`0x65de26015f7cad761ea92e206274b72a69fdc86ecdfc976b5e24aac91e29b1ab`](https://explorer-studio.genlayer.com/tx/0x65de26015f7cad761ea92e206274b72a69fdc86ecdfc976b5e24aac91e29b1ab)
- Data: [`0xa1228b49b3b4d36d15dafa36a4dbcdf23c514ef43c2139eddce53b907661db42`](https://explorer-studio.genlayer.com/tx/0xa1228b49b3b4d36d15dafa36a4dbcdf23c514ef43c2139eddce53b907661db42), [`0x3ca2a3d5079824b52739560b80d0f65e7d2c7d922348a298b901e71b65ada907`](https://explorer-studio.genlayer.com/tx/0x3ca2a3d5079824b52739560b80d0f65e7d2c7d922348a298b901e71b65ada907)

### Multi-domain action and approvals

Action 1 used context hash `11` repeated 32 bytes and action hash `22` repeated 32 bytes. Its description was: “Pay a 40 GEN invoice to an external security vendor for a production penetration test and grant the vendor privileged access to customer-data-derived test records.”

| Action | Transaction | Final state / proof |
|---|---|---|
| Open action | [`0x7371294edb587222618507b9be29d38ed69c9d78f909a510b5c4d25d5c4d231b`](https://explorer-studio.genlayer.com/tx/0x7371294edb587222618507b9be29d38ed69c9d78f909a510b5c4d25d5c4d231b) | Action ID 1 |
| Semantic classification | [`0x3cf367a8c1f40560c43296cdd0c279c5f1390212a3eb108d706f92a0bc1d8a00`](https://explorer-studio.genlayer.com/tx/0x3cf367a8c1f40560c43296cdd0c279c5f1390212a3eb108d706f92a0bc1d8a00) | AWAITING_APPROVALS; required mask 7 (money + security + data) |
| Party A: money/security/data | [`0xe286975d88e730de05be1e6f4a14439b31be03c819229db1bf880d05e2a50de0`](https://explorer-studio.genlayer.com/tx/0xe286975d88e730de05be1e6f4a14439b31be03c819229db1bf880d05e2a50de0), [`0x5f53869fa8512a7da684defa188c84c55cd015069a6178e596093979c0b602fa`](https://explorer-studio.genlayer.com/tx/0x5f53869fa8512a7da684defa188c84c55cd015069a6178e596093979c0b602fa), [`0x07cf2a94c880a43307ae064eade17687e634ae2ff8e8f27e54cef193d7170a7a`](https://explorer-studio.genlayer.com/tx/0x07cf2a94c880a43307ae064eade17687e634ae2ff8e8f27e54cef193d7170a7a) | One distinct approver was insufficient; status remained AWAITING_APPROVALS |
| Party B: money/security/data | [`0xd24f459680e78215bae0ea258e76ea9cf2e3adf7745c10ebe715a39d50fad05a`](https://explorer-studio.genlayer.com/tx/0xd24f459680e78215bae0ea258e76ea9cf2e3adf7745c10ebe715a39d50fad05a), [`0x7ca4da39042bb0c9c36e0773a7eaa7985e1f318163324c2e3f47f70628415064`](https://explorer-studio.genlayer.com/tx/0x7ca4da39042bb0c9c36e0773a7eaa7985e1f318163324c2e3f47f70628415064), [`0x2e0dd4906ca02efa982a2e15ef98dcdf0574b9ae4803a77df208af4fefd0be0d`](https://explorer-studio.genlayer.com/tx/0x2e0dd4906ca02efa982a2e15ef98dcdf0574b9ae4803a77df208af4fefd0be0d) | Final readback AUTHORIZED; mask 7; two distinct approvers and all thresholds met |


Three separate `is_authorized_for` read checks returned false when respectively supplying the wrong context hash, action hash, or matrix definition hash.

### Fail-closed classification

- Ambiguous action (“Do the thing we discussed”): open [`0xc4a81fa75799db25c5af7a54bcf708307ee3add02ecd76d7998a0539bbdae65e`](https://explorer-studio.genlayer.com/tx/0xc4a81fa75799db25c5af7a54bcf708307ee3add02ecd76d7998a0539bbdae65e), classify [`0xbbfb6545a7066a9588f4016ee6e9b34902cdd928eeba5c2484d1aaac65640937`](https://explorer-studio.genlayer.com/tx/0xbbfb6545a7066a9588f4016ee6e9b34902cdd928eeba5c2484d1aaac65640937); readback AMBIGUOUS, mask 0, authorization false.
- Out-of-scope cosmetic action explicitly excluding money/security/data: open [`0x7fb79bafc02baf402c28eb6323e6b7872fc60279d1f22d2fda4653b5efec54bc`](https://explorer-studio.genlayer.com/tx/0x7fb79bafc02baf402c28eb6323e6b7872fc60279d1f22d2fda4653b5efec54bc), classify [`0xf0694872e622b23cf2dcbd46ac6b9570a2bae56e80f184be8fc9b58e21861c97`](https://explorer-studio.genlayer.com/tx/0xf0694872e622b23cf2dcbd46ac6b9570a2bae56e80f184be8fc9b58e21861c97); readback OUT_OF_SCOPE, mask 0, authorization false.

### AuthorityGate reference consumer

- Address: [`0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0`](https://explorer-studio.genlayer.com/address/0x6Db3601D964AEE358f25A500b09C577C27dF3Ed0)
- Deployment: [`0xdbe358be0682f3f33dc9f8981a667e527aad035f771c534ceace0b0346719004`](https://explorer-studio.genlayer.com/tx/0xdbe358be0682f3f33dc9f8981a667e527aad035f771c534ceace0b0346719004), FINALIZED / MAJORITY_AGREE / SUCCESS
- Bound to AuthorityMatrix above and the fixed context hash `11` repeated 32 bytes.
- Unapproved action 4: open [`0x211f3bede8b340b03dfbc16b5277a718a81de2d92d0ebcc16003181216da349e`](https://explorer-studio.genlayer.com/tx/0x211f3bede8b340b03dfbc16b5277a718a81de2d92d0ebcc16003181216da349e); gate refusal [`0x00191a1c22b2a2835c946c3bb441c8677574d8b4b92e34a09fd44d3413e0e1e5`](https://explorer-studio.genlayer.com/tx/0x00191a1c22b2a2835c946c3bb441c8677574d8b4b92e34a09fd44d3413e0e1e5); finalized expected contract execution error and `was_executed(66…66)=false`.
- Authorized action 1 execution: [`0x6930ec9d7ffadf5976f2b376272168b4a6a113c531415f4dcfc467bd8034cc09`](https://explorer-studio.genlayer.com/tx/0x6930ec9d7ffadf5976f2b376272168b4a6a113c531415f4dcfc467bd8034cc09); finalized success; `was_executed(22…22)=true`, with a matching `get_execution` readback.
- Replay of that action: [`0x5063f35e99bf31e20d2530482f735cf9c6596bc121ea1f4dd79787e8d0b72bb9`](https://explorer-studio.genlayer.com/tx/0x5063f35e99bf31e20d2530482f735cf9c6596bc121ea1f4dd79787e8d0b72bb9); finalized expected contract execution error; original execution remained recorded once.

## Verification record

The repository's final verification in this task reported:

- `python scripts/preflight.py`: PASS
- `python -m compileall contracts scripts tests`: PASS
- Direct Mode: 28/28 passed using the repository-pinned `genlayer-test==0.29.2` environment.
- GitHub Actions on source commit `0ada2a68b2a34aeea52a27ad55e1a453267b6a3a`: PASS, run [35471002444](https://github.com/ometere123/authoritymatrix/actions/runs/35471002444).

A separate locally installed Direct Mode plugin version did not match the repository pin and failed; it was not used as the passing result. No frontend was added.
