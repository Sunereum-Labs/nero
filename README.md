<center>
<h1> SunRe: BitVM2 For Resinsurance </h1>
</center>

Adaptation of the BitVM2 protocol for securing Reinsurance Capacity to help communities around the world withstand Climate Disaster 
- credit to Nero's BitVM2 Protocol implemention originally at https://github.com/distributed-lab/nero

# SunRe's Reinsurance Smart Contract

## Overview

This project demonstrates a novel reinsurance smart contract implementation using BitVM2 on the Bitcoin blockchain. It showcases how complex financial instruments can be created on Bitcoin by leveraging off-chain computations with on-chain verification.

## Key Features

- Bitcoin-based reinsurance contract with yield generation
- Off-chain risk assessment and yield calculations
- On-chain verification of off-chain computations
- Dynamic premium and coverage calculations
- Claim processing and contract state management

## Novel Mechanisms in SunRe's Reinsurance Protocol

SunRe's reinsurance protocol introduces several novel mechanisms that enhance the traditional reinsurance process by leveraging blockchain technology and off-chain computations. Here are the key mechanisms and their benefits:

1. **Parametric Triggers**: 
   - SunRe's protocol allows for the integration of parametric triggers, which are predefined conditions that automatically trigger certain actions, such as liquidation or payout, based on specific events or data points.
   - These triggers can be integrated with any on-chain or off-chain oracle, providing flexibility and adaptability to various reinsurance scenarios.
   - For example, a parametric trigger could be set to automatically liquidate a portion of the reinsurance pool if a certain weather event occurs, as reported by a trusted weather oracle.

2. **Dynamic Yield Generation**:
   - The protocol supports dynamic yield generation through integration with multi-chain smart contracts.
   - This allows the reinsurance pool to generate yield based on the specific needs and risk profiles of the reinsurance business case.
   - By leveraging smart contracts on different blockchains, SunRe can optimize yield generation strategies and provide more competitive premiums and coverage options.

3. **Off-Chain Computations with On-Chain Verification**:
   - Complex computations, such as risk assessment, yield calculation, and claim verification, are performed off-chain to reduce on-chain computational load and costs.
   - The results of these off-chain computations are then verified on-chain using cryptographic proofs, ensuring the integrity and trustworthiness of the process.
   - This approach combines the efficiency of off-chain computations with the security and transparency of on-chain verification.

4. **Flexible Reinsurance Capacity and Premiums**:
   - The protocol allows users to set reinsurance capacity and premiums based on their specific requirements.
   - Users can deposit BTC into the reinsurance pool and receive a proportional share of the yield generated.
   - For example, if the total reinsurance capacity is $10M and a user deposits $1M, they would be entitled to 10% of the premiums and yield generated by the pool.

5. **Integration with Oracles**:
   - SunRe's protocol can integrate with both on-chain and off-chain oracles to obtain real-time data for parametric triggers and other computations.
   - This integration enhances the accuracy and reliability of the reinsurance process by providing up-to-date information from trusted sources.
   - Oracles can be used to fetch data such as weather conditions, market prices, and other relevant metrics that impact the reinsurance contract.

By incorporating these novel mechanisms, SunRe's reinsurance protocol offers a more efficient, transparent, and flexible solution for managing reinsurance contracts. The ability to integrate with oracles and multi-chain smart contracts further enhances the protocol's adaptability to various business cases and market conditions.

## How it Works with BitVM2

This contract utilizes BitVM2's capabilities to perform complex computations off-chain while still maintaining the security and trustlessness of the Bitcoin blockchain. Key aspects include:

1. **Off-chain Computation**: Complex operations like risk assessment, yield calculation, and claim verification are performed off-chain.
2. **On-chain Verification**: Results of off-chain computations are verified on-chain using BitVM2's verification mechanisms.
3. **Large Integer Arithmetic**: Utilizes U256 for precise calculations with large numbers.
4. **State Management**: Contract state is updated based on verified off-chain computations.

## Main Components

1. `ReinsuranceContractWithOffChain`: The main struct representing the reinsurance contract.
2. Off-chain data structures:
   - `OffChainRiskAssessment`
   - `OffChainYieldCalculation`
   - `OffChainClaimVerification`
3. Key operations:
   - Pledge verification
   - Risk assessment verification
   - Yield calculation verification
   - Claim verification
   - State update
   - Termination check

## Setup and Testing

### Prerequisites

- Rust programming environment
- Bitcoin-related crates (as specified in the `use` statements)

### Running the Tests

1. Clone the repository
2. Navigate to the project directory
3. Run the tests using Cargo:

## :file_folder: Contents

The project contains multiple crates:

| Crate | Description |
| --- | --- |
| [bitcoin-splitter](bitcoin-splitter/README.md) | A crate for splitting the Bitcoin script into multiple parts as suggested by the recent [^1]). |
| [bitcoin-winternitz](bitcoin-winternitz) | Winternitz Signature and recovery implementation based on BitVM's [`[signatures]`](https://github.com/BitVM/BitVM/tree/main/src/signatures) package. |
| [bitcoin-utils](bitcoin-utils) | Helper package containing implementation of certain fundamental operations and debugging functions. |
| [bitcoin-testscripts](bitcoin-testscripts) | A collection of test scripts for testing BitVM2 concept. |
| [bitcoin-scriptexec](bitcoin-scriptexec) | A helper crate for executing Bitcoin scripts. Fork of [BitVM package](https://github.com/BitVM/rust-bitcoin-scriptexec). |

## Setting up a Local Bitcoin Node

```shell
docker compose up -d
```

> [!WARNING]
> Sometimes Docker Compose may fail at step of creating the volumes, the most simple solution is, in regards of failure, just trying starting it again several times until it works.

Let us create a temporary alias for `bitcoin-cli` from the container like this:

```shell
alias bitcoin-cli="docker compose exec bitcoind bitcoin-cli"
```

Create a fresh wallet for your user:

```shell
bitcoin-cli createwallet "my"
```

> [!WARNING]
> Do not create more than one wallet, otherwise further steps would require
> a bit of modification.

Generate fresh address and store it to environmental variable:

```shell
export ADDRESS=$(bitcoin-cli getnewaddress "main" "bech32")
```

Then mine 101 blocks to your address:

```shell
bitcoin-cli generatetoaddress 101 $ADDRESS
```

> [!NOTE]
> Rewards for mined locally blocks will go to this address, but, by protocol rules, BTCs are mature only after 100 confirmations, so that's why 101 blocks are mined. You can see other in  `immature` balances fields, after executing next command.
>
> For more info about Bitcoin RPC API see [^2].

```shell
bitcoin-cli getbalances
```

## Examples

The `examples` folder contains a test script `sunre_test.rs` that allows setting reinsurance capacity and premiums paid, and demonstrates depositing BTC for a portion of yield. This script includes functions for verifying the contract and generating valid and invalid input-output pairs.

[^1]: https://bitvm.org/bitvm_bridge.pdf
[^2]: https://developer.bitcoin.org/reference/rpc/
