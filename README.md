# ✅ STEP-BY-STEP GUIDE TO PRE-SIGN AND BROADCAST TX IN PRESALES

This script lets you pre-sign a **Type 0 (legacy)** transaction to interact with `buyTokens()` function on the Arbitrum network. You’ll get a raw signed transaction that you can instantly broadcast during the presale — no delay from wallet popups.

> ⚠️ This is for **power users**. Use at your own risk and never share your private key.

---

## 🚀 What It Does

✅ Uses Type 0 (non-EIP-1559) transaction  
✅ Interacts with `buyTokens(uint256 amount, address beneficiary)`  
✅ Prepares a raw signed transaction  
✅ Instant broadcast 

---

## 🧰 Tools You’ll Need:
-Node.js (installed: https://nodejs.org)

-Metamask (for your private key export)

-VS Code or any code editor

-A terminal

Check installation:

```bash
node -v
npm -v
```
---
## 🧱 Setup a Node.js Project
```bash
mkdir SignTx
cd SignTx
npm init -y
npm install ethers
```
---

## 📁 Create Node file
```bash
notepad signTx.js

```
Now paste this code and save file

```
const { ethers } = require("ethers");

// ⚠️ Replace with your private key (NEVER share or commit this!)
const privateKey = "0xYOUR_PRIVATE_KEY_HERE";

// 🛰 Arbitrum RPC
const provider = new ethers.providers.JsonRpcProvider("CUSTOM_RPC");

// 🔐 Setup wallet
const wallet = new ethers.Wallet(privateKey, provider);

// 🏦 Presale contract
const contractAddress = "Peoject's_Contract_here";

// 🎯 Transaction details
const amountETH = "0.01"; // Amount you're contributing
const beneficiary = "0xYOUR_WALLET_ADDRESS_HERE"; // Your wallet address

(async () => {
  try {
    const nonce = await provider.getTransactionCount(wallet.address);
    const gasPrice = ethers.utils.parseUnits("2", "gwei"); // Increase if stuck
    const gasLimit = 150000;

  const iface = new ethers.utils.Interface([
      "function buyTokens(address beneficiary)"
    ]);

    const data = iface.encodeFunctionData("buyTokens", [
      //ethers.utils.parseEther(amountETH),
      beneficiary
    ]);

    const tx = {
      nonce,
      gasPrice,
      gasLimit,
      to: contractAddress,
      value: ethers.utils.parseEther(amountETH),
      data,
      chainId: 42161,
      type: 0 // Legacy transaction
    };

    const signedTx = await wallet.signTransaction(tx);
    console.log("\n✅ Signed Transaction Hex:");
    console.log(signedTx);

    const sentTx = await provider.sendTransaction(signedTx);
    console.log("\n📤 Broadcasted! TX Hash:", sentTx.hash);
    console.log("🔗 https://arbiscan.io/tx/" + sentTx.hash);
  } catch (err) {
    console.error("❌ Error broadcasting TX:", err);
  }
})();

```
| Placeholder                    | Replace With                                               |
| ------------------------------ | ---------------------------------------------------------- |
| `0xYOUR_PRIVATE_KEY_HERE`      | Your actual  private key                                   |
| `CUSTOM_RPC`                   | Or a custom Arbitrum RPC URL from Alchemy, Infura, etc.    |
| `0xCONTRACT_ADDRESS_HERE`      | The actual presale contract address                        |
| `"0.01"`                       | Amount you want to send                                    |
| `0xYOUR_WALLET_ADDRESS_HERE`   | Your  wallet address                                       |

---

## 🚀 Run the Script

```bash
node signTx.js
```

