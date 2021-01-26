# How-to-run-Cardano-Node
The program itself is called cardano-sl which stands for [Settlement Layer](https://cardanodocs.com/introduction/) and they'll be releasing the Computation Layer soon. If you are here because you are stuck on an error during install/config, then scroll to the bottom to see the list of errors I encountered.

The first set of instructions we use is:
https://github.com/input-output-hk/cardano-sl/blob/develop/docs/how-to/build-cardano-sl-and-daedalus-from-source-code.md
and the second set is a command from:
https://github.com/input-output-hk/cardano-sl/blob/develop/docs/how-to/connect-to-cluster.md
So I built this tutorial because all of that was not readily apparent to me and I want other web developers like me to have an easy time figure this all out!


## Getting Started

1. Follow all instructions under 'Common Build Steps' from here:
   ```
   https://github.com/input-output-hk/cardano-sl/blob/develop/docs/how-to/build-cardano-sl-and-daedalus-from-source-code.md
   ```
   Tip - If you install nix and ever get the error 'command nix-build not found', see note below*

2. Follow instructions under 'Nix Mode Build (recommended)' from the same above link. BUT when you get to step 2 in 'Nix Mode Build (recommended)' do:
   ```
   $ nix-build -A cardano-sl-static --cores 0 --max-jobs 2 --no-build-output --out-link master
   ```
   instead of:
   ```
   $ nix-build -A cardano-sl-node-static --cores 0 --max-jobs 2 --no-build-output --out-link master
   ```
   Explanation as to why you do this**

   And skip the step that says `ls master/bin` and don't bother trying to run `master/bin/cardano-mode-simple` or you'll get    an error***

   Instead, do this:

3. Build a better startup script by running:
   `nix-build -A connectScripts.mainnetWallet -o connect-to-mainnet`
   This starts your node and connects it to a wallet as well as the mainnet. The script this produces ends up in parent-    level of your cardano project: ~/path-to-your/cardano/connect-to-mainnet` so `cd` there and run it by:
   `./connect-to-mainnet`


## Troubleshooting Notes
\* Maybe its because the install updated your `bash` files but you're using something like `zshell` instead, this was my problem. So change into bash shell by doing `exec bash`, then continue the tutorial. Note - I tried updating my `/etc/zshrc` file with what the install did to `/etc/bashrc` but it wouldn't let me, despite using `chmod` commands on it to change file read/write permissions.

** If you use `cardano-sl-node-static` then you'll get error `error: attribute ‘cardano-sl-node-static’ in selection path ‘cardano-sl-node-static’ not found`
Here is the (Issue)[https://github.com/input-output-hk/cardano-sl/issues/2212] and (PR)[https://github.com/input-output-hk/cardano-sl/pull/2186/commits/c0395e6892140e5bd12d881b2b880429c9f81698] that explains why you need to do this

*** error you'll get would be `cardano-node-simple: MissingSystemStartTime`




# Old Readme
01/26/2021


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
