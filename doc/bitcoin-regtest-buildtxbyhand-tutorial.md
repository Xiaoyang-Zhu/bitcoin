# Building Our Identity Transactions by Hand in Bitcoin

## Preliminaries

Following our previous "bitcoin-regtest-addresstokeys" tutuorial, we got testnet info:

##### Offline key pair: `<sk_f>, <pk_f>, <identifier>`

`<pk_f>` : 03c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a5244

`<sk_f>` : 92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz


`identifier`: mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4

`identifier(hex)`: 2abb1f98b76bdc4380138c09040a5838b6847f12

#### Online key pair `<sk_o>, <pk_o>, <address>`;

`<pk_o>` : 03a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f4

`<sk_o>` : 939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F


`address`: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE

`address(hex)`: fbffa997e319a72ba2a812933626f6d658dc534b

#### redeem key pair `<sk_r>, <pk_r>, <address>`

`<pk_r>` : 03f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad3

`<sk_r>` : 93Tn6J2RzLDK5edFPoj9M4Af5ZkaHXGZq7y9dPmoMFTRBNLFGsA

`address`: my83oRtfWZXJ9u9ZaedY6AwkPeN7ZxL7re

`address(hex)`: c11d4b272ae99f5e7dc87dbeae633c623d1c3ee7

#### Random pointer

`address`: 1KWTQLVAuoU77H7JDqvh3UYNy77NeRQiXc

`address(hex)`: cb045dd7539de812bf2d5af41780003dc604d350

	$ python key-to-address-ecc-example.py
	Private Key (hex) is:  d7024f5a2964fc94c796a1fc9ee62436ae6520fe969ef2a89782d2a0b8b203b4
	Private Key (decimal) is:  97251343808247774221656628364352559190469878328126021472553072425394731353012
	Private Key (WIF) is:  5KSyhTo9kpEnym1CqLVyGpcgK5CGhpxXtZEMz23CawBM52f8PAR
	Private Key Compressed (hex) is:  d7024f5a2964fc94c796a1fc9ee62436ae6520fe969ef2a89782d2a0b8b203b401
	Private Key (WIF-Compressed) is:  2T87qqQGJQ2JTqePwXua5MMapSPUJBLXcyKP6tRHnZLCqHKDLmDvHN
	Public Key (x,y) coordinates is: (10179296894609731562706274773961785325032063790531013458874088320583233987812L, 81995174933993126387092853608309731630047525001659270540213336743932217060484L)
	Public Key (hex) is: 041681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4b5479fd7eb284fb43a0952791636fb2953ec63ab9fc6bef36e880b73a1c4ec84
	Compressed Public Key (hex) is: 021681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4
	Bitcoin Address (b58check) is: 1KWTQLVAuoU77H7JDqvh3UYNy77NeRQiXc
	Compressed Bitcoin Address (b58check) is: 1A2RM4oYRGpa5KuJiMkQhd345ZiibPT888



## Transactions:

### Transfer some bitcoins to online address

