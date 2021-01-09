### Documentation & Forum
https://docs.cardano.org/projects/cardano-node/en/latest/index.html

https://forum.cardano.org/

https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/understanding-config-files.html

[Installing the node from source](https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/install.html)

*Download it:*
https://hydra.iohk.io/build/5288424 executable
https://github.com/input-output-hk/cardano-node/releases full project

[How to build a haskell stakepool node - coincashew](https://www.coincashew.com/coins/overview-ada/guide-how-to-build-a-haskell-stakepool-node)

[Docker cardano-node](https://hub.docker.com/r/inputoutput/cardano-node)
[Docker nessus-cardano](https://github.com/tdiesler/nessus-cardano)  
> "This effort aims to provide the best-possible container image for Cardano"
It uses https://hub.docker.com/r/nessusio/cardano and [Dockerfile](https://github.com/tdiesler/nessus-cardano/blob/master/node/docker/Dockerfile)

https://developers.cardano.org/ not much

http://astorpool.org/

## Setup
1. wget -c https://hydra.iohk.io/build/5288424/download/1/cardano-node-1.24.2-linux.tar.gz
2. tar -xzf ../cardano-node-1.24.2-linux.tar.gz
3. touch state

```
cardano-cli 1.24.2 - linux-x86_64 - ghc-8.10
git rev 8e0501f4352a00a00330dc7641b1a7583c52e643

./cardano-node --version
cardano-node 1.24.2 - linux-x86_64 - ghc-8.10
git rev 8e0501f4352a00a00330dc7641b1a7583c52e643
```

## Run
1. 
```
./cardano-node run --topology ./configuration/cardano/mainnet-topology.json --database-path ./state --port 3001 --config ./configuration/defaults/byron-mainnet/configuration.yaml --socket-path \\.\pipe\cardano-node
```

## Next
- CORRECT is this correct running?
- BILLING 
- READ [Understanding your configuration files and how to use them](https://docs.cardano.org/projects/cardano-node/en/latest/getting-started/understanding-config-files.html)
- run in VirtualBox
- run in background (shell, nohup?)
- how to stop a node from running [how to gracefully restart cardano node](https://forum.cardano.org/t/how-to-gracefully-restart-cardano-node/37543) maybe?

- stake pool on Testnet
- good metrics for CPU/RAM usage? monitoring - see script's benchmarking too
- `./state` database file is small, is it trying to reach something 100's GB's?
`drwxrwxr-x 5 thinkocapo thinkocapo     4096 Jan  9 17:07 state`
- preferred to run as a `systemd` service
- try running a Shelley instead of Byron, which features [cardano-cli shelley system stop - command](https://docs.cardano.org/projects/cardano-node/en/latest/reference/cardano-node-cli-reference.html)
- and slot "297556" is what? block?
```
[cardano:cardano.node.ChainDB:Info:5] [2021-01-09 17:07:49.40 UTC] Closed db with immutable tip at e7fed2607e5b613a60589c6a7bd7e00024b9ec1fcede1909a4a8c
2d4fa90376c at slot 295396 and tip 4e115fcf75a6f49914cb99288351612c2ba912ec824ec64b9724b62c277833ec at slot 297556
```

- https://github.com/input-output-hk/cardano-wallet instead of Daedalus?


- why build from sourcs vs executable vs docker
- is this file specifying the genesis.json i'm connecting to?
https://github.com/input-output-hk/cardano-node/blob/master/configuration/defaults/byron-mainnet/configuration.yaml#L9
- why would you want to 'The Byron genesis generation operations will create a directory that contains:'? security?
- should I be using https://github.com/input-output-hk/cardano-node/blob/master/configuration/defaults/byron-mainnet/topology.json instead of 


### Stake Pool
[Staking and delegating for beginners - forum.cardano.org](https://forum.cardano.org/t/staking-and-delegating-for-beginners-a-step-by-step-guide/36681)

https://cardano-community.github.io/guild-operators/#/

[Staking & Delegation](https://forum.cardano.org/c/staking-delegation/156) for support running a node  
https://forum.cardano.org/t/need-help-with-cardano-node-operation/44076/4

[Shelley is delivered](https://forum.cardano.org/t/shelley-is-delivered/36502)

[Cardano Foundation announces its delgation methodology](https://forum.cardano.org/t/cardano-foundation-announces-its-delegation-methodology/41090)

[Cardano Staking: Everything You Need to Know About ADA Returns](https://cryptobriefing.com/cardano-staking-ada-returns/)

### Other Technical
[Why Cardano Chose Haskell](https://forum.cardano.org/t/why-cardano-chose-haskell-and-why-you-should-care/43085)

[Functional Blockchain Contracts](https://iohk.io/en/research/library/papers/functional-blockchain-contracts/)

[Cardano’s real competition is not who you’d expect, says new Cardano Foundation CEO](https://forum.cardano.org/t/cardanos-real-competition-is-not-who-youd-expect-says-new-cardano-foundation-ceo/40903)

> Most blockchain programming platforms depend on a custom language, such as Ethereum’s Solidity, but Plutus is provided as a set of libraries for Haskell. Both off-chain and on-chain code are written in Haskell: off-chain code using the Plutus library, and on-chain code in a subset of Haskell using Template Haskell. On-chain code is compiled to a tiny functional language called Plutus Core,

read https://github.com/input-output-hk/cardano-node/releases

[byron-genesis](https://github.com/input-output-hk/cardano-node/blob/master/doc/reference/byron-genesis.md)

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

