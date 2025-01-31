# Ulord - Node Open Mining Portal


This is a Cryptohello mining pool based on the Node Open Mining Portal.

Usage
=====


#### Requirements
* Coin daemon(s) (find the coin's repo and build latest version from source)
* [Node.js](http://nodejs.org/) v4.8.7 ([follow these installation instructions](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager))
* [Redis](http://redis.io/) key-value store v2.6+ ([follow these instructions](http://redis.io/topics/quickstart))

##### Seriously
Those are legitimate requirements. If you use old versions of Node.js or Redis that may come with your system package manager then you will have problems. Follow the linked instructions to get the last stable versions.


[**Redis security warning**](http://redis.io/topics/security): be sure firewall access to redis - an easy way is to
include `bind 127.0.0.1` in your `redis.conf` file. Also it's a good idea to learn about and understand software that
you are using - a good place to start with redis is [data persistence](http://redis.io/topics/persistence).


#### 0) Setting up coin daemon
Follow the build/install instructions for your coin daemon.Your ulord.conf file should end up looking something like this(you can find
this file at a dir named `.ulordcore`):
```
testnet=0
rpcuser=ulordpool
rpcpassword=ulordpool
txindex=1
addressindex=1
spentindex=1
timestampindex=1
reindex=0
```

#### 1) Downloading & Installing

Clone the repository and run `npm update` for all the dependencies to be installed:

```bash
sudo apt-get install build-essential libsodium-dev npm
sudo npm install n -g
sudo n 4.8.7
git clone https://github.com/UlordChain/ulord-mm-pool.git
cd ulord-mm-pool
npm update
```

##### 2) The overall configuration

You must change the daemon address to the same as the step 0 configuration. Note that there are two places to change. If your redis is configured with a password, modify it together.Configuration file located in `./config.json`.

The config_example.json file adds the USC configuration switch as follows:
```
"usc":{
    "enabled": true,
    "rpcIp": "localhost",
    "rpcPort": 58858
}
```

##### 3) Pool config

You can custom your configuration by modify file `pool_config/ulord.json`.The most important thing is to change pool address to yours.

```
Please Note that: 1 Difficulty is actually 8192, 0.125 Difficulty is actually 1024.

Whenever a miner submits a share, the pool counts the difficulty and keeps adding them as the shares. 

ie: Miner 1 mines at 0.1 difficulty and finds 10 shares, the pool sees it as 1 share. Miner 2 mines at 0.5 difficulty and finds 5 shares, the pool sees it as 2.5 shares. 
```

##### [Optional, recommended] Setting up blocknotify
1. In `config.json` set the port and password for `blockNotifyListener`
2. In your daemon conf file set the `blocknotify` command to use:
```
node [path to cli.js] [coin name in config] [block hash symbol]
```
Example: inside `ulord.conf` add the line
```
blocknotify=node /home/user/z-nomp/scripts/cli.js blocknotify ulord %s
```

Alternatively, you can use a more efficient block notify script written in pure C. Build and usage instructions
are commented in [scripts/blocknotify.c](scripts/blocknotify.c).


#### 3) Start the portal

```bash
npm start
```

###### Optional enhancements for your awesome new mining pool server setup:
* Use something like [forever](https://github.com/nodejitsu/forever) to keep the node script running
in case the master process crashes. 
* Use something like [redis-commander](https://github.com/joeferner/redis-commander) to have a nice GUI
for exploring your redis database.
* Use something like [logrotator](http://www.thegeekstuff.com/2010/07/logrotate-examples/) to rotate log 
output from Z-NOMP.
* Use [New Relic](http://newrelic.com/) to monitor your Z-NOMP instance and server performance.

#### Upgrading Z-NOMP
Our ulord pool based on Z-nomp. When updating Z-NOMP to the latest code its important to not only `git pull` the latest from this repo, but to also update
the `node-merged-pool` and `node-multi-hashing` modules, and any config files that may have been changed.
* Inside your Z-NOMP directory (where the init.js script is) do `git pull` to get the latest Z-NOMP code.
* Remove the dependenices by deleting the `node_modules` directory with `rm -r node_modules`.
* Run `npm update` to force updating/reinstalling of the dependencies.
* Compare your `config.json` and `pool_configs/coin.json` configurations to the latest example ones in this repo or the ones in the setup instructions where each config field is explained. <b>You may need to modify or add any new changes.</b>


Donations
-------
To support development of this project feel free to donate :)
* UT: UbZnu7QCHcate5RMmequjt1W4zuVT6KDSa

PS: The donated UT flows to Ulord community foundation and is supervised by the community.


Credits
-------
Z-NOMP
* [Joshua Yabut / movrcx](https://github.com/joshuayabut)
* [Aayan L / anarch3](https://github.com/aayanl)
* [hellcatz](https://github.com/hellcatz)

NOMP
* [ZyanWlayor / UlordChain](https://github.com/UlordChain/) - Ulord
* [Jerry Brady / mintyfresh68](https://github.com/bluecircle) - got coin-switching fully working and developed proxy-per-algo feature
* [Tony Dobbs](http://anthonydobbs.com) - designs for front-end and created the NOMP logo
* [LucasJones](//github.com/LucasJones) - got p2p block notify working and implemented additional hashing algos
* [vekexasia](//github.com/vekexasia) - co-developer & great tester
* [TheSeven](//github.com/TheSeven) - answering an absurd amount of my questions and being a very helpful gentleman
* [UdjinM6](//github.com/UdjinM6) - helped implement fee withdrawal in payment processing
* [Alex Petrov / sysmanalex](https://github.com/sysmanalex) - contributed the pure C block notify script
* [svirusxxx](//github.com/svirusxxx) - sponsored development of MPOS mode
* [icecube45](//github.com/icecube45) - helping out with the repo wiki
* [Fcases](//github.com/Fcases) - ordered me a pizza <3
* Those that contributed to [node-merged-pool](//github.com/UlordChain/node-merged-pool#credits)


License
-------
Released under the MIT License. See [LICENSE](https://github.com/UlordChain/ulord-mm-pool/blob/master/LICENSE) file.


