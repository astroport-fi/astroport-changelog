# ASTROPORT Launch : Deployment Guide

- <h2> Astroport Lockdrop + LBA Launch Guide </h2>
  <br>

  - **[ TESTNET ONLY ] Initialize ASTRO and 3rd party tokens with their terraswap Pools and mint LP tokens for Lockdrop**

    Requirements - Need to set Terraswap factory address and LUNA-UST Terraswap pair address in `astroport-periphery/artifacts/<Chain_ID>` before executing this script
    Command to execute on terminal, -

    ```
    node --loader ts-node/esm scripts/create_lockdrop_env.ts
    ```

    Addresses of the deployed tokens, their terraswap LP pairs and LP Pool tokens will be stored in `astroport-periphery//artifacts/<Chain_ID>`

    Optionally, you can sent testers some LP Tokens for testing. You can add the terra addresses of the testers in the `ADDRESSES` array in the script

    ```
    node --loader ts-node/esm scripts/transfer_tokens.ts
    ```

  - **Initialize Lockdrop, Airdrop, Auction Contracts, send them ASTRO tokens for incentives, and initialize LP pools on Lockdrop for deposits.**
    Requirements - Need to set token addresses in `astroport-periphery//artifacts/<Chain_ID>` before executing this script (for testnet, this will be done by the above script)
    Command to execute on terminal, -

    ```
    node --experimental-json-modules --loader ts-node/esm scripts/deploy_periphery_contracts.ts
    ```

    Function execution flow of this script -

    - deploys Lockdrop
    - deploys Airdrop
    - deploys Auction
    - Lockdrop::UpdateConfig : To set ASTRO, Auction addresses
    - Airdrop::UpdateConfig : To set Auction address
    - ASTRO::Send::Lockdrop::IncreaseAstroIncentives : Set Lockdrop incentives
    - ASTRO::Transfer : Set Airdrop incentives
    - ASTRO::Send::Auction::IncreaseAstroIncentives : Set Auction incentives
    - Lockdrop::InitPool : Initialize LUNA-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize LUNA-BLUNA Pool in Lockdrop
    - Lockdrop::InitPool : Initialize ANC-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize MIR-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize ORION-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize STT-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize VKR-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize MINE-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize PSI-UST Pool in Lockdrop
    - Lockdrop::InitPool : Initialize APOLLO-UST Pool in Lockdrop

      In case any error occurs while executing the above script, it can be re-executed and it will continue from where it previously faulted.

      Addresses of the deployed periphery contracts and certain variables describing which all functions have been called successfully can be found in `astroport-periphery//artifacts/<Chain_ID>.json`

  - **Deploy Astroport, probably near to Lockdrop : Phase-1 completion**
    Requirements - Need to set astro address in `astroport//artifacts/<Chain_ID>` before executing this script
    Command to execute in the `astroport` package terminal, -

    ```
        export WALLET="<mnemonic seed>"
        export LCD_CLIENT_URL="https://bombay-lcd.terra.dev"
        export CHAIN_ID="<Chain_ID>"
        npm run build-app
    ```

    Addresses of the Astroport's core contracts are stored in `astroport/artifacts/<Chain_ID>.json`

    Function execution flow of this script -

    - Register Pair (xyk and stable) Contracts
    - Deploy Vesting Contract
    - Deploy Staking Contract
    - Deploy Generator Contract
    - Setting tokens to Vesting Contract : assigning tokens to the generator
    - Deploy Factory Contract
    - Deploy Router Contract
    - Deploy Maker Contract

      In case any error occurs while executing the above script, it can be re-executed and it will continue from where it previously faulted.

      Addresses of the deployed astroport core contracts can be found in `astroport/artifacts/<Chain_ID>.json`

  - **Initialize Pools on Astroport to which liquidity is to be migrated**
    Requirements - Need to set Astroport factory address before executing this script
    Command to execute in `Astroport-periphery`'s terminal -

    ```
    node --loader ts-node/esm scripts/create_astroport_pools.ts
    ```

    In case any error occurs while executing the above script, it can be re-executed and it will continue from where it previously faulted.

    Addresses of the newly initialized pools on Astroport can be found in `astroport-periphery//artifacts/<Chain_ID>.json`

  - **Post Phase-1 completion : Migrating Liquidity to Astroport Pools on Astroport**
    Requirements - Need to set Astroport pool address before executing this script. Will be done by the above script automatically.
    Command to execute on terminal, -

    ```
    node --loader ts-node/esm scripts/migrate_liquidity_to_astroport.ts
    ```

    In case any error occurs while executing the above script, it can be re-executed and it will continue from where it previously faulted.
    <br>

  - **[ TESTNET ONLY ] Deploy 3rd party staking contracts for eligible tokens for dual incentives**
    <br>

  - **Deploy Proxy contracts for Astroport LP Tokens eligible for dual incentives**
    <br>

  - **Initialize ASTRO Rewards with the Generator for eligible Astroport LP Tokens**
    <br>

  - **Stake Astroport LP tokens with the Generator Contract**
    <br>

- Set environment variables

  For bombay testnet -

  ```bash
  export WALLET="<mnemonic seed>"
  export LCD_CLIENT_URL="https://bombay-lcd.terra.dev"
  export CHAIN_ID="<Chain_ID>"
  ```

  For mainnet -

  ```bash
  export WALLET="<mnemonic seed>"
  export LCD_CLIENT_URL="https://lcd.terra.dev"
  export CHAIN_ID="columbus-5"
  ```
