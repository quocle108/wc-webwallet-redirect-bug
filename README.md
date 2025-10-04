# WalletConnect Web Wallet Redirect Bug

## Overview
This repository documents a general issue with the WalletConnect SDK when used with **web-based wallets**.

- Only the **first transaction** in a multi-transaction flow includes a redirect URL.  
- Subsequent transactions are delivered via **WebSocket only**, making it impossible for web wallets to follow the [recommended flow in docs](https://docs.walletconnect.network/wallet-sdk/web/usage).  
- As a result, if the wallet tab is closed (per docs), the user never sees the second transaction.

## Reproduction
This can be reproduced on **any dApp** requiring multiple sequential transactions, such as Uniswap or OpenSea.

### Steps
1. Connect a web wallet (e.g., [NewWallet.io](https://newwallet.io)) to Uniswap.
2. Initiate a swap that requires 2 transactions (approval + swap).
3. First transaction includes a redirect URL ✅
4. User follows docs recommendation to close wallet tab.
5. Second transaction arrives via WebSocket only ❌
6. User never sees the second transaction.

### Video Demonstration
[Watch reproduction video](https://github.com/quocle108/wc-webwallet-redirect-bug/blob/main/reproduce-mutilple-txs-issue.MOV)

---

## Environment
- Wallet: [NewWallet.io](https://newwallet.io)
- SDK: `@walletconnect/core v2.14`
- dApp tested: [Uniswap](https://app.uniswap.org)
- OS: macOS
- Browser: Chrome v140.0.7339.208

---

## Impact
This breaks the UX for all web wallets following the official documentation. Multi-step flows (token swaps, NFT purchases, etc.) fail because redirect URLs are missing for transactions after the first.