In order to pay transaction fees to miners, we need firstly to transfer some bitcoins (let's say 10 btc in regtest) to the online address: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE. Following the instructions from the "bitcoin-regtest-tutorial", we use:

    $ bitcoin-cli -regtest sendtoaddress n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE 10

Or from the https://coinfaucet.eu/en/btc-testnet/, we can get a small amount bitcoin to fuel the transactions posted in testnet.

e.g.
https://live.blockcypher.com/btc-testnet/address/n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE/

Now we have bitcoins in our online address.


### First P2SH transaction in regtest

From https://en.bitcoin.it/wiki/Transaction, we can see the format of bitcoin transactions. Thus, the most important step is to decide the inputs and outputs, which are composed of scripts.

##### Constructing REG_TX redeem script


  `<pointer> DROP DUP HASH160 <identifier> EQUALVERFY CHECKSIG`

    Replace parameters:
    cb045dd7539de812bf2d5af41780003dc604d350 OP_DROP OP_DUP OP_HASH160 2abb1f98b76bdc4380138c09040a5838b6847f12 OP_EQUALVERIFY OP_CHECKSIG

    Push them to the stack format: (14 means how many bytes the following parameters that will be pushed to the stack)
    14 cb045dd7539de812bf2d5af41780003dc604d350 75 76 a9 14 2abb1f98b76bdc4380138c09040a5838b6847f12 88 ac

    Namely, 14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288ac

    Try to test the redeem script:
    $ bitcoin-cli -regtest decodescript 14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288ac
    {
    	  "asm": "cb045dd7539de812bf2d5af41780003dc604d350 OP_DROP OP_DUP OP_HASH160 2abb1f98b76bdc4380138c09040a5838b6847f12 OP_EQUALVERIFY OP_CHECKSIG",
    	  "type": "nonstandard",
    	  "p2sh": "2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6"
    }

At last, we got the redeem script hash: 2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6


##### Constructing the locking script


`OP_HASH160 [Hash160(redeemScript)] OP_EQUAL`



    OP_HASH160 2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6 OP_EQUAL

    ./bx address-decode 2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6
    wrapper
    {
        checksum 329980597
        payload feb1a122b740e102e1dbae1cbb0cb83550909ea8
        version 196
    }


    a9 14 feb1a122b740e102e1dbae1cbb0cb83550909ea8 87

    Namely, a914feb1a122b740e102e1dbae1cbb0cb83550909ea887




##### Creating a P2SH transaction

###### Step 1. We use createrawtransaction function to organize the outputs and inputs:

Assuming previous UTXO txid is: 93045ee56cbc1074957b40cc986d4f6c024bdbd26c9092e707947d6faee01f4f

    Usage: createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,"data":"hex",...} ( locktime ) ( replaceable )

    $ bitcoin-cli -regtest createrawtransaction '[{"txid":"93045ee56cbc1074957b40cc986d4f6c024bdbd26c9092e707947d6faee01f4f","vout":1}]' '{"2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6":9.9}'

    return tx hexstring value: 02000000014f1fe0ae6f7d9407e792906cd2db4b026c4f6d98cc407b957410bc6ce55e04930100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000


     $ bitcoin-cli -regtest decoderawtransaction 02000000014f1fe0ae6f7d9407e792906cd2db4b026c4f6d98cc407b957410bc6ce55e04930100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000
      {
        "txid": "ffc14ac145f42914cd0e0f4c8d44bcdffa7673e0941a620e1b23d105d7478a05",
        "hash": "ffc14ac145f42914cd0e0f4c8d44bcdffa7673e0941a620e1b23d105d7478a05",
        "version": 2,
        "size": 83,
        "vsize": 83,
        "locktime": 0,
        "vin": [
          {
            "txid": "93045ee56cbc1074957b40cc986d4f6c024bdbd26c9092e707947d6faee01f4f",
            "vout": 1,
            "scriptSig": {
              "asm": "",
              "hex": ""
            },
            "sequence": 4294967295
          }
        ],
        "vout": [
          {
            "value": 9.90000000,
            "n": 0,
            "scriptPubKey": {
              "asm": "OP_HASH160 feb1a122b740e102e1dbae1cbb0cb83550909ea8 OP_EQUAL",
              "hex": "a914feb1a122b740e102e1dbae1cbb0cb83550909ea887",
              "reqSigs": 1,
              "type": "scripthash",
              "addresses": [
                "2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6"
              ]
            }
          }
        ]
      }


###### Step 2. We use signrawtransaction function to sign the transactions

    Usage: signrawtransaction <hex string> [{"txid":txid,"vout":n,"scriptPubKey":hex,"redeemScript":hex},...] [<privatekey1>,...] [sighash="ALL"]

    $ bitcoin-cli -regtest signrawtransaction "02000000014f1fe0ae6f7d9407e792906cd2db4b026c4f6d98cc407b957410bc6ce55e04930100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000" '[{"txid":"93045ee56cbc1074957b40cc986d4f6c024bdbd26c9092e707947d6faee01f4f","vout":1,"scriptPubKey":"76a914fbffa997e319a72ba2a812933626f6d658dc534b88ac"}]' '["939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F"]' "ALL"


    {
      "hex": "02000000014f1fe0ae6f7d9407e792906cd2db4b026c4f6d98cc407b957410bc6ce55e0493010000008b483045022100a89de41497c8bd2a007458083770eb6073bdffc61b8547ddfe4bf96a9cfa720c0220475453cad63c209ebc38007411d990c380ac651c0883a37a51ad4c7f49efde69014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000",
      "complete": true
    }

###### Step 3. We use sendrawtransaction function to send the transactions

    $ bitcoin-cli -regtest sendrawtransaction 02000000014f1fe0ae6f7d9407e792906cd2db4b026c4f6d98cc407b957410bc6ce55e0493010000008b483045022100a89de41497c8bd2a007458083770eb6073bdffc61b8547ddfe4bf96a9cfa720c0220475453cad63c209ebc38007411d990c380ac651c0883a37a51ad4c7f49efde69014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000


    return transaction id: b5458f40ea28362ec1331246c7d49cfb21deea2e3fbe5a135c04f752f63bf42d

    $ bitcoin-cli -regtest generate 1

    we can get the mined block, then we could get the block and check if there is the sealed transaction

    bitcoin-cli -regtest getblock "blockid"


