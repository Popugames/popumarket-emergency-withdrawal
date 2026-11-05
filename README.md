# PopuMarket emergency withdrawal

[Citește în română](GHID-RO.md)

This guide explains how to take your TCL directly from the PopuMarket contract, without the PopuMarket website or servers.

## Before you need it

You can use this method only if you linked your wallet while PopuMarket was working. Do this now:

1. In PopuMarket, open the TCL withdrawal page.
2. Connect your wallet and select **Link wallet**. If your account has a wallet linked in the game, you must use that wallet.
3. Save your **account ID**, shown in the same section.

## What you need

- The linked wallet
- A small amount of EGLD for network fees
- Your account ID, in hexadecimal

**If you didn't save your account ID:** find your `bindWallet` transaction in the [MultiversX Explorer](https://explorer.multiversx.com). Under **Logs**, open the `wallet_bound` event. The account ID is the second topic.

## Take your TCL

1. Open [wallet.multiversx.com](https://wallet.multiversx.com) and connect the linked wallet.
2. Select **Send** and enter:
   - **To:** `erd1qqqqqqqqqqqqqpgq65t4mhrg4g8ymc83n4rhrwpa6x9uvsf60ufsfgcu9z`
   - **Amount:** `0`
   - **Gas limit:** `30000000`
   - **Data:** `requestEmergencyWithdrawal@<ACCOUNT_ID>`
3. Wait 24 hours.
4. Send a second transaction with the same details, except for the data:
   - **Data:** `executeEmergencyWithdrawal@<ACCOUNT_ID>`

The whole balance of the account goes to the linked wallet. To change your mind before step 4, send `cancelEmergencyWithdrawal@<ACCOUNT_ID>` instead.

While PopuMarket is working, your request also cancels your open orders and offers that hold TCL, and what they hold returns to your balance. You can't spend or withdraw TCL on the market until you cancel the request or collect.

## Contract details

| | |
|---|---|
| Network | MultiversX mainnet |
| Contract | `erd1qqqqqqqqqqqqqpgq65t4mhrg4g8ymc83n4rhrwpa6x9uvsf60ufsfgcu9z` |
| Owner (multisig, 2 of 3) | `erd1qqqqqqqqqqqqqpgqprl0xalqslxq9rh6qlg40mz9c6m3z6wg0ufszvw3j4` |
| Token | `TCL-fe459d` |

## Functions

| Function | Who can call it | What it does |
|---|---|---|
| `bindWallet` | The player, through the website | Links a wallet to the account. Needs a ticket signed by the PopuMarket server. |
| `requestEmergencyWithdrawal@<ACCOUNT_ID>` | Linked wallet | Starts the 24-hour wait and blocks debits from the account after 10 minutes |
| `cancelEmergencyWithdrawal@<ACCOUNT_ID>` | Linked wallet | Cancels the request |
| `executeEmergencyWithdrawal@<ACCOUNT_ID>` | Linked wallet | After 24 hours, sends the whole account balance to the linked wallet |
| `proposeWalletRecovery` | Owner (multisig) | Proposes a new wallet for an account, with a 7-day wait |
| `cancelWalletRecovery@<ACCOUNT_ID>` | Linked wallet or owner | Cancels the proposed change |
| `finalizeWalletRecovery@<ACCOUNT_ID>` | Anyone, after 7 days | Applies the change, unless an emergency withdrawal is pending |
| `getAccountWallet`, `getBalance`, `getEmergencyUnlock`, `getWalletRecovery` | Anyone (read only) | Show the linked wallet, the balance, the unlock time and any proposed wallet change |

## What this does and doesn't protect

- **After you request:** the guardian, the owner and a contract pause can't cancel the request or the payout. The operator can't debit the account starting 10 minutes after the request, so operations already in progress can finish.
- **Before you request and during those 10 minutes:** the PopuMarket operator can still execute operations on your balance.
- **Contract updates:** the multisig owner can update the contract code.
- **Lost wallet:** only the multisig can link a new wallet. The change is public for 7 days and the current wallet can cancel it.
- **No linked wallet:** this method isn't available.
- **Stolen wallet:** whoever controls the linked wallet can request the withdrawal.

This guide is technical information. It isn't financial advice or a promise of any token value.
