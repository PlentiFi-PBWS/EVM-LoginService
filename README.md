# PlentiFi - Login Service

## Description
This is a simple login service that allows users to login authorized users to get a signature which grants them a free deployment and transaction sponsoring on PlentiFi. The service is built using broker.

## Installation
1. Clone the repository
2. Run `npm install` to install the dependencies
3. Create a `.env` file and add the same environment variables as in the `.env.example` file
4. Run `npm start` to start the service

## Live contracts addresses 📂

Here is the listing of all the deployed contracts on the XRPL-EVM Sidechain testnet: 

- Account abstraction contracts:
  - Entry Point: ``         deploy tx: []()
  - WebAuthn verifier: ``         deploy tx: []()
  - WebAuthn Account Factory: ``         deploy tx: []()
- The Automated Marked Maker (AMM) contract used for the demo: (mock)
  - AMM: ``         deploy tx: []()
- Some ERC20 deployed and used for the demo:
  - WBTC: ``         deploy tx: []()
  - USDT: ``         deploy tx: []()
  - APPL: ``         deploy tx: []()
  - BRENT: ``         deploy tx: []()
  - GOLD: ``         deploy tx: []()
  - IMMO: ``         deploy tx: []()
  - US BOND: ``         deploy tx: []()

## Account Abstraction in PlentiFi 

PlentiFi takes advantage of the Account Abstraction feature to provide a clean user experience: **no need to worry about gas fees or managing your private keys**. The Account Abstraction feature allows users to interact with the blockchain without having to pay gas fees. Instead, the dApp pays the fees on behalf of the user. This feature is enabled by the use of a bundler and entryPoint, which are responsible for paying the gas fees on behalf of the user. The bundler is a smart contract that collects gas fees from the dApp and pays them to the network, while the entryPoint is a smart contract that interacts with the bundler to pay the gas fees.

## Account Abstraction setup 🛠️

Since there is no bundler and entryPoint on the XRPL-EVM Sidechain, we initiated a redeployment of all essential components. This strategic approach was taken to guarantee a user experience on par with that of other blockchains equipped with bundlers and entryPoint. The following steps were taken to achieve this:

1. **Entry Point**: The entryPoint was modified to remove the dependency on the bundler. Our contracts evolved in way that we didn't actually need it anymore. We kept it so future hackers can use it to setup their own account abstraction mechanism.
2. **Bundler**: We created a simple bundler to broadcast user operations to the network. It can also act as a paymaster for the user. 
3. **Factory Contracts**: The contracts were modified to remove the dependency on the bundler and entryPoint. The contracts were deployed using the Hardhat framework. Anyone could use our [smart account factory contract](https://github.com/PlentiFi-PBWS/Contracts/blob/main/src/Accounts/WebAuthnAccountFactory.sol) as reference to deploy their own smart Account Factory on The evm sidechain.


**The process in a nutshell:**
- the user creates an account through the factory contract (sponsored or not by the app)
- the user create and sign its operation
- the operation is sent to the bundler
- the bundler broadcast the operation to the network
- the operation is executed