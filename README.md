# How-to-run-Cardano-Node
The program itself is called cardano-sl which stands for [Settlement Layer](https://cardanodocs.com/introduction/) and they'll be releasing the Computation Layer soon. If you are here because you are stuck on an error during install/config, then scroll to the bottom to see the list of errors I encountered.

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
