
Docker Compose for setting up [Stakewise Oracle](https://github.com/stakewise/oracle).

- ETH1: Use free configured [Cloudflare Ethereum node](https://www.cloudflare.com/distributed-web-gateway/)

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

### Oracle private key

Generate [a private key](https://ethereum.stackexchange.com/q/82926/620):

```sh
# Stored outside the root folder to any accidental check ins
openssl rand -hex 32 > ../private-key.txt
```

Configure your Web3 URL for ETH1:

```sh
echo "https://eth1.capitalgram.com" > web3.txt
```
### Test ETH1 node connection

Load configuration:

```sh
source oracle-settings.env
```

Connect Websocket:

```sh
wscat -c ws://$WEB3_WS_ENDPOINT
```

Test it with curl:

```sh
curl --location --request POST '' \
--header 'Content-Type: application/json' \
--data-raw '{
	"jsonrpc":"2.0",
	"method":"eth_blockNumber",
	"params":[],
	"id":83
}
```