### Second P2SH transaction in regtest

##### Constructing UPD & RVC redeem script


`IF
DROP <pk_o> CHECKSIG
ELSE
2 <pk_o> <pk_f> 2 CHECKMULTISIG <new_pk> CHECKSIG
ENDIF`

    Replace parameters and stack format:
    63
    75 41 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 ac
    67
    52 41 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 41 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 52 ae 41 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 ac
    68

    Namely, 63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67524104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405952ae4104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5ac68

    Try to test the redeem script:
    $ bitcoin-cli -regtest decodescript 63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67524104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405952ae4104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5ac68
    {
      "asm": "OP_IF OP_DROP 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 OP_CHECKSIG OP_ELSE 2 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 2 OP_CHECKMULTISIG 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 OP_CHECKSIG OP_ENDIF",
      "type": "nonstandard",
      "p2sh": "2Myyr8eEhcUDAXPWbpAX9dMQXRcuyiistyJ"
    }


    At last, we got the redeem script hash: 2Myyr8eEhcUDAXPWbpAX9dMQXRcuyiistyJ

##### Constructing the locking script

`OP_HASH160 [Hash160(redeemScript)] OP_EQUAL`

    OP_HASH160 2Myyr8eEhcUDAXPWbpAX9dMQXRcuyiistyJ OP_EQUAL

    $./bx address-decode 2Myyr8eEhcUDAXPWbpAX9dMQXRcuyiistyJ
    wrapper
    {
       checksum 3990322939
       payload 49e0658d5f9346fcb95d897df612c743e693072f
       version 196
    }


    a9 14 49e0658d5f9346fcb95d897df612c743e693072f 87

    Namely, a91449e0658d5f9346fcb95d897df612c743e693072f87



##### Creating a P2SH transaction

###### Step 1. Constructing the transaction message that will be signed

    version:
    01000000

    number of inputs:
    01

    previous transaction id (reversed)
    2df43bf652f7045c135abe3f2eeade21fb9cd4c7461233c12e3628ea408f45b5

		previous output index:
		00000000

		script length:
		17

    scriptPubKey:
    a914feb1a122b740e102e1dbae1cbb0cb83550909ea887

    sequence:
    ffffffff

    number of output:
    01

    value: (9.8btc: 9.8 * 10^8 = 3a699d00)
		009d693a

		script length:
		17

		scriptPubKey:
    a91449e0658d5f9346fcb95d897df612c743e693072f87

		locktime:
		00000000

		sighash code:
		01000000


		Namely, 01000000012df43bf652f7045c135abe3f2eeade21fb9cd4c7461233c12e3628ea408f45b50000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea887ffffffff01009d693a17a91449e0658d5f9346fcb95d897df612c743e693072f870000000001000000


###### Step. 2 Double SHA256 hashing the transaction message Before signing

In ubuntu terminal we run:

		$mhex=01000000012df43bf652f7045c135abe3f2eeade21fb9cd4c7461233c12e3628ea408f45b50000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea887ffffffff01009d693a17a91449e0658d5f9346fcb95d897df612c743e693072f870000000001000000

		$ echo -n $mhex | xxd -r -p | openssl dgst -sha256 -binary | openssl dgst -sha256

		the result is: 03afbb3289563740fde3f3ef04d3c3c61508fa8573aa573a7d413421458df3e8


###### Step. 3 signing









We use createrawtransaction function to organize the outputs and inputs:

If the previous UTXO txid is: b5458f40ea28362ec1331246c7d49cfb21deea2e3fbe5a135c04f752f63bf42d


    $ bitcoin-cli -regtest createrawtransaction '[{"txid":"b5458f40ea28362ec1331246c7d49cfb21deea2e3fbe5a135c04f752f63bf42d","vout":0}]' '{"2Myyr8eEhcUDAXPWbpAX9dMQXRcuyiistyJ":9.8}'

    return tx hexstring value: 02000000012df43bf652f7045c135abe3f2eeade21fb9cd4c7461233c12e3628ea408f45b50000000000ffffffff01009d693a0000000017a91449e0658d5f9346fcb95d897df612c743e693072f8700000000

###### Step 2. We use signrawtransaction function to sign the transactions

    $ bitcoin-cli -regtest signrawtransaction "02000000012df43bf652f7045c135abe3f2eeade21fb9cd4c7461233c12e3628ea408f45b50000000000ffffffff01009d693a0000000017a91449e0658d5f9346fcb95d897df612c743e693072f8700000000" '[{"txid":"b5458f40ea28362ec1331246c7d49cfb21deea2e3fbe5a135c04f752f63bf42d","vout":0,"scriptPubKey":"a914feb1a122b740e102e1dbae1cbb0cb83550909ea887","redeemScript":"14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288ac"}]' '["92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz"]' "ALL"



