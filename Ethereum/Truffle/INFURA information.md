# INFURA information

## SAVE YOUR API KEY

copy and save your API key for use within your app. We've also emailed it to you.

R3aU441QwikQrtktZL6I



## CHOOSE A NETWORK

Use one of these test or production endpoints as your Ethereum client provider. Add your Access Token to the end of the URL. 

![1531395519054](C:\Users\waca\AppData\Local\Temp\1531395519054.png)

URL/infura.io/PersonalAPIKey

## JSON-RPC METHODS

Below is a quick command line example using CURL.

```
curl -X POST
  -H "Content-Type: application/json"
  --data '{"jsonrpc": "2.0", "id": 1, "method": "eth_blockNumber", "params": []}'
  "https://mainnet.infura.io/R3aU441QwikQrtktZL6I"
```

The result should be something like:

```
{"jsonrpc":"2.0","id":1,"result":"0x500263"}
```

NOTE: "0x3ccb11" will be replaced with the block number of the most recent block on the network.

```
https://api.infura.io/v1/jsonrpc/mainnet/eth_blockNumber?token=R3aU441QwikQrtktZL6I
```

