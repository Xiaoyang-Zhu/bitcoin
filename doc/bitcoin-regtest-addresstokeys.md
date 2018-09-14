OS environment: Ubuntu Version: 18.04 desktop image

download from site:  http://releases.ubuntu.com/18.04/


with Installed Bitcoin Client

from our previous tutorial:  
https://github.com/Xiaoyang-Zhu/bitcoin/blob/BIMS/doc/build-ubuntu.md


## Git Clone Mastering Bitcoin Repository
    $ git clone https://github.com/bitcoinbook/bitcoinbook

## Try to run the key generation python code

In directory: bitcoinbook/code

    $ python key-to-address-ecc-example.py

you might see the error

ImportError: No module named bitcoin


## Install python pip and missing import modules
    $ sudo apt-get install python-pip python-dev build-essential  
    $ sudo pip install --upgrade pip

If the python code alerts import package module is missing, please try to install using the following command

    $ sudo pip install bitcoin

## Retry to get keys

In directory: bitcoinbook/code

    $ python key-to-address-ecc-example.py
    Private Key (hex) is:  9c802826b94975db68649c1445ab75e0b73d6904005891913ad2ad8f5a6c18e5
    Private Key (decimal) is:  70787237917126028560551399631563256922989862033014521880535346302524429441253
    Private Key (WIF) is:  5K1DBNURcAC1Vd4DzZTdLTMteeJNoKYg5CXXKDp4BDyExbRRsn2
    Private Key Compressed (hex) is:  9c802826b94975db68649c1445ab75e0b73d6904005891913ad2ad8f5a6c18e501
    Private Key (WIF-Compressed) is:  2SyTrGCdpWHcqrbVKuXRX5XEBpq6xVw8p47GsVuLsdxyq44pvn2sW1
    Public Key (x,y) coordinates is: (87351246654006940321266655035762225154400190937710864217695849045510997037636L, 69132917292333773438747628975647872942339518088830942093065783481327826059353L)
    Public Key (hex) is: 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059
    Compressed Public Key (hex) is: 03c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a5244
    Bitcoin Address (b58check) is: 14twYcyj9nXpUzh3gJAgyqZ2W5Up1cRZML
    Compressed Bitcoin Address (b58check) is: 1H9QzqEBuThfYdAGjMcUtEuro4Q61mJzj4


    $ python key-to-address-ecc-example.py Private Key (hex) is:  cdbffb57b5803ff217bce03255de482c0c5eb4636dfe2408025a627ef9d6c30c
    Private Key (decimal) is:  93063336451904969191678757650577246799536103068067384135108429857071941731084
    Private Key (WIF) is:  5KNuBpKnwWpjCkQRGbd4Dx26vQ5KuwovAvuHYu1S4KjM51vVKbY
    Private Key Compressed (hex) is:  cdbffb57b5803ff217bce03255de482c0c5eb4636dfe2408025a627ef9d6c30c01
    Private Key (WIF-Compressed) is:  2T6kQE8FrjvGovypGDCT5K42gGvshBDBHgnTwqJ3qA1gj5HaC3ESxs
    Public Key (x,y) coordinates is: (74272359821405404949316163399575265306769599413562529471537738191148001711860L, 62810344916106188972914385504469131233742602495829897079947810266940103726615L)
    Public Key (hex) is: 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617
    Compressed Public Key (hex) is: 03a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f4
    Bitcoin Address (b58check) is: 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae
    Compressed Bitcoin Address (b58check) is: 17U86Wh2pEB1A6VKNEhBXUMaEKFTDFhBci



## Get testnet address
There are currently three address formats in use:

P2PKH which begin with the number 1, eg: 1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2.

P2SH type starting with the number 3, eg: 3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy.

Bech32 type starting with bc1, eg: bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq.

![Address Map](https://en.bitcoin.it/w/images/en/4/48/Address_map.jpg)

### Download the libbitcoin explorer

Download the libbitcoin explorer linux release version from:  
  https://github.com/libbitcoin/libbitcoin-explorer/releases

Then, we have executable binary file

bx-linux-x64-qrcode

Remane the file as bx

Add execute priviledge using chmod


### Get testnet address

We run the following commands and get:

Testnet address for 14twYcyj9nXpUzh3gJAgyqZ2W5Up1cRZML:

    mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4

Testnet address for 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae:

    n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE

Command lines: 

    $./bx bitcoin160 "04+ uncompressed public key"
    $./bx base58check-encode --version 111 "return value"

    e.g. 1
    $./bx bitcoin160 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617
    return value: fbffa997e319a72ba2a812933626f6d658dc534b

    $./bx base58check-encode --version 111 fbffa997e319a72ba2a812933626f6d658dc534b
    testnet address: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE

    $./bx base58check-encode --version 00 fbffa997e319a72ba2a812933626f6d658dc534b
    mainnet bitcoin address: 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae

    e.g. 2
    $./bx bitcoin160 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059
    return value: 2abb1f98b76bdc4380138c09040a5838b6847f12

    $./bx base58check-encode --version 111 2abb1f98b76bdc4380138c09040a5838b6847f12
    testnet address: mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4

    $./bx base58check-encode --version 00 2abb1f98b76bdc4380138c09040a5838b6847f12
    mainnet bitcoin address: 14twYcyj9nXpUzh3gJAgyqZ2W5Up1cRZML
