
https://docs.cardano.org  
https://developers.cardano.org/en/  
https://forum.cardano.org/  

## Download
[Installing the node from source](https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html)

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
export MAGIC=1097911063
```
and  
```
cabal clean
cabal update

touch build.sh
vim build.sh
    cabal build
chmod u+x build.sh

Do not nohup ./build.sh &
nohup ./build.sh >/dev/null 2>&1 &
```

```
// 02/07/2021 1.25.1
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-node-1.25.1/x/cardano-node/build/cardano-node/cardano-node ~/.local/bin/
cp -p dist-newstyle/build/x86_64-linux/ghc-8.10.2/cardano-cli-1.25.1/x/cardano-cli/build/cardano-cli/cardano-cli ~/.local/bin/
```

[Cardano Configurations](https://hydra.iohk.io/build/5102327/download/1/index.html)

## Run - Stake Pool Course
Make sure the Protocol in `testnet-config.json` is set to Cardano and not TPraos

```
cd relay
cardano-node run \
 --topology testnet-topology.json \
 --database-path db \
 --socket-path db/node.socket \
 --host-addr 0.0.0.0 \
 --port 3001 \
 --config testnet-config.json

// or, Tested, does not create nohup.out file that will suck up all your disk space 
nohup ./run.sh >& /dev/null &
```

open a new terminal session and:

```
cardano-cli shelley query tip --testnet-magic 1097911063
```
output:  
```
{
    "blockNo": 324839,
    "headerHash": "2aa319360e076b263e3f6b2b5772ca3d6d6bd9e206bb3c3a7624e6a8c4fb3358",
    "slotNo": 325881
}
```

```
cardano-cli get-tip --testnet-magic 1097911063
```
output:
```
Current tip: 
Block hash: 9241cb65e1218f8b0cabeba899d9ad560eaa871ad3f1ca5bc115119d874bc898
Slot: 18342960
Block number: 2298369
```

`free` to see disk space

tail -f nohup.out
killall cardano-node

## Run - Stake Pool Course - Make vkey skey and address
`mkdir ~/keys` and follow  [instructions](https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/stake-pool-operations/keys_and_addresses)

vkey verification key
skey signing key

^ use these to make a payment address

Check balance at the address
```
cardano-cli query utxo \
--mary-era \
--address $(cat payment.addr) \
--testnet-magic 1097911063
```

`cardano-cli get-tip --testnet-magic $MAGIC`

1. Get test ADA from the faucet:  
https://developers.cardano.org/en/testnets/cardano/tools/faucet/

2.
```
Success
Your transaction has been successful and 1000 ADA have been sent to addr_test1vqmn9dphgt5df90y34evf8jgv7ukl7rllwyhd06q55azsfs2v9ekv.

Please verify the following transaction hash:

9c010b50cfacc0530aff9a100d30cb13511adb9cefc73711c1439681e400c925
```

verified on cardano testnet explorer:  
https://explorer.cardano-testnet.iohkdev.io/en/transaction?id=9c010b50cfacc0530aff9a100d30cb13511adb9cefc73711c1439681e400c925

re-run `cardano-cli query utxo` to check.


3. When you have finished using your test tokens, please return them to the faucet so that other members of the community can use them. Please return your test tokens to this address:
```
addr_test1qqr585tvlc7ylnqvz8pyqwauzrdu0mxag3m7q56grgmgu7sxu2hyfhlkwuxupa9d5085eunq2qywy7hvmvej456flknswgndm3
```

## Generate transaction
Make protocol params in protocol.json
```
   cardano-cli query protocol-parameters \
   --mary-era \
   --testnet-magic 1097911063 \
   --out-file protocol.json
```

Get the latest SlotNo.
```
cardano-cli query tip --testnet-magic 1097911063
```
1 slot per second, and need 20 minutes, so add 1200 to the SlotNo.
18363437 becomes 18364637

```
// Get the TxId (index#1)
cardano-cli query utxo \
--mary-era \
--address $(cat payment.addr) \
--testnet-magic 1097911063

