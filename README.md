# NexusDeFi-Protocol

## Overview

The **NexusDeFi-Protocol** is a decentralized finance (DeFi) ecosystem composed of multiple interconnected smart contracts that function together to enable decentralized financial services. Built on the Stacks blockchain, Nexus leverages Bitcoin's security to offer a range of DeFi services, including staking, governance, lending, and tokenized asset management.

### Key Components

1. **Nexus Token**: A fungible token compliant with the SIP-010 standard, used throughout the protocol for various operations.
2. **Vault**: A smart contract for securely storing tokens with a fee-based system for deposits and withdrawals.
3. **Lending**: Facilitates borrowing of tokens with collateral, including liquidation mechanisms.
4. **Oracle**: Supplies price data for assets, which is vital for the lending system.
5. **Governance**: Allows Nexus token holders to create and vote on proposals to improve the protocol.
6. **Staking**: Users can stake Nexus tokens to earn rewards in a decentralized manner.

---

## Installation

To deploy the NexusDeFi-Protocol contracts on the Stacks blockchain, you need to follow the steps below.

### Prerequisites

- [Node.js](https://nodejs.org/) v14.x or later.
- [Clarinet](https://github.com/hirosystems/clarinet) - A tool for developing and testing smart contracts on Stacks.
- A local Stacks blockchain or a testnet/mainnet account.

### Deployment Steps

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/nicholas-source/NexusDeFi-Protocol.git
   cd NexusDeFi-Protocol
   ```

2. **Install Dependencies:**

   Install Clarinet if you haven't:

   ```bash
   npm install -g @hirosystems/clarinet
   ```

3. **Compile the Contracts:**

   Use Clarinet to compile the contracts:

   ```bash
   clarinet check
   ```

4. **Deploy the Contracts:**

   To deploy the contracts to the testnet or mainnet, configure your deployment settings and use Clarinet to initiate deployment:

   ```bash
   clarinet deploy --network testnet
   ```

---

## Contract Overview

### 1. **Nexus Token (nexus-token.clar)**

The Nexus Token contract implements a SIP-010 compliant fungible token. It includes minting, transferring, and balance checking functionalities.

- **Key Functions:**

  - `transfer(amount, sender, recipient, memo)`: Transfers tokens between users.
  - `mint(amount, recipient)`: Mints new tokens for a given recipient (only by the contract owner).
  - `get-balance(who)`: Retrieves the balance of a specific user.
  - `set-token-uri(new-uri)`: Allows the contract owner to set a URI for token metadata.

- **Considerations:**
  - The contract allows minting by the owner but could benefit from a capped supply or a minting schedule.
  - Important actions like transfers and mints do not emit events, which would improve tracking and transparency.

### 2. **Vault (nexus-vault.clar)**

The Vault contract enables users to deposit tokens and withdraw them later, with a small fee deducted on deposits.

- **Key Functions:**

  - `deposit(token, amount)`: Allows users to deposit tokens into the vault.
  - `withdraw(token, amount)`: Withdraws tokens, ensuring the user has sufficient balance.
  - `get-deposit-balance(user, token)`: Retrieves a user's deposit balance.
  - `get-total-deposits(token)`: Shows the total amount of tokens deposited in the vault.

- **Considerations:**
  - Implement events for deposits and withdrawals for better transaction tracing.
  - Add functionality for updating the vault fee percentage and emergency withdrawal capabilities.

### 3. **Lending (nexus-lending.clar)**

The Lending contract allows users to borrow tokens by providing collateral. It supports liquidation if the collateral falls below a defined ratio.

- **Key Functions:**

  - `borrow(collateral-token, borrow-token, collateral-amount, borrow-amount)`: Facilitates borrowing by locking collateral.
  - `repay(collateral-token, borrow-token, repay-amount)`: Allows users to repay their loans.
  - `liquidate(borrower, collateral-token, borrow-token)`: Liquidates a borrower's collateral if they are below the liquidation threshold.

- **Considerations:**
  - Interest accrual is handled through a constant rate but could be improved with dynamic interest rates based on utilization.
  - Add events for borrows, repayments, and liquidations to enhance visibility.
  - Support for multiple collateral types could improve the flexibility of the lending system.

### 4. **Oracle (nexus-oracle.clar)**

The Oracle contract sets and retrieves price data for various assets, which the lending contract depends on for determining collateral and liquidation conditions.

- **Key Functions:**

  - `set-price(asset, price)`: Allows the contract owner to set the price of an asset.
  - `get-price(asset)`: Retrieves the price of an asset.

- **Considerations:**
  - Currently, the oracle is centralized (controlled by the contract owner). A decentralized oracle mechanism would improve trustlessness.
  - Add time-based price validity to ensure prices are fresh and accurate.

### 5. **Governance (nexus-governance.clar)**

The Governance contract enables Nexus token holders to create and vote on proposals for protocol changes.

- **Key Functions:**

  - `create-proposal(description)`: Allows users to create a new governance proposal.
  - `vote(proposal-id, vote-for)`: Lets users cast their vote on a proposal.
  - `end-proposal(proposal-id)`: Ends the voting process and decides whether the proposal passes or fails.

- **Considerations:**
  - Add a quorum requirement for proposal execution.
  - Implement time locks for executed proposals to prevent malicious governance actions.
  - Events should be added for the creation, voting, and execution of proposals.

### 6. **Staking (nexus-staking.clar)**

The Staking contract allows users to stake Nexus tokens in return for rewards, calculated based on the amount staked and the time spent staking.

- **Key Functions:**

  - `stake(token, amount)`: Users can stake their tokens to start earning rewards.
  - `unstake(token, amount)`: Users can unstake their tokens and stop earning rewards.
  - `claim-rewards(token)`: Claims pending staking rewards for the user.

- **Considerations:**
  - The reward calculation mechanism could be optimized for better gas efficiency.
  - Add cooldown periods for unstaking to discourage short-term staking behavior.

---

## Security Considerations

1. **Reentrancy Protection**: The contracts use the `try!` pattern to prevent reentrancy attacks, but a full audit is advised to ensure safety against reentrancy across all state-changing functions.
2. **Access Control**: Critical functions are limited to the contract owner, but a more robust multi-signature access control mechanism may enhance security.
3. **Precision Loss**: Be cautious of precision loss during arithmetic operations, particularly in the lending and staking contracts.
4. **Oracle Dependency**: As the oracle provides critical price data, ensure its security and consider adding fallback mechanisms for increased reliability.

---

## Potential Improvements

- **Flash Loans**: Introduce flash loans to improve capital efficiency in the lending system.
- **Advanced Governance**: Consider implementing quadratic voting or vote delegation to increase the fairness of the governance system.
- **Fee Distribution**: Add a mechanism for distributing protocol fees to token holders or stakers.
- **Liquidation Incentives**: Create incentives for third parties to liquidate undercollateralized loans promptly.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Contributors

We welcome contributions! Please refer to the [CONTRIBUTING](CONTRIBUTING.md) guidelines to get started.

Feel free to raise issues, submit PRs, and help grow the Nexus DeFi Protocol!
