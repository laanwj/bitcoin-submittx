What is this?
--------------

This is a stand-alone P2P transaction submission tool.

The motivation for this command is the `-walletbroadcast=0` command introduced
in Bitcoin Core 0.11 (see the [release notes](https://github.com/bitcoin/bitcoin/blob/v0.11.0rc1/doc/release-notes.md#privacy-disable-wallet-transaction-broadcast).)

Usage
--------

Usage:

    usage: bitcoin-submittx [-h] [--proxy PROXY] [--timeout TIMEOUT] [--no-color]
                            [--nodes-file NODES_FILE] [--tx-file TX_FILE]
                            NETWORK TXHEX [NODES [NODES ...]]

    Transaction submission tool

    positional arguments:
      NETWORK               Network to connect to (mainnet, regtest, testnet).
                            This also determines the default port
      TXHEX                 Serialized transactions to broadcast, separated by
                            commas
      NODES                 Nodes to connect to, denoted either host or host:port

    optional arguments:
      -h, --help            show this help message and exit
      --proxy PROXY, -p PROXY
                            SOCKS5 proxy to connect through
      --timeout TIMEOUT, -t TIMEOUT
                            Number of seconds to wait before disconnecting from
                            nodes (default is 10)
      --no-color            Use no terminal color in output
      --nodes-file NODES_FILE, -n NODES_FILE
                            Read list of nodes from file (format: one per line)
      --tx-file TX_FILE, -r TX_FILE
                            Read list of transactions from file (format: one per
                            line)

The tool will connect to the provided nodes and announce the transactions. If the
nodes subsequently request them within the timeout, they are sent.

The return status of the program will be 0 if at least one node requested the transaction, and 1
otherwise.

Example w/ Bitcoin Core
-------------------------

- Send the transaction as normal, either e.g. through RPC `sendtoaddress` or the GUI
```
$ bitcoin-cli sendtoaddress mjqhocRebTHZRhkbkQs8Uzzb1T3GhEvEB4 0.25
f91948e5...
```
- Using the transaction hash, retrieve the transaction data through RPC using `gettransaction` (NOT
  `getrawtransaction`). The `hex` field of the result will contain the raw
  hexadecimal representation of the transaction
```
$ bitcoin-cli gettransaction f91948e5...
{
  ...
  "txid": "f91948e5...",
  ...
  "hex": "010000000..."
}
```
- Provide the `hex` field as TXHEX to this tool. e.g.
```
$ bitcoin-submittx mainnet '010000000...' 127.0.0.1
```
(normally one would not submit the transaction to the localhost node, but this is just an illustrative example)

Segwit support
---------------

As of 2017-11-30, this requires the master version of python-bitcoinlib.
Version 0.8 fails with `bitcoin.core.serialize.DeserializationExtraDataError: Not all bytes consumed during deserialization`.

A known issue is that bitcoin-submittx invs the wtxid instead of the txid, but
when it sends the tx the node accepts it. This is probably not correct per
BIP141. Patches welcome!

Fork support
-------------

`bitcoin-submittx`

- can be used for BCH by passing BCH nodes, and using the `mainnet` network parameter.

- can be used for BTG by passing BTG nodes, and using the `btg-mainnet` network parameter.

- can be used for BCX by passing BCX nodes, and using the `bcx-mainnet` network parameter.

You can find nodes for the appropriate fork on [https://bitnodes.earn.com/](bitnodes), or by manually querying their DNS seed.

TODOs and contribution ideas
-----------------------------

- Automatically fetch list of nodes to submit to
  - possibly from DNS seeds (but this won't work behind Tor)
- IPv6 support
- Provide feedback if the transaction is rejected, apart from
```
   recv msg_reject(messsage=tx, ccode=@, reason=non-final)
```
- Tor stream isolation (like `-proxyrandomize` in Bitcoin Core)
- Multi-hop proxies, different proxy types?
- Feature to handle incoming connections: can be handy when submitting transactions to nodes that are not listening for
  e.g. testing

Dependencies
--------------

- [python-bitcoinlib](https://github.com/petertodd/python-bitcoinlib), also available via [pypi](https://pypi.python.org/pypi/python-bitcoinlib): `pip install python-bitcoinlib`

Other projects
---------------

Some other projects that might be useful when working with bitcoin transactions at a low level:

- [payment-proto-interface](https://github.com/achow101/payment-proto-interface) This is a simple (Python) application which allows you to interact with the Bitcoin payment protocol manually
