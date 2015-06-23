What is this?
--------------

This is a stand-alone P2P transaction submission tool.

Usage
--------

Usage:

    bitcoin-submittx <mainnet|testnet|regtest> TXHEX[,TXHEX...] NODE [NODE...]

- `TXHEX` is the serialized transaction, encoded as hexadecimal
- `NODE` is one or more node addresses (`host` or `host:port`) to submit the transaction to

TODO
------

- SOCKS5 proxy support (port from [test_framework/socks5.py](https://github.com/bitcoin/bitcoin/blob/master/qa/rpc-tests/test_framework/socks5.py))
- IPv6 support

Dependencies
--------------

- [python-bitcoinlib](https://github.com/petertodd/python-bitcoinlib)

