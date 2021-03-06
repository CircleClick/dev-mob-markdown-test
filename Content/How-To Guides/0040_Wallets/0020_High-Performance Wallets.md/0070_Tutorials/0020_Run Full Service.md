---
title: Run Full Service
---
Direct your Full Service API calls to localhost:9090/wallet.

## Open Your Account

### Create a New Account

1.  Call [`create_account`](/how-to-guides/wallets/high-performance-wallets/accounts/account/create-account) to open a new account.

2.  To protect yourself from ever losing your account, run [`export_account_secrets`](/how-to-guides/wallets/high-performance-wallets/accounts/account-secrets/export-account-secrets) to create a mnemonic that will allow you to recover your account.

(!) Creating a mnemonic is the only way to recover your account.

### Import an Existing Account

1.  If you already have an account, you can access it with the [import_account](/how-to-guides/wallets/high-performance-wallets/accounts/account/import-account) method.

    -   To identify your account, you must provide the method with your secret mnemonic and an account name if you have one. 

    -   To speed up the import process, you can provide the method with the first block index that you'd like to scan from the ledger. If you don’t include the first block index, it will default to scanning the entire ledger, which will take longer as the ledger size increases.

## MOB Transactions

To receive MOB, you must provide the sender with an account address.

When you create an account, the API response includes a `main_address` that you can share to receive funds. The `main_address` is a subaddress at index 0. You must know which subaddress your MOB was sent to in order to spend it. Limiting the number of subaddresses makes it simpler to keep track of your funds. Using a single address for multiple transactions will anonymize the senders and there will not be a way to verify the amount sent by each sender.

In order to track who is sending what payments, you can create unique subaddresses to share with a particular sender and or a particular transaction.

### Receive MOB

1.  Generate a subaddress to share with the sender by calling [`assign_address_for_account`](/how-to-guides/wallets/high-performance-wallets/accounts/address/assign-address-for-account).

2.  Call [`get_wallet_status`](/how-to-guides/wallets/high-performance-wallets/wallet/wallet-status/get-wallet-status) to view the `total_unspent_pmob` that you received in the transaction.

### Send MOB

1.  Review the initial balance of your account by calling [`get_balance_for_account`](/how-to-guides/wallets/high-performance-wallets/accounts/balance/get-balance-for-account) with your account_id.

2.  Since you are running a test that doesn't require you to review the tx_proposal before submitting it to the ledger, call the convenience method [`build_and_submit_transaction`](/how-to-guides/wallets/high-performance-wallets/transactions/transaction/build_and_submit_transaction) to send MOB to a public address.

3.  Verify whether the transaction was successful by calling the [`get_balance_for_account`](/how-to-guides/wallets/high-performance-wallets/accounts/balance/get-balance-for-account) endpoint again to compare the balance in your account before and after the transaction.

    -   If you sent MOB to your own account, subtract the unspent MOB transaction fee from your initial balance.

    -   If you sent MOB to someone else, subtract the unspent MOB transaction fee and the amount sent from your initial balance.