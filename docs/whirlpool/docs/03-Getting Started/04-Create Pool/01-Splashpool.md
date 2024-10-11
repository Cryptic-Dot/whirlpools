# Splash Pool

Creating a Splash Pool is the easiest way to launch your token on Orca. You only need to provide the mint addresses of 2 tokens and the initial price. Easy as that!

## Function overview

- **Inputs**:

    - `rpc`: A Solana RPC client used to communicate with the blockchain.
    - `tokenMintOne` and `tokenMintTwo`: Addresses of the two token mints that will make up the liquidity pool. Selecting which of the two tokens will be token 1 and token 2 matters for the price you are going to set. In most cases, you select your token as token 1 and select SOL/USDC/USDT as token 2.
    - `initialPrice`: The initial price between the two tokens. You express the value of token 1 in terms of token 2.
    - `funder`: The account funding the initialization process.

- **Outputs**: The function returns a promise resolving to an object containing:

    - `instructions`: A list of instructions required to initialize the pool.
    - `initializationCost`: The minimum balance required for [rent](https://solana.com/docs/core/fees#rent) exemption, in lamports.
    - `poolAddress`: The address of the created pool.

## Basic usage

```tsx title="main.ts"
import { createSplashPoolInstructions } from '@orca-so/whirlpools-sdk'
import { Connection, clusterApiUrl } from '@solana/web3.js';
import { getWallet } from './wallet';
import { airdropSolIfNeeded } from './airdrop';

async function main() {
  const connection = new Connection(clusterApiUrl('devnet'));
  const wallet = getWallet();
  await airdropSolIfNeeded(connection, wallet);

  const tokenMintOne = "TOKEN_MINT_ADDRESS_1"; // Replace with actual mint address
  const tokenMintTwo = "TOKEN_MINT_ADDRESS_2"; // Replace with actual mint address
  const initialPrice = 0.01;  // Example initial price

  // Generate the instructions to create the Splash Pool
  const poolInstructions = await createSplashPoolInstructions(
    connection,
    tokenMintOne,
    tokenMintTwo,
    initialPrice,
    wallet
  );

  // Log the pool address and instructions
  console.log("Pool Address:", poolInstructions.poolAddress);
  console.log("Initialization Instructions:", poolInstructions.instructions);
  console.log("Initialization Cost (lamports):", poolInstructions.initializationCost);
}

main()
```

## Next Steps

After creating a Splash Pool, the pool is still empty and requires liquidity for people to trade against.

To do this, you’ll need to open a position and deposit both tokens into the pool at a ratio that is equal the current price. Luckily, our SDK takes care of that in 1 simple step: [Open a Position](../03-Provide%20Liquidity/01-Open%20Position.md). By providing liquidity, you enable trades between the two tokens and start earning fees.