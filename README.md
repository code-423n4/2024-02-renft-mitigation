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
- [M-04: DoS of Rental stopping mechanism](https://github.com/code-423n4/2024-01-renft-findings/issues/501)
- [M-05: DOS possible while stopping a rental with erc777 tokens](https://github.com/code-423n4/2024-01-renft-findings/issues/487)
- [M-06: Incorrect ordering for deletion allows to flash steal rented NFT's](https://github.com/code-423n4/2024-01-renft-findings/issues/466)
- [M-08: Assets in a Safe can be lost](https://github.com/code-423n4/2024-01-renft-findings/issues/323)
- [M-09: Guard::checkTransaction restricts native ETH transfer from user's safes](https://github.com/code-423n4/2024-01-renft-findings/issues/292)
- [M-10: The owners of a rental safe can continue to use the old guard policy contract for as long as they want, regardless of a new guard policy upgrade](https://github.com/code-423n4/2024-01-renft-findings/issues/267)
- [M-11: Protocol does not implement EIP712 correctly on multiple occasions](https://github.com/code-423n4/2024-01-renft-findings/issues/239)
- [M-12: paused ERC721/ERC1155 could cause stopRent to revert, potentially causing issues for the lender.](https://github.com/code-423n4/2024-01-renft-findings/issues/220)
- [M-13: RentPayload's signature can be replayed](https://github.com/code-423n4/2024-01-renft-findings/issues/162)
- [M-16: Blacklisted extensions can't be disabled for rental safes](https://github.com/code-423n4/2024-01-renft-findings/issues/43)

## Overview of changes

### Token Whitelist
A token whitelist has been implemented to filter the tokens that are supported by the protocol. There are two whitelists: asset whitelist and payment whitelist.

For the asset whitelist, please assume that only "standard" ERC721/ERC1155 tokens will be added to this whitelist. The only exceptions to this are the burnable token extension and tokens that support `permit()` ([EIP-4494](https://eips.ethereum.org/EIPS/eip-4494)). Any vulnerabilities found that involves that functionality *will* be considered in scope.

For the payment whitelist, please assume that only "standard" ERC20 tokens will be added to this whitelist. More specifically, we will plan to support: DAI, USDC, USDT. Other payment tokens will be considered out of scope.

> In terms of what "standard" means, think of the Open Zeppelin implementations of these tokens. We will not be whitelisting any tokens that deviate too far from how we expect an ERC721/ERC1155/ERC20 token to behave.

### Custom Fallback Handler
A custom fallback handler has been implemented on all rental safes upon deployment. This fallback handler should never be allowed to be removed by an owner of a rental safe. Additionally, it should be able to prevent tokens that support `permit()` from being able to gaslessly approve another account to transfer assets out of the rental safe. The current implementation prevents all eip-1271 signatures that originate from assets that are on the active whitelist, which is intended behavior.

### Rental Creation Flow
The process of creating a rental has been slightly altered. Now, all tokens *must* be sent to the Create Policy when creating an order. From there, the Create Policy will perform accounting, set up the rental order, and then distribute the tokens to the correct parties. 

Any vulnerabilities that can demonstrate that the assets go to the wrong addresses, pass execution flow to another account while in possession of the rented asset but before it has been registered by the protocol, or can break the internal accounting of the payment system will be much appreciated. 

> Vulnerabilities that stem from Seaport will be considered in scope if they can be demonstrated to directly affect this protocol

### Maximum Rental Duration
A maximum rental duration has been introduced to prevent locking up assets for an unreasonable amount of time. Please assume that the maximum rental duration for any order is set to 21 days. 

### Replayability
Steps have been taken to prevent replayability of the co-signer's signature. Each `RentPayload` will now contain an order hash of the seaport order that it is intended for. Additionally, partial orders from seaport will not be supported by the protocol and will revert execution if a partial order is seen. With these two changes, we hope to ensure that one `RentPayload` can only be used one time and to fulfill only one unique seaport order. 

Any vulnerabilities that can show the same `RentPayload` being used either with 2 different order hashes or the same order hash more than once will be welcomed.

## Mitigations to be reviewed

### Branch
https://github.com/re-nft/smart-contracts/tree/97e5753e5398da65d3d26735e9d6439c757720f5

### Individual PRs

Wherever possible, mitigations should be provided in separate pull requests, one per issue. If that is not possible (e.g. because several audit findings stem from the same core problem), then please link the PR to all relevant issues in your findings repo. 

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/re-nft/smart-contracts/pull/7 | H-01, M-05, M-12 | Implements a whitelist so only granted assets can be used in the protocol | 
| https://github.com/re-nft/smart-contracts/pull/17 | H-01, M-05, M-12 | Implements batching functionality for whitelisting tokens so that multiple can be added at once | 
| https://github.com/re-nft/smart-contracts/pull/4 | H-02 | Adds check to prevent setting of the fallback handler by the safe owner | 
| https://github.com/re-nft/smart-contracts/pull/14 | H-03, H-06 | Introduces an intermediary transfer on rental creation to ensure assets are not sent to the safe until they have been registered as rented by the protocol | 
| https://github.com/re-nft/smart-contracts/pull/1 | H-04 | Fixes offset value so that the proper address is checked when disabling a gnosis module from a safe | 
| - | H-05 | Mitigation of this issue is the result of mitigating a handful of other findings (H-07, M-13, M-11, M-06) |
| https://github.com/re-nft/smart-contracts/pull/10 | H-07, M-13 | Prevents `RentPayload` replayability and ensures that orders must be unique by disallowing partial orders from seaport |
| https://github.com/re-nft/smart-contracts/pull/8 | M-01 | Allows distinction between rented and non-rented ERC1155 tokens. Renters should be able to transfer amounts of these tokens that do not cut into the active rented amount |
| https://github.com/re-nft/smart-contracts/pull/11 | M-02 | Introduces support for rented tokens that use `Permit()` for gasless approvals. The rental safe should now be able to prevent gasless signatures from approving rented assets |
| https://github.com/re-nft/smart-contracts/pull/9 | M-04 | `onStart` and `onStop` hook functions are now independent of one another and do not both need to be implemented at the same time. |
| https://github.com/re-nft/smart-contracts/pull/13 | M-06 | Checks if a rental order exists right away when stopping a rental. Prevents stopping of an order that doesnt exist, only to create it during a callback in the `stopRent()` execution flow |
| https://github.com/re-nft/smart-contracts/pull/5 | M-08 | Added support for burnable ERC721 and ERC1155 tokens |
| https://github.com/re-nft/smart-contracts/pull/6 | M-09 | Allows native ETH to be transferred out of a rental safe |
| https://github.com/re-nft/smart-contracts/pull/12 | M-10 | Introduces `isActive` check on the guard policy. Ensures that safes cannot use a guard that has been deactivated |
| https://github.com/re-nft/smart-contracts/pull/2 | M-11 | Properly implements EIP-712 |
| https://github.com/re-nft/smart-contracts/pull/15 | M-16 | Introduces more granular controls over whitelist for extensions |
| 

## Out of Scope

Please list any High and Medium issues that were judged as valid but you have chosen not to fix.

- [M-03: Risk of DoS when stoping large rental orders due to block gas limit](https://github.com/code-423n4/2024-01-renft-findings/issues/538)
- [M-07: Upgrading modules via executeAction() will brick all existing rentals](https://github.com/code-423n4/2024-01-renft-findings/issues/397)
- [M-14: Lender of a PAY order lending can grief renter of the payment](https://github.com/code-423n4/2024-01-renft-findings/issues/65)
- [M-15: Blocklisting in payment ERC20 can cause rented NFT to be stuck in Safe](https://github.com/code-423n4/2024-01-renft-findings/issues/64)