#### P2SH Ouputs










UPD_TX redeem script:

	<pointer> DROP HASH160 <identifier> EQUALVERFY <sig_sk_o> <pk_o> CHECKSIG




How to spend the P2SH output? signauture of the offline sk




1.





2. spend the p2sh output


$ bitcoin-cli -regtest decoderawtransaction 0200000001d36d9f9a7e103654f7fe1eb2934f7bee6138607bfae5a96f73d0fc83aa3be489010000008a473044022023fa441d0591a9beffb8c861baf13a70922474942788c891ec35e27b857260d1022050bfe3a19e1cb86e8360718044b98a0a9bc38b4c5794116da4ac5144d39e9dbd014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000
{
  "txid": "d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3",
  "hash": "d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3",
  "version": 2,
  "size": 221,
  "vsize": 221,
  "locktime": 0,
  "vin": [
    {
      "txid": "89e43baa83fcd0736fa9e5fa7b603861ee7b4f93b21efef75436107e9a9f6dd3",
      "vout": 1,
      "scriptSig": {
        "asm": "3044022023fa441d0591a9beffb8c861baf13a70922474942788c891ec35e27b857260d1022050bfe3a19e1cb86e8360718044b98a0a9bc38b4c5794116da4ac5144d39e9dbd[ALL] 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617",
        "hex": "473044022023fa441d0591a9beffb8c861baf13a70922474942788c891ec35e27b857260d1022050bfe3a19e1cb86e8360718044b98a0a9bc38b4c5794116da4ac5144d39e9dbd014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617"
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 9.90000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_HASH160 feb1a122b740e102e1dbae1cbb0cb83550909ea8 OP_EQUAL",
        "hex": "a914feb1a122b740e102e1dbae1cbb0cb83550909ea887",
        "reqSigs": 1,
        "type": "scripthash",
        "addresses": [
          "2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6"
        ]
      }
    }
  ]
}





p2sh transaction id: d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3



	bitcoin-cli -regtest createrawtransaction '[{"txid":"d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3","vout":0}]' '{"mqXL5Sr1V3jf14f6A9CCY3vzYgXGsuZosY":9.8}'


$ bitcoin-cli -regtest createrawtransaction '[{"txid":"d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3","vout":0}]' '{"mqXL5Sr1V3jf14f6A9CCY3vzYgXGsuZosY":9.8}'

0200000001d3e9bc378c1144d2faf4108aba91d560c0d7ea504340421ebf18980a21b23dd60000000000ffffffff01009d693a000000001976a9146dc370e94793b246e54072ecd625a14ac62e52cd88ac00000000


$ bitcoin-cli -regtest decoderawtransaction 0200000001d3e9bc378c1144d2faf4108aba91d560c0d7ea504340421ebf18980a21b23dd60000000000ffffffff01009d693a000000001976a9146dc370e94793b246e54072ecd625a14ac62e52cd88ac00000000
{
  "txid": "eab696f40878fd6ffe604233562bbffe2d8f8c47cf635c715eb868c913f14c81",
  "hash": "eab696f40878fd6ffe604233562bbffe2d8f8c47cf635c715eb868c913f14c81",
  "version": 2,
  "size": 85,
  "vsize": 85,
  "locktime": 0,
  "vin": [
    {
      "txid": "d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3",
      "vout": 0,
      "scriptSig": {
        "asm": "",
        "hex": ""
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 9.80000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 6dc370e94793b246e54072ecd625a14ac62e52cd OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a9146dc370e94793b246e54072ecd625a14ac62e52cd88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "mqXL5Sr1V3jf14f6A9CCY3vzYgXGsuZosY"
        ]
      }
    }
  ]
}




	bitcoin-cli -regtest signrawtransaction "0200000001d3e9bc378c1144d2faf4108aba91d560c0d7ea504340421ebf18980a21b23dd60000000000ffffffff01009d693a000000001976a9146dc370e94793b246e54072ecd625a14ac62e52cd88ac00000000" '[{"txid":"d63db2210a9818bf1e42404350ead7c060d591ba8a10f4fad244118c37bce9d3","vout":0,"scriptPubKey":"a914feb1a122b740e102e1dbae1cbb0cb83550909ea887","redeemScript":"14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288ac"}]' '["939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F"]' "ALL"






















#New Improved Scheme
