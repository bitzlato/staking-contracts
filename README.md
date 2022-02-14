# staking-contracts

Smart contracts for IBFT PoS

## Running validator node

#### Prerequisites

You should be authorized at nexus.lgk.one:5000 docker repository.

```shell
$ docker login --username <username> --password <password> https://nexus.lgk.one:5000
```

Create directory for the blockchain files

```shell
$ mkdir blockchain
$ cd blockchain
```

### Generate signer/p2p keys

```shell
$ mkdir data
$ docker run --rm --user "$(id -u):$(id -g)" --volume $(pwd)/data:/data nexus.lgk.one:5000/bzbchain:latest secrets init --data-dir /data
```

This command creates two directories inside the `data` directory:
  * `consensus` with validator's private key
  * `libp2p` with p2p's private key

The command returns:

```shell
[SECRETS INIT]
Public key (address) = 0x0123456789012345678901234567890123456789
Node ID              = 16...
```

### Download `genesis.json` 

```shell
$ wget https://raw.githubusercontent.com/bitzlato/staking-contracts/6327b4bf7a870943abb131ef5ff42e0854537835/genesis/genesis.json
```

### Run validator node

```shell
$ docker run --name bzbchain --publish 127.0.0.1:10002:8545 --volume $(pwd)/genesis.json:/genesis.json --volume $(pwd)/data:/data --user "$(id -u):$(id -g)" nexus.lgk.one:5000/bzbchain:latest server --data-dir /data --chain /genesis.json --jsonrpc 0.0.0.0:8545 --seal
```

You may look for more configuration options in the [documentation](https://edge-docs.polygon.technology/docs/get-started/cli-commands#server-flags).

## How to start staking

```shell
$ git clone https://github.com/bitzlato/staking-contracts.git
$ cd staking-contracts
$ git checkout bitzlato
$ npm i
```

### Check current total staked amount and validators in contract

Please make sure required values are set in .env to use this command. You can get signer's private key from the `./data/consensus/validator.key` file. Place the value under `PRIVATE_KEYS` variable in the `.env` file.
```shell
$ npm run info
```

### Stake balance to contract

You should have at least 1000 (+ fees) on you account to initiate staking.

```shell
$ npm run stake
```

### Unstake from contract

```shell
$ npm run unstake
```

