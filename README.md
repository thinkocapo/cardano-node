### Documentation & Forum

https://docs.cardano.org/projects/cardano-node/en/latest/index.html
https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/understanding-config-files.html
https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/cli.html

https://forum.cardano.org/

## Download
[Installing the node from source](https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html)

https://hydra.iohk.io/build/5288424 executable  
https://github.com/input-output-hk/cardano-node/releases full project

[Docker cardano-node](https://hub.docker.com/r/inputoutput/cardano-node)  
[Docker nessus-cardano](https://github.com/tdiesler/nessus-cardano)  

> "This effort aims to provide the best-possible container image for Cardano"  
It uses https://hub.docker.com/r/nessusio/cardano and [Dockerfile](https://github.com/tdiesler/nessus-cardano/blob/master/node/docker/Dockerfile)

https://developers.cardano.org/ not much

http://astorpool.org/

## Setup (Old))
https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html  

In a VM...download the executable (not the [source code from github Releases page](https://github.com/input-output-hk/cardano-node/releases))
1. wget -c https://hydra.iohk.io/build/5288424/download/1/cardano-node-1.24.2-linux.tar.gz
2. tar -xzf cardano-node-1.24.2-linux.tar.gz
```
# Gives the following executables and some config/log dirs
ls
cardano-cli  cardano-node  cardano-node-chairman  cardano-testnet  configuration  logs

cardano-cli 1.24.2 - linux-x86_64 - ghc-8.10
git rev 8e0501f4352a00a00330dc7641b1a7583c52e643

./cardano-node --version
cardano-node 1.24.2 - linux-x86_64 - ghc-8.10
git rev 8e0501f4352a00a00330dc7641b1a7583c52e643
```
3. touch state

Change testnet-config.json's `"Protocol": "Cardano"` to `"Protocol": "Tpraos"


#### Run (Old)
```
# mainnet
./cardano-node run --topology ./configuration/cardano/mainnet-topology.json --database-path ./state --port 3001 --config ./configuration/defaults/byron-mainnet/configuration.yaml --socket-path \\.\pipe\cardano-node

```
// testnet setup
change GenesisFile: testnet_genesis.json to GenesisFile: genesis.json
change protocol in yaml to TPraos, get error There was an error parsing the genesis file: ./configuration/defaults/byron-testnet/genesis.json Error: "Error in $: key \"systemStart\" not found"
```

// testnet run
./cardano-node run --topology ./configuration/defaults/byron-testnet/topology.json --database-path ./state --port 3001 --config ./configuration/defaults/byron-testnet/configuration.yaml --socket-path \\.\pipe\cardano-node
```
and test it
```
export CARDANO_NODE_SOCKET_PATH=\\.\pipe\cardano-node
./cardano-cli byron query get-tip --mainnet
./cardano-cli query get-tip --mainnet
```

key-pair stake  
key-pair address   
payment address  
stake address  
https://docs.cardano.org/projects/cardano-node/en/latest/stake-pool-operations/keys_and_addresses.html

key-pair offline cold
https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/getting-started/cli
https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/cli.html


## Setup - Stake Pool Course (Build from Source)
https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/getting-started/install-node

```
PATH=$PATH:/home/thinkocapo/.local/bin or export PATH="~/.local/bin:$PATH"

export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"

export CARDANO_NODE_SOCKET_PATH=~/relay/db/node.socket
```
and make sure you 
```
cabal clean
cabal update
// optional - echo 'cabal build all' > build.sh and then nohup ./build.sh
cabal build
https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html#build-and-install-the-node  
```

Skip `cabal install all --bindir ~/.local/bin` and run:
```
// check version...
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.24.2/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.24.2/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
```

### Run - Stake Pool Course
cd relay

change testnet-config.json Protocol from TPraos to Cardano

```
cardano-node run \
 --topology testnet-topology.json \
 --database-path db \
 --socket-path db/node.socket \
 --host-addr 0.0.0.0 \
 --port 3001 \
 --config testnet-config.json
```

open a new terminal session and:

`export CARDANO_NODE_SOCKET_PATH=~/relay/db/node.socket`  
```
thinkocapo@dev-1:~/relay$ cardano-cli shelley query tip --testnet-magic 1097911063
WARNING: The "shelley" subcommand is now deprecated and will be removed in the future. Please use the top-level commands instead.
{
    "blockNo": 324839,
    "headerHash": "2aa319360e076b263e3f6b2b5772ca3d6d6bd9e206bb3c3a7624e6a8c4fb3358",
    "slotNo": 325881
}
```

## Questions
- Q. Building from Source vs Download Executable vs Docker. put some discussion here for onlookers.
- Q. why would you want to 'The Byron genesis generation operations will create a directory that contains:'
security? vs. use the one that comes with it
- Q. need run Byron to Shelley script?

## Next
- machineimage/snapshot after relay config files

- MONITOR'ing
    ps fjx
    Prometheus
    script's benchmarking
- RUN - run node
    run node
    new terminal in cardano-cli
    run node, in background somehowm, write output somewhere
- MONITOR - disk space, after running once...

- SSH TO VM
https://cloud.google.com/compute/docs/instances/connecting-advanced  
https://cloud.google.com/compute/docs/instances/connecting-advanced#provide-key  

- GRACEFUL SHUTDOWN
    - Setup systemd service (start/stop, or restart)
    - Systemd
       https://www.youtube.com/watch?v=JXIaQevXlvg  
       https://github.com/DamjanOstrelic/Cardano-stuff
- RASPBERRY PI

- STAKE POOL COURSE

- github issue the Untitled section in stake pool school. videos
- try running docker container / make my own
- try running a Shelley instead of Byron, which features [cardano-cli shelley system stop - command](https://docs.cardano.org/projects/cardano-node/en/latest/reference/cardano-node-cli-reference.html)
- do Exercises from docs.cardano.org

### Stake Pool
[Staking and delegating for beginners - forum.cardano.org](https://forum.cardano.org/t/staking-and-delegating-for-beginners-a-step-by-step-guide/36681)

https://cardano-community.github.io/guild-operators/#/

[Staking & Delegation](https://forum.cardano.org/c/staking-delegation/156) for support running a node  
https://forum.cardano.org/t/need-help-with-cardano-node-operation/44076/4

[Shelley is delivered](https://forum.cardano.org/t/shelley-is-delivered/36502)

[Cardano Foundation announces its delgation methodology](https://forum.cardano.org/t/cardano-foundation-announces-its-delegation-methodology/41090)

[Cardano Staking: Everything You Need to Know About ADA Returns](https://cryptobriefing.com/cardano-staking-ada-returns/)

[How to build a haskell stakepool node - coincashew](https://www.coincashew.com/coins/overview-ada/guide-how-to-build-a-haskell-stakepool-node)

### Upgrade Path
[backup your binaries](https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/getting-started/install-node)

### Other Technical
[Why Cardano Chose Haskell](https://forum.cardano.org/t/why-cardano-chose-haskell-and-why-you-should-care/43085)

[Functional Blockchain Contracts](https://iohk.io/en/research/library/papers/functional-blockchain-contracts)

[Cardano’s real competition is not who you’d expect, says new Cardano Foundation CEO](https://forum.cardano.org/t/cardanos-real-competition-is-not-who-youd-expect-says-new-cardano-foundation-ceo/40903)

> Most blockchain programming platforms depend on a custom language, such as Ethereum’s Solidity, but Plutus is provided as a set of libraries for Haskell. Both off-chain and on-chain code are written in Haskell: off-chain code using the Plutus library, and on-chain code in a subset of Haskell using Template Haskell. On-chain code is compiled to a tiny functional language called Plutus Core,

read https://github.com/input-output-hk/cardano-node/releases

[byron-genesis spec](https://github.com/input-output-hk/cardano-node/blob/master/doc/reference/byron-genesis.md)

Plutus Core is a compilation target, yet https://prod.playground.plutus.iohkdev.io/tutorial/

[Delegating in Daedalus - VIDEO](https://www.youtube.com/watch?v=VtkjM_0k4R0&feature=emb_logo)


https://docs.cardano.org/en/latest/getting-started/stake-pool-operators/

https://docs.cardano.org/en/latest/explore-cardano/cardano-network.html

https://medium.com/@contact_73710/a-non-technical-guide-for-running-a-stake-pool-part-1-a9071022d125

https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html

### Organizations & Events
https://cardanosummit.iohk.io/

https://iohk.io/

https://cardano.org/

https://emurgo.io/

N2 type. $24 -> $57