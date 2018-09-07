Ubuntu Version: 17.10 desktop image

download from site [http://releases.ubuntu.com/17.10.1/]

Install Bitcoin
---------------------

```bash
./autogen.sh
./configure
make
make install
```

Creating a Configuration File
------------------------------------------
In your home user directory

```bash
mkdir .bitcoin
nano .bitcoin/bitcoin.conf
```

Define parameters

    rpcuser=bitcoinrpc
    rpcpassword=123456

Press Ctrl+X, Y, Enter to save the file.


Starting the Bitcoin Daemon
----------------------------------------------
```bash
bitcoind -regtest -daemon
netstat -pant
```

Blockchain Client Operations
----------------------------------------------
```bash
bitcoin-cli -regtest getblockchaininfo
bitcoin-cli -regtest getwalletinfo
```

Getting a New Address
```bash
bitcoin-cli -regtest getnewaddress
```

Mining a Block
```bash
bitcoin-cli -regtest generate 1
bitcoin-cli -regtest getblockchaininfo
```

Listing Transactions and Balance
```bash
bitcoin-cli -regtest listtransactions
bitcoin-cli -regtest generate 100
bitcoin-cli -regtest getbalance
```

Viewing Your Unspent Bitcoins
```bash
bitcoin-cli -regtest listunspent
```

Preparing Other Ubuntu
----------------------------------------------
Install the Bitcoin

Add node info in configuration file:

    rpcuser=bitcoinrpc
    rpcpassword=wqfgewgewi
    addnode=192.168.3.36 //your first node IP address

Starting Bitcoin Client
```bash
bitcoind -regtest -daemon
```

Stopping Bitcoin Client
```bash
ps -ef | grep bitcoind
kill pid
```

```bash
bitcoin-cli -regtest getpeerinfo
```

Reset Bitcoin Private Network: delete the regtest directory in the Bitcoin Core datadir:

Linux: ~/.bitcoin/regtest

Windows: %appdata%\bitcoin\regtest

MacOS: $HOME/Library/Application Support/Bitcoin/regtest

Sending Some Coins from Node1 to Node2
----------------------------------------------
In your Node2, execute

    bitcoin-cli -regtest getnewaddress

You could see the new address: 2NFRHUCVyMSoj2vS3nScbyN5gshvUWee5Ze

Or you could just dump the wallet and choose a address in your wallet though commands

    bitcoin-cli -regtest dumpwallet wallet.txt

In your Node1, execute

    bitcoin-cli -regtest sendtoaddress 2NFRHUCVyMSoj2vS3nScbyN5gshvUWee5Ze 20

  Mining another 100 blocks

    bitcoin-cli -regtest generate 100

  Check your Balance

    bitcoin-cli -regtest getbalance