// Build Tx - Paste the transaction hash into --tx-in:  
cardano-cli transaction build-raw \ 
--tx-in 9c010b50cfacc0530aff9a100d30cb13511adb9cefc73711c1439681e400c925#1 \ 
--tx-out $(cat payment2.addr)+100000000 \ 
--tx-out $(cat payment.addr)+0 \ 
--ttl 0 \ 
--fee 0 \ 
--out-file tx.raw
```
```
// Syntax that worked
cardano-cli transaction build-raw --tx-in 9c010b50cfacc0530aff9a100d30cb13511adb9cefc73711c1439681e400c925#1 --tx-out $(cat payment2.addr)+100000000 --tx-out $(cat payment.addr)+0 --ttl 0 --fee 0 --out-file tx.raw
```
Generates tx.raw

Build Tx w/ fee
cardano-cli transaction build-raw \
--tx-in 9c010b50cfacc0530aff9a100d30cb13511adb9cefc73711c1439681e400c925#1 \
--tx-out $(cat payment2.addr)+100000000 \
--tx-out $(cat payment.addr)+899825567 \
--ttl 18364637 \
--fee 174433 \
--out-file tx.raw

cardano-cli transaction sign \
--tx-body-file tx.raw \
--signing-key-file payment.skey \
--testnet-magic 1097911063 \
--out-file tx.signed

cardano-cli transaction submit \
        --tx-file tx.signed \
        --testnet-magic 1097911063

        cardano-cli transaction submit \
        --tx-file tx.signed \
        --testnet-magic 42

## Monitor & Shutdown
Prometheus and EKG (haskel)  

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

### Upgrade Path
[backup your binaries](https://cardano-foundation.gitbook.io/stake-pool-course/stake-pool-guide/getting-started/install-node)

```
cd cardano-node
git fetch --all --tags
git tag
git checkout tags/<the-tag-you-want>
cabal update
cabal build cardano-node cardano-cli
```
So not `cabal build all`. I did cabal build all and it worked fine.

### Stake Pool Documentation
[Staking and delegating for beginners - forum.cardano.org](https://forum.cardano.org/t/staking-and-delegating-for-beginners-a-step-by-step-guide/36681)

https://cardano-community.github.io/guild-operators/#/

[Staking & Delegation](https://forum.cardano.org/c/staking-delegation/156) for support running a node  
https://forum.cardano.org/t/need-help-with-cardano-node-operation/44076/4

[Shelley is delivered](https://forum.cardano.org/t/shelley-is-delivered/36502)

[Cardano Foundation announces its delgation methodology](https://forum.cardano.org/t/cardano-foundation-announces-its-delegation-methodology/41090)

[Cardano Staking: Everything You Need to Know About ADA Returns](https://cryptobriefing.com/cardano-staking-ada-returns/)

[How to build a haskell stakepool node - coincashew](https://www.coincashew.com/coins/overview-ada/guide-how-to-build-a-haskell-stakepool-node)

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
https://developers.cardano.org/en/testnets/cardano/tools/staking-calculator/  

## Additional Ways to Get Up and Running
https://hydra.iohk.io/build/5288424 executable  
https://github.com/input-output-hk/cardano-node/releases full project

[Docker cardano-node](https://hub.docker.com/r/inputoutput/cardano-node)  
[Docker nessus-cardano](https://github.com/tdiesler/nessus-cardano)  

> "This effort aims to provide the best-possible container image for Cardano"  
It uses https://hub.docker.com/r/nessusio/cardano and [Dockerfile](https://github.com/tdiesler/nessus-cardano/blob/master/node/docker/Dockerfile)

http://astorpool.org/

### Organizations & Events
https://cardanosummit.iohk.io/

https://iohk.io/

https://cardano.org/

https://emurgo.io/

N2 type. $24 -> $57


`ps fjx`
![HealthyRunningNode](./img/healthy-running-node.png)

https://stackoverflow.com/questions/10408816/how-do-i-use-the-nohup-command-without-getting-nohup-out  
https://unix.stackexchange.com/questions/23010/how-to-make-nohup-not-create-any-output-files-and-so-not-eat-all-space says:  
nohup ./build.sh >& /dev/null &
