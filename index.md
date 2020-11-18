<!-- Electrum documentation master file, created by
sphinx-quickstart on Fri Sep 18 14:24:02 2015.
You can adapt this file completely to your liking, but it should at least
contain the root `toctree` directive. -->
# Welcome to the Electrum Documentation!

Electrum is a lightweight Bitcoin wallet.

## GUI and beginners


* Frequently Asked Questions


    * How does Electrum work?


    * Does Electrum trust servers?


    * What is the seed?


    * How secure is the seed?


    * I have forgotten my password. What can I do?


    * My transaction has been unconfirmed for a long time. What can I do?


    * What does it mean to “freeze” an address in Electrum?


    * How is the wallet encrypted?


    * Does Electrum support cold wallets?


    * Can I import private keys from other Bitcoin clients?


    * Can I sweep private keys from other Bitcoin clients?


    * Where is the Electrum datadir located?


    * Where is my wallet file located?


    * How to enable debug logging?


    * Can I do bulk payments with Electrum?


    * Can Electrum create and sign raw transactions?


    * Electrum freezes when I try to send bitcoins.


    * What is the gap limit?


    * How can I pre-generate new addresses?


    * How do I upgrade Electrum?


    * My anti-virus has flagged Electrum as malware! What now?


    * Electrum requires recent Python. My Linux distribution does not yet have it. What now?


    * I might run my own server. Are client-server connections authenticated?


* Invoices


* Multisig Wallets


    * Create a pair of 2-of-2 wallets


    * Receiving


    * Spending


* Cold Storage


    * Create an offline wallet


    * Create a watching-only version of your wallet


    * Create an unsigned transaction


    * Get your transaction signed


    * Broadcast your transaction


* Hardware wallets on Linux


    * 1. Dependencies


    * 2. Python libraries


    * 3. udev rules


    * 4. Done


* Using the most current Electrum on Tails


    * Steps to use AppImage


## 3 - Keys


* 3 - Keys


* Implementation details


    * Transactions


    * Wallet local storage


    * Wallets


## Advanced users


* Using cold storage with the command line


    * Create an unsigned transaction


    * Sign the transaction


    * Broadcast the transaction


* How to split your coins using Electrum in case of a fork


    * Note:


    * What is a fork?


    * What does it mean to ‘split your coins’?


    * Fork detection


    * Procedure


* Using Electrum Through Tor


    * List of Active .onion servers


    * LINUX


    * Option 1: Single Server


    * Option 2: Multiple servers but Tor Main


    * WINDOWS


    * Option 1: Connecting to a single Server


    * Option 2


* Verifying GPG signature of Electrum using Linux command line


    * Obtain public GPG key for ThomasV


    * Download Electrum and signature file (asc)


    * Verify GPG signature


## Daemon and Command Line


* Command Line


    * Using the inline help


    * How to use the daemon


    * Magic words


    * Aliases


    * Formatting outputs using jq


    * Examples


* How to configure SSL with Electrum


    * Add your SSL private key


    * Add your SSL certificate bundle


    * Check that your certificate was accepted by Electrum


* How to accept Bitcoin on a website using Electrum


    * Add your SSL certificate to Electrum


    * Create and use your merchant wallet


    * Start the Electrum daemon


    * Create a signed payment request


    * Open the payment request page in your browser


    * Lightning payments


* How to setup a watchtower


    * Local and remote watchtower


    * How to configure a local watchtower


    * How to configure a remote watchtower


    * Configure the watchtower in your client


* JSONRPC interface


## For developers


* The Python Console


* Simple Payment Verification


* Electrum Seed Version System


    * Description


    * Motivation


    * Version number


    * List of reserved numbers


    * Seed generation


    * Security implications


* Electrum protocol specification


    * Format


    * Methods


    * External links


* Serialization of unsigned or partially signed transactions


    * Extended public keys


    * Public key


    * BIP32 derivation


    * Legacy Electrum Derivation


    * Bitcoin address
