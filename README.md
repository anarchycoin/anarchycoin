#Anarchycoin integration/staging tree
================================
Copyright (c) 2009-2013 Bitcoin Developers<br>
Copyright (c) 2011-2013 Litecoin Developers<br>
Copyright (c) 2011-2014 Anarchycoin Developers<br>

![Anarchycoin](https://raw.githubusercontent.com/gamers-coin/gamers-coinv3/01d1ca6d63b565ea46dcee3b6552b030d57d1187/src/qt/res/icons/bitcoin.png)![Anarchycoin](http://i.imgur.com/Nfb8DQx.png)

#What is Anarchycoin?
----------------

Anarchycoin is a lite version of Bitcoin using scrypt as a proof-of-work algorithm.
 - 2.5 minute block targets
 - subsidy halves in 840k blocks (~4 years)
 - ~84 million total coins
 - 50 coins per block
 - 1 blocks to retarget difficulty
 - KGW Implemented


#License
-------


#Development process
-------------------

Developers work in their own trees, then submit pull requests when they think
their feature or bug fix is ready.

If it is a simple/trivial/non-controversial change, then one of the Anarchycoin
development team members simply pulls it.

The patch will be accepted if there is broad consensus that it is a good thing.
Developers should expect to rework and resubmit patches if the code doesn't
match the project's coding conventions (see `doc/coding.txt`) or are
controversial.

The `master` branch is regularly built and tested, but is not guaranteed to be
completely stable. [Tags](https://github.com/bitcoin/bitcoin/tags) are created
regularly to indicate new official, stable release versions of Anarchycoin.

#Compiling the Anarchycoin daemon from source on Debian
-----------------------------------------------------
The process for compiling the Anarchycoin daemon, anarchycoind, from the source code is pretty simple. This guide is based on the latest stable version of Debian Linux, though it should not need many modifications for any distro forked from Debian, such as Ubuntu and Xubuntu.

###Update and install dependencies

```
apt-get update && apt-get upgrade
apt-get install ntp git build-essential libssl-dev libdb-dev libdb++-dev libboost-all-dev libqrencode-dev

wget http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.8.tar.gz && tar -zxf download.php\?file\=miniupnpc-1.8.tar.gz && cd miniupnpc-1.8/
make && make install && cd .. && rm -rf miniupnpc-1.8 download.php\?file\=miniupnpc-1.8.tar.gz
```
Note: Debian testing and unstable require libboost1.54-all-dev.

###Compile the daemon
```
git clone https://github.com/anarchycoin/anarchycoin
```

###Compile anarchycoind
```
cd anarchycoin/src/
make -f makefile.unix USE_UPNP=1 USE_QRCODE=1 USE_IPV6=1
strip anarchycoind
```

###Add a user and move anarchycoind
```
adduser anarchycoin && usermod -g users anarchycoin && delgroup anarchycoin && chmod 0701 /home/anarchycoin
mkdir /home/anarchycoin/bin
cp ~/anarchycoin/src/anarchycoind /home/anarchycoin/bin/anarchycoind
chown -R anarchycoin:users /home/anarchycoin/bin
cd && rm -rf anarchycoin
```

###Run the daemon
```
su anarchycoin
cd && bin/anarchycoind
```

On the first run, anarchycoind will return an error and tell you to make a configuration file, named anarchycoin.conf, in order to add a username and password to the file.
```
nano ~/.anarchycoin/anarchycoin.conf && chmod 0600 ~/.anarchycoin/anarchycoin.conf
```
Add the following to your config file, changing the username and password to something secure: 
```
daemon=1
rpcuser=<username>
rpcpassword=<secure password>
```

You can just copy the username and password provided by the error message when you first ran anarchycoind.

Run anarchycoind once more to start the daemon! 

###Using anarchycoind
```
anarchycoind help
```

The above command will list all available functions of the Anarchycoin daemon. To safely stop the daemon, execute Anarchycoind stop. 

#Testing
-------

Testing and code review is the bottleneck for development; we get more pull
requests than we can review and test. Please be patient and help out, and
remember this is a security-critical project where any mistake might cost people
lots of money.

### Automated Testing

Developers are strongly encouraged to write unit tests for new code, and to
submit new unit tests for old code.

Unit tests for the core code are in `src/test/`. To compile and run them:

    cd src; make -f makefile.unix test

Unit tests for the GUI code are in `src/qt/test/`. To compile and run them:

    qmake BITCOIN_QT_TEST=1 -o Makefile.test bitcoin-qt.pro
    make -f Makefile.test
    ./anarchycoin-qt_test

