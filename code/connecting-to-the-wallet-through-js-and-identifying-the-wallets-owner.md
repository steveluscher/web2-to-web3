# 🔌 Connecting to the wallet through JS and identifying the wallet's owner

{% hint style="success" %}
Contributors: [@rajkaria](https://github.com/rajkaria), [@kb24x7](https://github.com/kb24x7)
{% endhint %}

![](../.gitbook/assets/code-1.png)

1. User visits the website, at this point the wallet extension injects code into the window object so that the website's code can connect and interact with the wallet to get the wallet's details, perform actions, etc.
2. As soon as the user clicks the connect button, the JavaScript code in the front-end prompts the wallet to get an approval from the user to connect to that website.
3. If approved, the wallet returns the public key of that user for identification.

## Identifying the wallet

While this example only includes code for Phantom and SolFlare, the process is pretty similar for other wallets as well.

```javascript
// exportWallet.ts
export const getWallet = () => window.solana || window.solflare
```

Now that we have the wallet exported, we can import it into our other files for us to utilize it.

```javascript
// wallet.ts
import { getWallet } from './exportWallet.ts'

export const connectWallet = async () => {
    const wallet = getWallet()
    if (!wallet) {
      window.open("https://phantom.app", "_blank"); 
      return
    }
    const publicKey = await wallet.connect()
    return publicKey
}
```

See? It was that simple, now that we have the connect to wallet function ready, we can use the user's public key to save it to the DB and identify them as it will always be unique.

If there are no supported wallets detected, it automatically redirects the user to phantom's website.

## FAQs

Add questions you have for this topic below and create a merge request.
