# Using cold storage with the command line

This page will show you how to sign a transaction with
an offline Electrum wallet, using the Command line.

## Create an unsigned transaction

With your online (watching-only) wallet, create an
unsigned transaction:

```
electrum payto 1Cpf9zb5Rm5Z5qmmGezn6ERxFWvwuZ6UCx 0.1 --unsigned > unsigned.txn
```

The unsigned transaction is stored in a file named ‘unsigned.txn’.
Note that the –unsigned option is not needed if you use a
watching-only wallet.

You may view it using:

```
cat unsigned.txn | electrum deserialize -
```

## Sign the transaction

The serialization format of Electrum contains the master
public key needed and key derivation, used by the offline
wallet to sign the transaction.

Thus we only need to pass the serialized transaction to
the offline wallet:

```
cat unsigned.txn | electrum signtransaction - > signed.txn
```

The command will ask for your password, and save the
signed transaction in ‘signed.txn’

## Broadcast the transaction

Send your transaction to the Bitcoin network, using broadcast:

```
cat signed.txn | electrum broadcast -
```

If successful, the command will return the ID of the
transaction.
