## Technical Architecture

### Table of Contents
- [Kickstart Phase Progress](#kickstart-phase-progress)
  - [Vault Standard on Soroban](#vault-standard-on-soroban)
  - [Hedge/Risk Market Implementation](#hedgerisk-market-implementation)
  - [Acurast Oracle Integration](#acurast-oracle-integration)
  - [Insurance Use Case: Flight Delay POC](#insurance-use-case-flight-delay-poc)
  - [Manual Market Creation](#manual-market-creation)
- [Build Phase Additions](#build-phase-additions)
  - [Vault Factory and UI Development](#vault-factory-and-ui-development)
  - [Real User Onboarding and Beta Planning](#real-user-onboarding-and-beta-planning)
  - [New Insurance Verticals and Developer Outreach](#new-insurance-verticals-and-developer-outreach)

## Kickstarter Phase Progress

During the **Kickstarter Grant**, we focused on validating the core building blocks required to enable decentralized parametric insurance on Soroban. Our goal was to prove that if each part of the system—vaults, controllers, oracles—worked in isolation and together, then Sentinel Protocol could serve as a powerful foundation for building real-world risk markets.


### 🧱 Vault Standard on Soroban

We began by porting the **ERC-4626 vault standard**—commonly used in Ethereum DeFi for managing deposits and yield—to Soroban using Rust-based smart contracts.

- We implemented two types of vaults:
  - **Hedge Vaults**, where insurance buyers deposit funds to hedge against risk.
  - **Risk Vaults**, where counterparties deposit funds to take on risk in exchange for yield.
- When a user deposits funds, they receive **LP tokens** representing their ownership share in the vault. These tokens can be burned to withdraw funds based on the vault’s balance.
- This design mirrors successful yield-bearing vaults in DeFi, adapted for the parametric insurance use case.

[View the Vault Repo](#)

### ⚖️ Hedge/Risk Market Implementation

With the vaults implemented, we connected them using a **Controller Contract** that manages the lifecycle of each insurance market:

- It decides when funds should flow from one vault to the other based on external data.
- It supports two core actions:
  - **Liquidation**: If a covered event (e.g., flight delay) occurs, capital is transferred from the Risk Vault to the Hedge Vault.
  - **Maturity**: If the event does not occur by a certain time, the Hedge Vault’s capital is released to the Risk Vault.

This established the basic mechanics of a **decentralized risk market**, where participants on both sides interact with immutable contracts instead of trusted intermediaries.

### 🌐 Acurast Oracle Integration

A key innovation of our Kickstarter phase was integrating **Acurast Oracles** into Soroban using **Trusted Execution Environments (TEEs)**.

- We wrote a **Node.js script** that runs inside an Acurast TEE processor.
- The script fetches **flight data** from the [FlightAware API](https://www.flightaware.com/commercial/aeroapi/) and forwards it to our smart contracts using `js-stellar-sdk`.
- To ensure authenticity, the controller only accepts signed messages from a whitelisted oracle address.

This allowed us to forward real-world data into Soroban **securely and without centralized servers**, aligning with the trust-minimization goals of the protocol.

[Read the Integration Article](#)


### 🛡️ Insurance Use Case: Flight Delay POC

We combined all the above components into a working **Flight Delay Insurance Proof of Concept**:

- Users deposit funds into Hedge or Risk Vaults to bet on whether a flight will be delayed beyond a fixed threshold (e.g., 3 hours).
- An oracle running on Acurast monitors the flight status and triggers contract logic when a delay is detected.
- If the delay occurs, the Risk Vault is liquidated, and capital flows to the Hedge Vault (paying the insured party).
- If no delay occurs by market maturity, the Hedge Vault’s capital goes to the Risk Vault.

This demonstrated that **parametric insurance** can be implemented on Soroban using decentralized, programmable primitives.


### 🧪 Manual Market Creation

To simplify the initial Proof of Concept, we **manually deployed** Hedge and Risk Vaults for the flight delay market.

- We did not yet implement the **Vault Factory**, which would allow anyone to permissionlessly create new insurance markets.
- For the Kickstarter phase, the Sentinel team acted as **market makers**, simulating how new markets would be launched in a more advanced version of the system.

This approach allowed us to keep the architecture minimal while validating the core logic behind the insurance lifecycle.

---

### 🔗 What’s Next: Build Award Focus

With core components working in isolation and in coordination, we are now focused on the following during the Build Award phase:

- Finalizing and testing the **Vault Factory** for dynamic market creation.
- Improving the **UI/UX** for both insurance buyers and risk counterparties.
- Onboarding **real users** for a **closed beta** of Flight Delay Insurance.
- Encouraging other teams to build **new insurance verticals** (e.g., crop, fire) on top of our framework.

You can view our technical progress and roadmap in more detail in the [Kickstart Progress & Build Award Plan](https://github.com/SentinelFi/build_35_submission/blob/main/technical.md).
