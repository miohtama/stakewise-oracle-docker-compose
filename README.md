
Docker Compose for setting up [Stakewise Oracle](https://github.com/stakewise/oracle).

- Ethereum 1: Get a hosted node from [QuikNode](https://www.quiknode.io/)

- Ethereum 2: Run Prysm through Docker Compose and have all data in local `data` folder

- Oracle: Run Docker Compose and use the image from Stakewise =

# Prerequisites

* Ubuntu Linux or macOS

* Docker

# Setup

### Tools

wscat - needed for testing if Websocket connections work:

```sh
npm install -g wscat
```

### Setting up Telegram

You need a bot id and and a chat id. You can get there from Telegram [botfather](https://core.telegram.org/bots).

* Create a bot - you get `NOTIFIERS_TELEGRAM_TOKEN`

* Create a groupt chat and invite your bot there

* Type `/hello` in the group chat

* Run `curl https://api.telegram.org/bot$NOTIFIERS_TELEGRAM_TOKEN/getUpdates | jq` to get a chat id

* Save chat id as `NOTIFIERS_TELEGRAM_CHAT_ID`

### Setting up secrets

Generate [a private key](https://ethereum.stackexchange.com/q/82926/620).

Send 0.1 ETH to your Oracle private key.

Create secrets.env

```sh
export ORACLE_PRIVATE_KEY=111111111111...
export HTTP_WEB3_PROVIDER=wss://misty-withered-violet.quiknode.pro/...
export NOTIFIERS_TELEGRAM_TOKEN=
export NOTIFIERS_TELEGRAM_CHAT_ID=
```
### Test ETH1 node connection

Load configuration:

```sh
source secrets.env && source oracle-settings.env
```
### Test ETH1 webocket

Use wscat:

```sh
wscat -c ${WEB3_WS_ENDPOINT}
```

After it is connected, test with a gas price RPC call:

```
{"jsonrpc":  "2.0", "id": 0, "method":  "eth_gasPrice"}
```

You should get a response:

```
{"jsonrpc":"2.0","result":"0x33ebd5f600","id":0}
```

### Test Beacon node

Test that Beacon node starts:

```sh
docker-compose up --build -V beacon
```

After a while you should see the beacon syncing:

```
eth2-beacon | time="2021-02-03 15:28:15" level=info msg="Processing deposits from Ethereum 1 chain" deposits=7680 genesisValidators=7675 prefix=powchain
```

### Test oracle

```sh
docker-compose up stakewise
```

# Run

Start everything:

```
```

# Notes

- ETH1: Using a free [Cloudflare Ethereum node](https://www.cloudflare.com/distributed-web-gateway/) is [not possible](https://community.cloudflare.com/t/ethereum-getlogs-and-128-max-entries-limitation/241204)

## Testing ETH2 over HTTPS

In the case, you are unsure if Webdsockets sork correctly, test your ETH1 node HTTPS endpoint with curl:

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


