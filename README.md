What is this?
--------------

This is a stand-alone P2P transaction submission tool.

The motivation for this command is the `-walletbroadcast=0` command introduced
in Bitcoin Core 0.11 (see the [release notes](https://github.com/bitcoin/bitcoin/blob/v0.11.0rc1/doc/release-notes.md#privacy-disable-wallet-transaction-broadcast).)

Usage
--------

Usage:

    bitcoin-submittx <mainnet|testnet|regtest> TXHEX[,TXHEX...] NODE [NODE...]

- `<mainnet|testnet|regtest>` selects the network to use. This also determines the default port
- `TXHEX` is the serialized transaction, encoded as hexadecimal
- `NODE` is one or more node addresses (`host` or `host:port`) to submit the transaction to

The tool will connect to the provided nodes and inv the transactions. If the
nodes subsequently request them within the timeout (currently hardcoded to 10
seconds), they are sent.

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

TODO
------

- SOCKS5 proxy support (port from [test_framework/socks5.py](https://github.com/bitcoin/bitcoin/blob/master/qa/rpc-tests/test_framework/socks5.py))
- IPv6 support
- Provide feedback if the transaction is rejected

Dependencies
--------------

- [python-bitcoinlib](https://github.com/petertodd/python-bitcoinlib), also available via [pypi](https://pypi.python.org/pypi/python-bitcoinlib): `pip install python-bitcoinlib`

