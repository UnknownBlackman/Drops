## Drops by Ola

### Description

- This is going to be project which allows the creation of airdrops by anyone with parameters set.
- It would make allow users to create airdrops with erc1155, erc20 and erc721 tokens.
- This protocol would be decentralized and upgradeable. We would use openzeppelin's transparent proxy to allow the dao to upgrade when possible. There would be a custom token which has governance functions and would also be used in paying fees.
- It's just an airdrop engine where users can come create any airdrops at any point but pay certain fees in the protocol's custom token or eth. Fees would depend on certain parameters such as airdrop length, number of users etc.
- This would make use of chainlink to check price of tokens at any point.

### Contracts

1.  CreateAirdrop contract which allows users to create and launch airdrops with certain given parameters set by the user. These airdrops would be sort of like how proposals are done in governance.
2.  Main contract which controls all of the logic including airdrop owners paying fees to launch the airdrop, users interacting with the airdrop such as claiming, withdrawing, checking for new airdrops.
3.  Custom token contract which is used for both governance and payment of fees. We would just use openzeppelin's ercvotes here.
4.  May make use of an escrow vault (contract) which airdrop owners have to send tokens for airdrop to, or nfts as the case may be. From this vault the eligible users can then claim after completion of airdrop.
5.  Peripheral contracts
    - Pricefeed
    - Merkle proof

#### Airdrop

- Struct

1.  id - uint256
2.  Owner - address
3.  Airdrop type - bytes
    - type of airdrop - erc1155, erc20, erc721
4.  Eligibility criteria - bytes
5.  Description - bytes
6.  Start time - uint256
7.  Allocation - percentage of user deposit in vault
8.  Completion time - uint256 checks that ct is higher than st
9.  Grace period to claim - uint256
10. Status - enum

#### Functions

- createAirdrop contract

1. deposit to vault
2. create airdrop
3. cancel airdrop - only owner of airdrop and only during a period after airdrop creation (30% after st)
4. withdraw from vault -
   - Check owner no active airdrop or if active check withdraw amount less than allocation to active airdrops.
   - If airdrop, check cancelled airdrop or completed airdrop.
   - If completed airdrop, grace period must have passed.
   - Withdraw all deposit if cancelled or no airdrops
   - Withdraw unclaimed deposits if grace period has passed.

- Vault contract
- Allows deposit of erc20s, eth, protocol custom token, erc721, erc1155

1. deposit
2. withdraw

- dropsToken contract

1. custom erc20vote token allow voting and fee payment.

- dropsMain contract

1. claim airdrop
2. Join airdrop
3. check open airdrops or upcoming airdrops
