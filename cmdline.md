# Command Line

Electrum has a powerful command line. This page will show you a few basic principles.

Note that this page has been updated for Electrum 4.0.

## Using the inline help

To see the list of Electrum commands, type:

```
electrum help
```

To see the documentation for a command, type:

```
electrum help <command>
```

## How to use the daemon

By default, commands are sent to an Electrum daemon.
Here is how to start and stop the daemon:

```
electrum daemon -d
electrum getinfo
electrum stop
```

Some commands require a wallet. Here is how to load a wallet in the daemon:

```
electrum load_wallet  # this will load the default wallet
electrum load_wallet -w /path/to/wallet/file
electrum list_wallets
```

Once the wallet is loaded, wallet operations are possible, such as:

```
electrum listaddresses
electrum payto <address> <amount>
```

Some commands do not require network access, and can be executed without a running daemon.
This is done with the –offline flag:

```
electrum -o listaddresses
electrum -o payto <address> <amount>
electrum -o -w /path/to/wallet/file listaddresses
```

## Magic words

The arguments passed to commands may be one of the following magic words: ! ? : and -.


* The exclamation mark ! is a shortcut that means ‘the maximum amount
available’.

Example:

```
electrum payto 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE !
```

Note that the transaction fee will be computed and deducted from the
amount.


* A question mark ? means that you want the parameter to be prompted.

Example:

```
electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE ?
```


* Use a colon : if you want the prompted parameter to be hidden (not
echoed in your terminal).

```
electrum importprivkey :
```

Note that you will be prompted twice in this example, first for the
private key, then for your wallet password.


* A parameter replaced by a dash - will be read from standard input
(in a pipe)

```
cat LICENCE | electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -
```

## Aliases

You can use DNS aliases in place of bitcoin addresses, in most
commands.

```
electrum payto ecdsa.net !
```

## Formatting outputs using jq

Command outputs are either simple strings or json structured data. A
very useful utility is the ‘jq’ program.  Install it with:

```
sudo apt-get install jq
```

The following examples use it.

## Examples

### Sign and verify message

We may use a variable to store the signature, and verify
it:

```
sig=$(cat LICENCE| electrum signmessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE -)
```

And:

```
cat LICENCE | electrum verifymessage 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE $sig -
```

### Show the values of your unspents

The ‘listunspent’ command returns a list of dict objects,
with various fields. Suppose we want to extract the ‘value’
field of each record. This can be achieved with the jq
command:

```
electrum listunspent | jq 'map(.value)'
```

### Select only incoming transactions from history

Incoming transactions have a positive ‘value’ field

```
electrum history | jq '.[] | select(.value>0)'
```

### Filter transactions by date

The following command selects transactions that were
timestamped after a given date:

```
after=$(date -d '07/01/2015' +"%s")

electrum history | jq --arg after $after '.[] | select(.timestamp>($after|tonumber))'
```

Similarly, we may export transactions for a given time
period:

```
before=$(date -d '08/01/2015' +"%s")

after=$(date -d '07/01/2015' +"%s")

electrum history | jq --arg before $before --arg after $after '.[] | select(.timestamp&gt;($after|tonumber) and .timestamp&lt;($before|tonumber))'
```

### Encrypt and decrypt messages

First we need the public key of a wallet address:

```
pk=$(electrum getpubkeys 1JuiT4dM65d8vBt8qUYamnDmAMJ4MjjxRE| jq -r '.[0]')
```

Encrypt:

```
cat | electrum encrypt $pk -
```

Decrypt:

```
electrum decrypt $pk ?
```

Note: this command will prompt for the encrypted message, then for the
wallet password

### Export private keys and sweep coins

The following command will export the private keys of all wallet
addresses that hold some bitcoins:

```
electrum listaddresses --funded | electrum getprivatekeys -
```

This will return a list of lists of private keys. In most
cases, you want to get a simple list. This can be done by
adding a jq filer, as follows:

```
electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])'
```

Finally, let us use this list of private keys as input to the sweep
command:

```
electrum listaddresses --funded | electrum getprivatekeys - | jq 'map(.[0])' | electrum sweep - [destination address]
```
