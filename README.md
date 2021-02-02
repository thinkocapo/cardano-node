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


## Setup - Stake Pool Course (Build from Source)
Instructions:  
https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/getting-started/install-node  

https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html#build-and-install-the-node  

#### Machine
2 CPU  
8GB RAM per [release 1.24.2](https://github.com/input-output-hk/cardano-node/releases)
24GB SSD  
Ubuntu 20.04 LTS  


```
export PATH="~/.local/bin:$PATH"

export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"

export CARDANO_NODE_SOCKET_PATH=~/relay/db/node.socket
```
and make sure you 
```
cabal clean
cabal update

touch build.sh
vim build.sh
    cabal build
chmod u+x build.sh

NOOOO nohup ./build.sh &
nohup ./build.sh >/dev/null 2>&1 &

https://stackoverflow.com/questions/10408816/how-do-i-use-the-nohup-command-without-getting-nohup-out



https://unix.stackexchange.com/questions/23010/how-to-make-nohup-not-create-any-output-files-and-so-not-eat-all-space says:
nohup ./build.sh >& /dev/null &


```

Skip `cabal install all --bindir ~/.local/bin` and run:
```
// check version...
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.24.2/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.24.2/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
```

Follow the mkdir relay steps...

### Run - Stake Pool Course
Make sure testnet-config.json' Protocol is set to Cardano and not TPraos

```
cd relay
cardano-node run \
 --topology testnet-topology.json \
 --database-path db \
 --socket-path db/node.socket \
 --host-addr 0.0.0.0 \
 --port 3001 \
 --config testnet-config.json

// or 
nohup ./run.sh >& /dev/null &
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
note - continues running after X'ing out the terminal, so may not need `nohup ./run.sh &`
note - HOWEVER using `nohup` gives you a nohup.out

8:06p nohup, then `free`, keep seeing available disk decline...

tail -f nohup.out
killall cardano-node

## Monitor & SHUTDOWN
CPU | RAM | Disk Space  
`df -h`  
`htop`  

`free`  and https://www.binarytides.com/linux-command-check-memory-usage/
```
free -h
free -s 3
```

TRY `df -h` too!

On a log file...
```
tail -f nohup.out
```

size of db...
```
du -h
```
Prometheus/Grafana https://forum.cardano.org/t/cardano-stake-pool-monitoring-with-prometheus-grafana/38047  

scripts benchmarking


killall cardano-node


## Next
- 24GB HD
- MONITOR'ing basics?
    COMES WITH TMUX - SET THAT UP
- RUN node
    tmux monitor the ls, du, dh,
    `rm nohup.out` after build but before running
    `tail -f /logs`

- SSH TO VM
https://cloud.google.com/compute/docs/instances/connecting-advanced  
https://cloud.google.com/compute/docs/instances/connecting-advanced#provide-key  



- GRACEFUL SHUTDOWN
    - Setup systemd service (start/stop, or restart)
    - Systemd
       https://www.youtube.com/watch?v=JXIaQevXlvg  
       https://github.com/DamjanOstrelic/Cardano-stuff  
       https://forum.cardano.org/t/systemd-service-file-for-cardano-node/33490/8
- STAKE POOL COURSE
- RASPBERRY PI
- github issue the Untitled section in stake pool school. videos
- try running docker container / make my own
- try running a Shelley instead of Byron, which features [cardano-cli shelley system stop - command](https://docs.cardano.org/projects/cardano-node/en/latest/reference/cardano-node-cli-reference.html)
- do Exercises from docs.cardano.org

## Questions
- Q. Building from Source vs Download Executable vs Docker. put some discussion here for onlookers.
- Q. why would you want to 'The Byron genesis generation operations will create a directory that contains:'
security? vs. use the one that comes with it
- Q. need run Byron to Shelley script?


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

https://iohk.zendesk.com/hc/en-us/articles/900001219843-What-are-Block-producing-nodes-and-relay-nodes

### Organizations & Events
https://cardanosummit.iohk.io/

https://iohk.io/

https://cardano.org/

https://emurgo.io/

N2 type. $24 -> $57


`ps fjx`
![HealthyRunningNode](./img/healthy-running-node.png)