# reNFT - Mitigation Review details
- Total Prize Pool: $19,800 in USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-02-renft-mitigation/submit)
- Starts TBD XXX XXX XX 20:00 UTC (ex. `Starts March 22, 2023 20:00 UTC`)
- Ends TBD XXX XXX XX 20:00 UTC (ex. `Ends March 30, 2023 20:00 UTC`)

## Important note 

Each warden must submit a mitigation review for *every High and Medium finding* from the parent audit that is listed as in-scope for the mitigation review. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues will be considered in-scope and listed here.

- [H-01: All orders can be hijacked to lock rental assets forever by tipping a malicious ERC20](https://github.com/code-423n4/2024-01-renft-findings/issues/614)
- [H-02: An attacker is able to hijack any ERC721 / ERC1155 he borrows because guard is missing validation on the address supplied to function call setFallbackHandler()](https://github.com/code-423n4/2024-01-renft-findings/issues/593)
- [H-03: An attacker can hijack any ERC1155 token he rents due to a design issue in reNFT via reentrancy exploitation](https://github.com/code-423n4/2024-01-renft-findings/issues/588)
- [H-04: Incorrect gnosis_safe_disable_module_offset constant leads to removing the rental safe's module without verification](https://github.com/code-423n4/2024-01-renft-findings/issues/565)
- [H-05: Malicious actor can steal any actively rented NFT and freeze the rental payments (of the affected rentals) in the escrow contract](https://github.com/code-423n4/2024-01-renft-findings/issues/418)
- [H-06: Escrow contract can be drained by creating rentals that bypass execution invariant checks](https://github.com/code-423n4/2024-01-renft-findings/issues/387)
- [H-07: Attacker can lock lender NFTs and ERC20 in the safe if the offer is set to partial](https://github.com/code-423n4/2024-01-renft-findings/issues/203)
  
- [M-01: A malicious lender can freeze borrower's ERC1155 tokens indefinitely because the guard can't differentiate between rented and non-rented ERC1155 tokens in the borrower's safe.](https://github.com/code-423n4/2024-01-renft-findings/issues/600)
- [M-02: A malicious borrower can hijack any NFT with permit() function he rents.](https://github.com/code-423n4/2024-01-renft-findings/issues/587)
- [M-03: Risk of DoS when stoping large rental orders due to block gas limit](https://github.com/code-423n4/2024-01-renft-findings/issues/538)
- [M-04: DoS of Rental stopping mechanism](https://github.com/code-423n4/2024-01-renft-findings/issues/501)
- [M-05: DOS possible while stopping a rental with erc777 tokens](https://github.com/code-423n4/2024-01-renft-findings/issues/487)
- [M-06: Incorrect ordering for deletion allows to flash steal rented NFT's](https://github.com/code-423n4/2024-01-renft-findings/issues/466)
- [M-07: Upgrading modules via executeAction() will brick all existing rentals](https://github.com/code-423n4/2024-01-renft-findings/issues/397)
- [M-08: Assets in a Safe can be lost](https://github.com/code-423n4/2024-01-renft-findings/issues/323)
- [M-09: Guard::checkTransaction restricts native ETH transfer from user's safes](https://github.com/code-423n4/2024-01-renft-findings/issues/292)
- [M-10: The owners of a rental safe can continue to use the old guard policy contract for as long as they want, regardless of a new guard policy upgrade](https://github.com/code-423n4/2024-01-renft-findings/issues/267)
- [M-11: Protocol does not implement EIP712 correctly on multiple occasions](https://github.com/code-423n4/2024-01-renft-findings/issues/239)
- [M-12: paused ERC721/ERC1155 could cause stopRent to revert, potentially causing issues for the lender.](https://github.com/code-423n4/2024-01-renft-findings/issues/220)
- [M-13: RentPayload's signature can be replayed](https://github.com/code-423n4/2024-01-renft-findings/issues/162)
- [M-14: Lender of a PAY order lending can grief renter of the payment](https://github.com/code-423n4/2024-01-renft-findings/issues/65)
- [M-15: Blocklisting in payment ERC20 can cause rented NFT to be stuck in Safe](https://github.com/code-423n4/2024-01-renft-findings/issues/64)
- [M-16: Blacklisted extensions can't be disabled for rental safes](https://github.com/code-423n4/2024-01-renft-findings/issues/43)

[ ⭐️ SPONSORS ADD INFO HERE ]

## Overview of changes

Please provide context about the mitigations that were applied if applicable and identify any areas of specific concern.

## Mitigations to be reviewed

### Branch
[ ⭐️ SPONSORS ADD A LINK TO THE BRANCH IN YOUR REPO CONTAINING ALL PRS ]

### Individual PRs
[ ⭐️ SPONSORS ADD ALL RELEVANT PRs TO THE TABLE BELOW:]

Wherever possible, mitigations should be provided in separate pull requests, one per issue. If that is not possible (e.g. because several audit findings stem from the same core problem), then please link the PR to all relevant issues in your findings repo. 

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/your-repo/sample-contracts/pull/XXX | H-01 | This mitigation does XYZ | 

## Out of Scope

Please list any High and Medium issues that were judged as valid but you have chosen not to fix.
