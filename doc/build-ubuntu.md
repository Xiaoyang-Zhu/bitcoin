Ubuntu Version: 17.10 desktop image

download from site [http://releases.ubuntu.com/17.10.1/]

To Build
---------------------

```bash
./autogen.sh
./configure
make
make install # optional
```


Dependency Build Instructions: Ubuntu & Debian
----------------------------------------------
Build requirements:

    sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3

Options when installing required Boost library files:

    sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev


db4.8 packages are available [here](https://launchpad.net/~bitcoin/+archive/bitcoin).
You can add the repository and install using the following commands:

    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev


Optional (see --with-miniupnpc and --enable-upnp-default):

    sudo apt-get install libminiupnpc-dev

ZMQ dependencies (provides ZMQ API 4.x):

    sudo apt-get install libzmq3-dev

Dependencies for the GUI: Ubuntu & Debian
-----------------------------------------

To build with Qt 5 (recommended) you need the following:

    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler


libqrencode (optional) can be installed with:

    sudo apt-get install libqrencode-dev

Once these are installed, they will be found by configure and a bitcoin-qt executable will be
built by default.
