# 💸 Sending SOLs through browser JavaScript

Now that we have figured out how to connect the wallet to our website. Let's see how we can create and sign a transaction through the wallets.

Prerequisites:

```
yarn add @solana/web3.js
```

OR

```
npm i @solana/web3.js
```

Let's import some constructors from this module:

```
// wallet.ts

import {
  Connection,
  Transaction,
  SystemProgram,
  PublicKey
} from '@solana/web3.js';
```

If you're coming from the last article, you know that we have a file that exports the user's wallet and a method that connects to the wallet in this file itself, if you've missed that I highly recommend that you check out the last article. So let's import the getWallet function in this file.

```
import { getWallet } from './exportWallet.ts'
```

We now have everything we need, let's create a function to send SOLs.

```
export const sendMoney = async (to: PublicKey, amount: number) => {
  const wallet = getWallet()
  const sender = connectWallet()
  const environment = "mainnet-beta"
  const network = `https://api.${environment}.solana.com`

  // This method connects to the mainnet-beta solana network,
  // you can change it to devnet as well.

  const connection = new Connection(network)
  
  // Let's construct the transaction here
  const transaction = new Transaction()
    .add(
      SystemProgram.transfer({
        fromPubkey: sender,
        toPubkey: to,
        lamports: amount * 1000000000,
      })
    );

 // These are the transaction instructions,
 // 1 SOL = 1000000000 LAMPs
  
  const { blockhash } = await connection.getRecentBlockhash()
  transaction.recentBlockhash = blockhash
  transaction.feePayer = publicKey
  
 // This transaction needs to be signed by the user before it can
 // be sent
  let signedTransaction = await wallet.signTransaction(transaction)

  // Let's send this transaction to the network now

  try {
    const txid = await connection.sendRawTransaction(
      signedTransaction.serialize()
    )
    return txid;
  catch (error) {
    console.error(error)
  }
  
}
```

Now we can utilize this function anywhere in our working directory. There are

```
import { sendMoney } from './wallet.ts'
import { PublicKey } from '@solana/web3.js'

async function main() {
  const receiver = new PublicKey("....") // Receiver's Address
  const txid = await sendMoney(receiver, 1) // Send 1 SOL
}
```