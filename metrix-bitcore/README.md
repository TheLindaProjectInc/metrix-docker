# Quickstart

This Dockerfile trace the latest dev version of metrix-bitcore, which is forked of Bitcore to work on Metrix - Not as well tested and primarily used for the extra RPC calls needed for the block explorer

## Get docker image

You might take either way:

### Pull a image from Public Docker hub

```
$ docker pull metrixcoin/metrix-bitcore:latest
```

### Or, build metrix image with provided Dockerfile

This is recommended since it ensures build the latest dev version of metrix-bitcore.

```
$docker build --rm -t metrix/metrix-bitcore:latest .
```

## Prepare data path and metrix.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save metrix block chain data:

```
sudo rm -rf /data/metrix-data
sudo mkdir -p /data/metrix-data
sudo chmod a+w /data/metrix-data
```

Create your config file, refer to the example [metrix.conf]!(https://github.com/TheLindaProjectInc/metrix/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/metrix.conf). Note rpcuser and rpcpassword to required for later `metrix-cli` usage for docker, so it is better to set those two options. Then please create the file ${PWD}/metrix.conf with content:

```
rpcuser=metrix
rpcpassword=metrixtest

# This will allow you to RPC from your localhost outside the container
rpcallowip=0.0.0.0/0
rpcbind=0.0.0.0
```

## Launch metrixd

To launch metrix node:

```
## to launch metrixd
$ docker run -d --rm --name metrix_node \
             -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf \
             -v /data/metrix-data/:/root/.metrixcoin/ \
             -p 127.0.0.1:33831:33831 \
             metrixcoin/metrix-bitcore:latest metrixd

## check docker processed
$ docker ps

## to stop metrixd
$ docker run -i --network container:metrix_node \
             -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf \
             -v /data/metrix-data/:/root/.metrixcoin/ \
             metrixcoin/metrix-bitcore:latest metrix-cli stop
```

`${PWD}/metrix.conf` will be used, and blockchain data saved under /data/metrix-data/

## Interact with `metrixd` using `metrix-cli`

Use following docker command to interact with your metrix node with `metrix-cli`:

```
$ docker run -i --network container:metrix_node \
             -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf \
             -v /data/metrix-data/:/root/.metrixcoin/ \
             metrixcoin/metrix-bitcore:latest metrix-cli getblockchaininfo
```

For more metrix-cli commands, use:

```
$ docker run -i --network container:metrix_node \
             -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf \
             -v /data/metrix-data/:/root/.metrixcoin/ \
             metrixcoin/metrix-bitcore:latest metrix-cli help
```

## RPC from outside container

While the metrix-bitcore node container is running, you can do RPC outside the container on your localhost like this:

```
curl -i --user metrix:metrixtest --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockchaininfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:33831/
```

Or for Windows replace the single quotes and escape quotes inside.

```
curl -i --user metrix:metrixtest --data-binary "{\"jsonrpc\":\"1.0\", \"id\":\"curltest\", \"method\":\"getblockchaininfo\", \"params\":[] }" -H "content-type:text/plain;" http://127.0.0.1:33831/

```