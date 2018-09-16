OS environment: Ubuntu Version: 18.04 desktop image

download from site:  http://releases.ubuntu.com/18.04/


with Installed Bitcoin Client

from our previous tutorial:  
https://github.com/Xiaoyang-Zhu/bitcoin/blob/BIMS/doc/build-ubuntu.md


## Git clone Mastering Bitcoin repository
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

    //Offline key pair
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

    //Online key pair
    $ python key-to-address-ecc-example.py
    Private Key (hex) is:  cdbffb57b5803ff217bce03255de482c0c5eb4636dfe2408025a627ef9d6c30c
    Private Key (decimal) is:  93063336451904969191678757650577246799536103068067384135108429857071941731084
    Private Key (WIF) is:  5KNuBpKnwWpjCkQRGbd4Dx26vQ5KuwovAvuHYu1S4KjM51vVKbY
    Private Key Compressed (hex) is:  cdbffb57b5803ff217bce03255de482c0c5eb4636dfe2408025a627ef9d6c30c01
    Private Key (WIF-Compressed) is:  2T6kQE8FrjvGovypGDCT5K42gGvshBDBHgnTwqJ3qA1gj5HaC3ESxs
    Public Key (x,y) coordinates is: (74272359821405404949316163399575265306769599413562529471537738191148001711860L, 62810344916106188972914385504469131233742602495829897079947810266940103726615L)
    Public Key (hex) is: 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617
    Compressed Public Key (hex) is: 03a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f4
    Bitcoin Address (b58check) is: 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae
    Compressed Bitcoin Address (b58check) is: 17U86Wh2pEB1A6VKNEhBXUMaEKFTDFhBci

    //Redeem key pair
    $ python key-to-address-ecc-example.py
    Private Key (hex) is:  f72e8e6dd32ac3acff3b5183056555aabefb8239241db98ee5076aa90e356f2e
    Private Key (decimal) is:  111803531573900520365605284033588879230304401253850810433915443489764141264686
    Private Key (WIF) is:  5Kh9WZCtQ79B7b7xmTqEUTchRuPs8MjNVB7CYmRJ1WiNQK1dzBi
    Private Key Compressed (hex) is:  f72e8e6dd32ac3acff3b5183056555aabefb8239241db98ee5076aa90e356f2e01
    Private Key (WIF-Compressed) is:  2TCst2EQMZd3qUfJH1QgrB3eeDQ4r2tP4gRy5Z6DEhorw7F1pgcjGV
    Public Key (x,y) coordinates is: (111659282457299324564486768928982430427989002009177306275839193938168430140115L, 19455728555461611406846379665161860193184161026197862945358133634527093265621L)
    Public Key (hex) is: 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5
    Compressed Public Key (hex) is: 03f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad3
    Bitcoin Address (b58check) is: 1Jc6WNoghY63Nnfws5fAGFjRXemQiWqgdw
    Compressed Bitcoin Address (b58check) is: 1AbbZU4ZqaFw5C3H7ZSC8s7Qwvk3e7YQmd

## Get testnet address and keys
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


### Get testnet address and keys

We run the following commands and get:

Testnet address and keys for offline address 14twYcyj9nXpUzh3gJAgyqZ2W5Up1cRZML:


    Public key hash(hex): 2abb1f98b76bdc4380138c09040a5838b6847f12
    Testnet address: mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4
    Testnet private key: 92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz

Testnet address and keys for online address 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae:

    Public key hash(hex): fbffa997e319a72ba2a812933626f6d658dc534b
    Testnet address: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE
    Testnet private key: 939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F

Testnet address and keys for redeem address 1Jc6WNoghY63Nnfws5fAGFjRXemQiWqgdw:

    Public key hash(hex): c11d4b272ae99f5e7dc87dbeae633c623d1c3ee7
    Testnet address: my83oRtfWZXJ9u9ZaedY6AwkPeN7ZxL7re
    Testnet private key: 93Tn6J2RzLDK5edFPoj9M4Af5ZkaHXGZq7y9dPmoMFTRBNLFGsA



Command lines:

    $./bx bitcoin160 "04+ uncompressed public key"
    $./bx base58check-encode --version 111 "return value"

    e.g. 1
    $./bx bitcoin160 04c11f0db597b264746e3042096955a5f8509d2374320b02900c2b9995073a524498d7d8345b5d4a8d0a188c5f4a599ec97d592c6a7a38a00fecc1c46dd7bd4059
    return value: 2abb1f98b76bdc4380138c09040a5838b6847f12

    $./bx base58check-encode --version 111 2abb1f98b76bdc4380138c09040a5838b6847f12
    testnet address: mjQtqg4hxoy5G7AfPs94okmMN55WuWffu4

    $./bx base58check-encode --version 00 2abb1f98b76bdc4380138c09040a5838b6847f12
    mainnet bitcoin address: 14twYcyj9nXpUzh3gJAgyqZ2W5Up1cRZML

    $./bx base58check-encode --version 239 9c802826b94975db68649c1445ab75e0b73d6904005891913ad2ad8f5a6c18e5
    testnet privacy key: 92mqm7HyCPG9TgZWcuMYD3urJJf5xV5sR9PUPrAZWxiHja4iAiz

    e.g. 2
    $./bx bitcoin160 04a434aa7ac19a69558243772914f79281b555ffed2f825a78006ef6f51b1ed6f48add6538e56d067f7d4f6d4cb9efb59d6e83ff26e40a0559bd0c1e53456c1617
    return value: fbffa997e319a72ba2a812933626f6d658dc534b

    $./bx base58check-encode --version 111 fbffa997e319a72ba2a812933626f6d658dc534b
    testnet address: n4VQAwgPUUdsYFskbkzjx1AwmoskhCVYNE

    $./bx base58check-encode --version 00 fbffa997e319a72ba2a812933626f6d658dc534b
    mainnet bitcoin address: 1PySstbQfTCcm9Q8tC2N85xcupH3o7zeae

    $./bx base58check-encode --version 239 cdbffb57b5803ff217bce03255de482c0c5eb4636dfe2408025a627ef9d6c30c
    testnet private key: 939XmZ9LXjtsAouhtwWy6Ya4a4S357M7WsmEdXMwQ4UPr7LQo3F


    e.g. 3
    $ ./bx bitcoin160 04f6dcea0cb836e2343b96b6fae8b8aa09f0488d58c469830e16e1a5db6e471ad32b038d583d7fc8e32b7e8d68c19b03cdfad4b33811c061040329f6a2fd9714d5
    return value: c11d4b272ae99f5e7dc87dbeae633c623d1c3ee7

    $ ./bx base58check-encode --version 111 c11d4b272ae99f5e7dc87dbeae633c623d1c3ee7
    testnet address: my83oRtfWZXJ9u9ZaedY6AwkPeN7ZxL7re


    ./bx base58check-encode --version 00 c11d4b272ae99f5e7dc87dbeae633c623d1c3ee7
    mainnet bitcoin address: 1Jc6WNoghY63Nnfws5fAGFjRXemQiWqgdw


    $./bx base58check-encode --version 239 f72e8e6dd32ac3acff3b5183056555aabefb8239241db98ee5076aa90e356f2e
    testnet private key: 93Tn6J2RzLDK5edFPoj9M4Af5ZkaHXGZq7y9dPmoMFTRBNLFGsA
