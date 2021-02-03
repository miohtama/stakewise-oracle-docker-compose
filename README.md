
Docker Compose for setting up [Stakewise Oracle](https://github.com/stakewise/oracle).

- ETH1: Get a node from [QuikNode](https://www.quiknode.io/)

- ETH2: Run Prysm and have all data in local `eth2` folder

- Oracle: Run within a Docker from Stakewise Docker hub

## Prerequisites

* Ubuntu Linux or macOS

* Docker

## Setup

### Tools

wscat:

```sh
npm install -g wscat
```

### Setting up secrets

Generate [a private key](https://ethereum.stackexchange.com/q/82926/620).

Get a Websock and HTTPS endpoint for an Ethereum 1 node.

Create secrets.env

```sh
export ORACLE_PRIVATE_KEY=
export WEB3_WS_ENDPOINT=
export HTTP_WEB3_PROVIDER=
```
### Test ETH1 node connection

Load configuration:

```sh
source secrets.env && source oracle-settings.env
```

Test your ETH1 node API with curl:

```sh
curl --location --request POST ${HTTP_WEB3_PROVIDER} \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "jsonrpc":"2.0",
    "method":"eth_blockNumber",
    "params":[],
    "id":1
  }'
```

You get a block number as hex in the reply:

```
{"jsonrpc":"2.0","id":1,"result":"0xb3cf53"}%
```

### Test Beacon node

Test that Beacon node starts:

```sh
docker-compose up --build -V beacon
```

# Notes

- ETH1: Using a free [Cloudflare Ethereum node](https://www.cloudflare.com/distributed-web-gateway/) is [not possible](https://community.cloudflare.com/t/ethereum-getlogs-and-128-max-entries-limitation/241204)



