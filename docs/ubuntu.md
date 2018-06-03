

Install On Ubuntu 16.04
=======================

Prerequisites
=============
Install these dependencies:
```
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update && sudo apt-get install python3.6 python3.6-dev python3-leveldb libleveldb-dev python3-pip -y 
sudo pip3 install --upgrade pip
sudo python3.6 -m pip install --upgrade pip setuptools wheel
sudo pip3 install aiohttp pylru leveldb plyvel aiorpcx scrypt
```


Re-Index Geysercoin Daemon 
=======================

Stop the geysercoin daemon by:

```
./geysercoind stop
```

Open the `geysercoin.conf` file & Add ` txindex=1` on it. If your coin support staking then you have to disable it by:

```
rpcuser=
rpcpassword=
addnode=185.90.163.251
addnode=78.24.221.136
addnode=92.51.46.40
addnode=87.98.142.204
rpcport=10555
port=10556
enableaccounts=1
staking=0
server=1
daemon=1
txindex=1
```

Then start the geysercoin daemon by:

```
./geysercoind -reindex
```
It's a one time process & Then you can stop & start the geysercoin daemon without `-reindex` at anytime.

Running The ElectrumX Server
============================

Install the prerequisites above.

Check out the code from Github:
```
git clone https://github.com/knkrth/electrumx.git
cd electrumx
```
You can now install with file:`setup.py`
```
sudo python3.6 setup.py install
```
Next create a directory where the database will be stored. I recommend this directory
live on an SSD:
```
mkdir ~/.electrumx
```

Set the Environment Variable & Start The ElectrumX Server
---------------------------------------------------------
Set the environmental variable as per your config:
```
export DB_DIRECTORY=~/.electrumx
export DAEMON_URL=http://<bitcoin.conf rpcuser>:<bitcoin.conf rpcpassowrd>@localhost:<rpcport>/
export COIN=Geysercoin
export ALLOW_ROOT=1
export HOST=0.0.0.0
export BANNER_FILE=~/.electrumx/banner.txt
export TCP_PORT=50001
export DONATION_ADDRESS=GZ6xb2FxpzGfnsHMktAKLR2abubSLUkDs3
```
Now we can start the electrumx server::
```
sudo python3.6 electrumx_server.py
```
If you got no error means everything is good.

Run The ElectrumX Server In The Background
------------------------------------------
Stop the electrumx server & run this command:
```
nohup sudo python3.6 electrumx_server.py &
```
It might take a minute to start the server. The server output can be logged in `nohup.out`

To stop the server run this command:
```
ps -ef
```
You will get the output like:
```
UID          PID    PPID  C STIME TTY          TIME CMD
ubuntu      7730    7061  0 05:56 pts/6    00:00:10 electrumx_server.py
ubuntu      7738    7061  0 05:56 pts/6    00:00:00 ps -ef
```
Kill the PID by:
```
sudo kill 7730
```
You will get the output like:
```
[1]+  Killed                  nohup python electrumx_server.py
```

For full instruction see https://github.com/kyuupichan/electrumx/blob/master/docs/HOWTO.rst
