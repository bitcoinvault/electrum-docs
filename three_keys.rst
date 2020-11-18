.. role:: python(code)
    :language: python

********
3 - Keys
********

3-Keys mechanism, which aims to increase safety in Bitcoin transactions, is implemented in BitcoinVault coin.
Such approach introduces into blockchain ecosystem "Secure", "Cancel" and "Secure Fast" transactions. General speaking
"Secure" transaction are able to be reverted from chain by so called "Cancel" transaction. Moreover "Secure Fast"
transaction behaves like the standard one. More technical details are presented in
`https://docs.google.com/document/d/1P1mrlpOEwJO2A6qRbSV-zT5I041N6y9nH8NwJkbTxTY <https://docs.google.com/document/d/1P1mrlpOEwJO2A6qRbSV-zT5I041N6y9nH8NwJkbTxTY>`_
however comprehensive user manual is available on
`https://bitcoinvault.global/pdf/testnet3_0/Beta_1_0_Testnet_Gold_Wallet_Tutorial_DESKTOP_EN.pdf <https://bitcoinvault.global/pdf/testnet3_0/Beta_1_0_Testnet_Gold_Wallet_Tutorial_DESKTOP_EN.pdf>`_

**********************
Implementation details
**********************

Implementation of 3-keys functionality in ElectrumValut (EV) required some adjustment
in source code. The crucial changes was made in classes which represent wallets,
transactions and local storage.

Transactions
============
3-Key wallets make only segwit type of transactions, which are labeled as :python:`p2wsh-p2sh`.
3-Key transactions introduced transaction type, which identifies received and sent transaction.
There are 5 different types, listed below.

Transaction types
-----------------

.. code-block:: python

    class TxType(IntEnum):
        NONVAULT = 0
        ALERT_PENDING = 1
        ALERT_RECOVERED = 2
        RECOVERY = 3
        INSTANT = 4
        ALERT_CONFIRMED = 5

All non 3-key transactions are labeled as :python:`NONVAULT`. :python:`ALERT_PENDING` is called "Secure" transaction,
which becomes :python:`ALERT_CONFIRMED` after 144 confirmations. Before changing transaction status from
:python:`ALERT_PENDING` to :python:`ALERT_CONFIRMED` user can cancel transaction. Then :python:`ALERT_PENDING` becomes
:python:`ALERT_RECOVERED` and new transaction, called "Cancel", turns up on chain. The "Cancel" transaction is
:python:`RECOVERY` type. In 3-keys ecosystem user can perform transaction like :python:`NONVAULT`, type. They are called
"Secure fast" transactions and are labeled as :python:`INSTANT` type.

Wallet local storage
====================
Transactions types are reflected in wallet local storage. Storage is handled by :python:`json_db.py` module.
Type of the transaction was added to `addr_history` items, like:

.. code-block:: json

    {
        "addr_history": {
            "rtroyale1qeuj5qd44f80x0568pde44ajr0f0r7q47fd0s09": [
                [
                    "2f210d9455729b4599c0c7e8fe817e5ff63ff9bf99d5c24c4eacdcb52cdcc154",
                    102,
                    "NONVAULT"
                ]
            ],

and to `verified_tx3` items, like:

.. code-block:: json

    {
        "verified_tx3": {
            "2f210d9455729b4599c0c7e8fe817e5ff63ff9bf99d5c24c4eacdcb52cdcc154": [
                102,
                1605504388,
                1,
                "2b24949582d723c5cbc853ec71a4659870ede15fa42d472f2f169c0e60feea92",
                0
            ],

where :python:`0` stands for :python:`NONVALUT` transaction.

Wallets
=======
Main changes, such as preparing and signing transactions, was made in wallet classes. There were added new classes,
such as :python:`class MultikeyWallet(Simple_Deterministic_Wallet)`, :python:`class TwoKeysWallet(MultikeyWallet)`,
:python:`class ThreeKeysWallet(MultikeyWallet)` in :python:`wallet.py` module.
