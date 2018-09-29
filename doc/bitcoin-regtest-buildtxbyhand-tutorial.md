# Building Our Identity Transactions by Hand in Bitcoin

## Preliminaries

Following our previous "bitcoin-regtest-addresstokeys" tutuorial, we got testnet info:

##### Offline key pair: `<sk_f>, <pk_f>, <identifier>`

`<pk_f>` : 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059

`<sk_f>` : 92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz


`identifier`: mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4

`identifier(hex)`: 2abb1f98b76bdc4380138c09040a5838b6847f12

#### Online key pair `<sk_o>, <pk_o>, <address>`;

`<pk_o>` : 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617

`<sk_o>` : 939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F


`address`: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE

`address(hex)`: fbffa997e319a72ba2a812933626f6d658dc534b

#### redeem key pair `<sk_r>, <pk_r>, <address>`

`<pk_r>` : 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5

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

Assuming previous UTXO txid is: 7672e259ed4822e7fb68e0b481d160a4a70fee0fa63d8ca272370fc04334fdfe

    Usage: createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,"data":"hex",...} ( locktime ) ( replaceable )

    $ bitcoin-cli -regtest createrawtransaction '[{"txid":"7672e259ed4822e7fb68e0b481d160a4a70fee0fa63d8ca272370fc04334fdfe","vout":1}]' '{"2NGTvQvDwv8RvAJvCYkpJSotu79Pyd5s3Q6":9.9}'

    return tx hexstring value: 0200000001fefd3443c00f3772a28c3da60fee0fa7a460d181b4e068fbe72248ed59e272760100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000


     $ bitcoin-cli -regtest decoderawtransaction 0200000001fefd3443c00f3772a28c3da60fee0fa7a460d181b4e068fbe72248ed59e272760100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000
			{
			  "txid": "1fd800fc0f0f806b569ec6f4c4c23e436f05b2be1208f39d44e4d8dd74061c7e",
			  "hash": "1fd800fc0f0f806b569ec6f4c4c23e436f05b2be1208f39d44e4d8dd74061c7e",
			  "version": 2,
			  "size": 83,
			  "vsize": 83,
			  "locktime": 0,
			  "vin": [
			    {
			      "txid": "7672e259ed4822e7fb68e0b481d160a4a70fee0fa63d8ca272370fc04334fdfe",
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

    $ bitcoin-cli -regtest signrawtransaction "0200000001fefd3443c00f3772a28c3da60fee0fa7a460d181b4e068fbe72248ed59e272760100000000ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000" '[{"txid":"7672e259ed4822e7fb68e0b481d160a4a70fee0fa63d8ca272370fc04334fdfe","vout":1,"scriptPubKey":"76a914fbffa997e319a72ba2a812933626f6d658dc534b88ac"}]' '["939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F"]' "ALL"


		{
		  "hex": "0200000001fefd3443c00f3772a28c3da60fee0fa7a460d181b4e068fbe72248ed59e27276010000008a4730440220309181fec13f4f9696ab6030546a125dcd04b148d0c316fe76c96ec5fd809d8d02204ad03c1a1ef9363b894abcbc63d5b4b14871841963743b76a1fc44ee6454ac65014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000",
		  "complete": true
		}


###### Step 3. We use sendrawtransaction function to send the transactions

    $ bitcoin-cli -regtest sendrawtransaction 0200000001fefd3443c00f3772a28c3da60fee0fa7a460d181b4e068fbe72248ed59e27276010000008a4730440220309181fec13f4f9696ab6030546a125dcd04b148d0c316fe76c96ec5fd809d8d02204ad03c1a1ef9363b894abcbc63d5b4b14871841963743b76a1fc44ee6454ac65014104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ffffffff018033023b0000000017a914feb1a122b740e102e1dbae1cbb0cb83550909ea88700000000


    return transaction id: 9973974eca92a937ee565536d64f3f11fc58e6dffc3a410fc502e0fcbe57ad80

    $ bitcoin-cli -regtest generate 1

    we can get the mined block, then we could get the block and check if there is the sealed transaction

    bitcoin-cli -regtest getblock "blockid"


### Second P2SH transaction in regtest

##### Constructing UPD & RVC redeem script


`IF
DROP <pk_o> CHECKSIG
ELSE
3 <new_pk> <pk_o> <pk_f> 3 CHECKMULTISIG
ENDIF`

    Replace parameters and stack format:
		63
		75 41 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 ac
		67
		53 41 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 41 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 41 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 53 ae
		68

    Namely, 63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68

    Try to test the redeem script:
		$ bitcoin-cli -regtest decodescript 63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68
		{
		  "asm": "OP_IF OP_DROP 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 OP_CHECKSIG OP_ELSE 3 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 3 OP_CHECKMULTISIG OP_ENDIF",
		  "type": "nonstandard",
		  "p2sh": "2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s"
		}


    At last, we got the redeem script hash: 2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s

##### Constructing the locking script

`OP_HASH160 [Hash160(redeemScript)] OP_EQUAL`

    OP_HASH160 2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s OP_EQUAL

		./bx address-decode 2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s
		wrapper
		{
		    checksum 1219552648
		    payload abb7f242eb213ff67159d1a49d58ae19ef2fbee6
		    version 196
		}

    a9 14 abb7f242eb213ff67159d1a49d58ae19ef2fbee6 87

    Namely, a914abb7f242eb213ff67159d1a49d58ae19ef2fbee687




##### Creating a P2SH transaction

###### Step 1. We use createrawtransaction function to organize the outputs and inputs:

Previous txid: 9973974eca92a937ee565536d64f3f11fc58e6dffc3a410fc502e0fcbe57ad80

		bitcoin-cli -regtest createrawtransaction '[{"txid":"9973974eca92a937ee565536d64f3f11fc58e6dffc3a410fc502e0fcbe57ad80","vout":0}]' '{"2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s":9.8}'

		return value: 020000000180ad57befce002c50f413afcdfe658fc113f4fd6365556ee37a992ca4e9773990000000000ffffffff01009d693a0000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000


###### Step 2. We use signrawtransaction function to sign the transactions

		bitcoin-cli -regtest signrawtransaction "020000000180ad57befce002c50f413afcdfe658fc113f4fd6365556ee37a992ca4e9773990000000000ffffffff01009d693a0000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000" '[{"txid":"9973974eca92a937ee565536d64f3f11fc58e6dffc3a410fc502e0fcbe57ad80","vout":0,"scriptPubKey":"a914feb1a122b740e102e1dbae1cbb0cb83550909ea887","redeemScript":"14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288ac"}]' '["92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz"]' "ALL"


		{
		  "hex": "020000000180ad57befce002c50f413afcdfe658fc113f4fd6365556ee37a992ca4e97739900000000bb48304502210088370fa635156f5cbe47ef56da6e426d98569bcab606b4269de9c2215c37199b0220444a9d973a8bab3ca1dc82c99057a2fff375e66f7f120422141fc264e4f295bf014104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd40592f14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288acffffffff01009d693a0000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000",
		  "complete": true
		}


###### Step 3. We use sendrawtransaction function to send the transactions



		$ bitcoin-cli -regtest sendrawtransaction 020000000180ad57befce002c50f413afcdfe658fc113f4fd6365556ee37a992ca4e97739900000000bb48304502210088370fa635156f5cbe47ef56da6e426d98569bcab606b4269de9c2215c37199b0220444a9d973a8bab3ca1dc82c99057a2fff375e66f7f120422141fc264e4f295bf014104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd40592f14cb045dd7539de812bf2d5af41780003dc604d3507576a9142abb1f98b76bdc4380138c09040a5838b6847f1288acffffffff01009d693a0000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000

		return transaction id: 029866392548b4bc3ec2c8438dd88e3e5b419ba67eff0901c68fb767c0650e3e

		$ bitcoin-cli -regtest generate 1

		we can get the mined block, then we could get the block and check if there is the sealed transaction

		$ bitcoin-cli -regtest getblock "blockid"



### Third P2SH transaction (modifying pointer in update transaction)

##### Constructing UPD & RVC redeem script

		modified pointer + redeemscript
		14cb045dd7539de812bf2d5af41780003dc604d350
		63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68

		namely, 14cb045dd7539de812bf2d5af41780003dc604d35063754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68

##### Creating a P2SH transaction

###### Step 1. We use createrawtransaction function to organize the outputs and inputs:
		Previous txid: 029866392548b4bc3ec2c8438dd88e3e5b419ba67eff0901c68fb767c0650e3e

		$ bitcoin-cli -regtest createrawtransaction '[{"txid":"029866392548b4bc3ec2c8438dd88e3e5b419ba67eff0901c68fb767c0650e3e","vout":0}]' '{"2N8uBtuHWLSwVzXrZzCJF71sLLLDNWa9t2s":9.7}'

		return value: 02000000013e0e65c067b78fc60109ff7ea69b415b3e8ed88d43c8c23ebcb44825396698020000000000ffffffff018006d1390000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000

###### Step 2. We use signrawtransaction function to sign the transactions

		$ bitcoin-cli -regtest signrawtransaction "02000000013e0e65c067b78fc60109ff7ea69b415b3e8ed88d43c8c23ebcb44825396698020000000000ffffffff018006d1390000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000" '[{"txid":"029866392548b4bc3ec2c8438dd88e3e5b419ba67eff0901c68fb767c0650e3e","vout":0,"scriptPubKey":"a914abb7f242eb213ff67159d1a49d58ae19ef2fbee687","redeemScript":"14cb045dd7539de812bf2d5af41780003dc604d35063754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68"}]' '["939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F"]' "ALL"


		{
	  "hex": "02000000013e0e65c067b78fc60109ff7ea69b415b3e8ed88d43c8c23ebcb448253966980200000000fd7101473044022064bfc17c78fd1a7755aa5f129c8543f185e9cefc73d9bfff5ac061f832a611fc022020e6a153046aa23dbd7148d5b8b69775cac136af35ad36104e767c88c85f9dd50114cb045dd7539de812bf2d5af41780003dc604d350514d100163754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68ffffffff018006d1390000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000",
	  "complete": true
		}


###### Step 3. We use sendrawtransaction function to send the transactions

		$ bitcoin-cli -regtest sendrawtransaction 02000000013e0e65c067b78fc60109ff7ea69b415b3e8ed88d43c8c23ebcb448253966980200000000fd7101473044022064bfc17c78fd1a7755aa5f129c8543f185e9cefc73d9bfff5ac061f832a611fc022020e6a153046aa23dbd7148d5b8b69775cac136af35ad36104e767c88c85f9dd50114cb045dd7539de812bf2d5af41780003dc604d350514d100163754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68ffffffff018006d1390000000017a914abb7f242eb213ff67159d1a49d58ae19ef2fbee68700000000

		return transaction id: 4ef83ef84eea12b1db07a7217e4119dc1b0dbff5618506c633e15b5d7d64e334

		$ bitcoin-cli -regtest generate 1

		we can get the mined block, then we could get the block and check if there is the sealed transaction

		$ bitcoin-cli -regtest getblock "blockid"



### Fourth P2SH transaction (modifying keys in update and revoke transaction)

##### Constructing the NEW UPD & RVC redeem script

If you want to modify the online key, you have

`IF
DROP <modified pk_o> CHECKSIG
ELSE
3 <the second new_pk> <modified pk_o> <pk_f> 3 CHECKMULTISIG
ENDIF`

If you want to modify the offline key, you have

`IF
DROP <pk_o> CHECKSIG
ELSE
3 <the second new_pk> <pk_o> <modified pk_f> 3 CHECKMULTISIG
ENDIF`

We take the first one as the example to get the hash value (payment address) of this new redeem script:


	Replace parameters and stack format:
	63
	75 41 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 ac
	67
	53 41 041681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4b5479fd7eb284fb43a0952791636fb2953ec63ab9fc6bef36e880b73a1c4ec84 41 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 41 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 53 ae
	68

	Namely,
	63754104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5ac675341041681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4b5479fd7eb284fb43a0952791636fb2953ec63ab9fc6bef36e880b73a1c4ec844104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68

	Try to test the redeem script:
	$ bitcoin-cli -regtest decodescript 63754104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5ac675341041681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4b5479fd7eb284fb43a0952791636fb2953ec63ab9fc6bef36e880b73a1c4ec844104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68
	{
	  "asm": "OP_IF OP_DROP 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 OP_CHECKSIG OP_ELSE 3 041681472282f8138eaad307dab831c3271abd8365c023d18853eee1b1e8ac64e4b5479fd7eb284fb43a0952791636fb2953ec63ab9fc6bef36e880b73a1c4ec84 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059 3 OP_CHECKMULTISIG OP_ENDIF",
	  "type": "nonstandard",
	  "p2sh": "2My25BT3zm7uK3fq5FqTPQ9RQ2gzxsWUc5B"
	}



	At last, we got the redeem script hash: 2My25BT3zm7uK3fq5FqTPQ9RQ2gzxsWUc5B

##### Creating a P2SH transaction


###### Step 1. We use createrawtransaction function to organize the outputs and inputs:

Previous txid: 4ef83ef84eea12b1db07a7217e4119dc1b0dbff5618506c633e15b5d7d64e334

	bitcoin-cli -regtest createrawtransaction '[{"txid":"4ef83ef84eea12b1db07a7217e4119dc1b0dbff5618506c633e15b5d7d64e334","vout":0}]' '{"2My25BT3zm7uK3fq5FqTPQ9RQ2gzxsWUc5B":9.6}'

	return value: 020000000134e3647d5d5be133c6068561f5bf0d1bdc19417e21a707dbb112ea4ef83ef84e0000000000ffffffff01007038390000000017a9143f53febdcaac76d550ca8d3bd7cc535eff975ed08700000000

###### Step 2. We use signrawtransaction function to sign the transactions
	bitcoin-cli -regtest signrawtransaction "020000000134e3647d5d5be133c6068561f5bf0d1bdc19417e21a707dbb112ea4ef83ef84e0000000000ffffffff01007038390000000017a9143f53febdcaac76d550ca8d3bd7cc535eff975ed08700000000" '[{"txid":"4ef83ef84eea12b1db07a7217e4119dc1b0dbff5618506c633e15b5d7d64e334","vout":0,"scriptPubKey":"a914abb7f242eb213ff67159d1a49d58ae19ef2fbee687","redeemScript":"63754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68"}]' '["93Tn6J2RzLDK5edFPoj9M4Af5ZkaHXGZq7y9dPmoMFTRBNLFGsA","939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F","92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz"]' "ALL"


	{
		"hex": "020000000134e3647d5d5be133c6068561f5bf0d1bdc19417e21a707dbb112ea4ef83ef84e00000000fded0100473044022014dd61f673714ffe2281abbe8308f322f8c36c1a681e651fc0f0556641c51b420220763b0fdd6e851fc87d65205520d38e3e14078b79d71b1d839f7ebc3b47e5d1bf01473044022076a702a9acae692cbaa29d0bb34c1b772171f596af5a947c1aff058e58cd6065022068f2a0244eb47290546de0096d70781191d324eec72f2eaeac32c8d92ff2aeee0147304402205001cd692a16be704e86b8ff8a75fda12c53099b2672c3159898e5550c3411be02203ae7bcf929e044123178d89f46426f6f2c650fc9d09bfd3a0210d16bf8ba93b701004d100163754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68ffffffff01007038390000000017a9143f53febdcaac76d550ca8d3bd7cc535eff975ed08700000000",
		"complete": true
	}


###### Step 3. We use sendrawtransaction function to send the transactions



		$ bitcoin-cli -regtest sendrawtransaction 020000000134e3647d5d5be133c6068561f5bf0d1bdc19417e21a707dbb112ea4ef83ef84e00000000fded0100473044022014dd61f673714ffe2281abbe8308f322f8c36c1a681e651fc0f0556641c51b420220763b0fdd6e851fc87d65205520d38e3e14078b79d71b1d839f7ebc3b47e5d1bf01473044022076a702a9acae692cbaa29d0bb34c1b772171f596af5a947c1aff058e58cd6065022068f2a0244eb47290546de0096d70781191d324eec72f2eaeac32c8d92ff2aeee0147304402205001cd692a16be704e86b8ff8a75fda12c53099b2672c3159898e5550c3411be02203ae7bcf929e044123178d89f46426f6f2c650fc9d09bfd3a0210d16bf8ba93b701004d100163754104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617ac67534104f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d54104a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c16174104c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd405953ae68ffffffff01007038390000000017a9143f53febdcaac76d550ca8d3bd7cc535eff975ed08700000000

		return transaction id: 4ef83ef84eea12b1db07a7217e4119dc1b0dbff5618506c633e15b5d7d64e334

		$ bitcoin-cli -regtest generate 1

		we can get the mined block, then we could get the block and check if there is the sealed transaction

		$ bitcoin-cli -regtest getblock "blockid"
