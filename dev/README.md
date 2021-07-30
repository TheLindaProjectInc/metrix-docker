# Quickstart

This Dockerfile trace the latest dev version of metrix.

## Get docker image

You might take either way:

### Pull a image from Public Docker hub

```
$ docker pull metrixcoin/metrix:dev
```

### Or, build metrix image with provided Dockerfile

This is recommended since it ensures build the latest dev version.

```
$docker build --rm -t metrixcoin/metrix:dev .
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
```
## Launch metrixd

To launch metrix node:

```
## to launch metrixd
$ docker run -d --rm --name metrix_node -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf -v /data/metrix-data/:/root/.metrixcoin/ metrixcoin/metrix:dev metrixd

## check docker processed
$ docker ps

## to stop metrixd
$ docker run -i --network container:metrix_node -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf -v /data/metrix-data/:/root/.metrixcoin/ metrixcoin/metrix:dev metrix-cli stop
```

`${PWD}/metrix.conf` will be used, and blockchain data saved under /data/metrix-data/

## Interact with `metrixd` using `metrix-cli`

Use following docker command to interact with your metrix node with `metrix-cli`:

```
$ docker run -i --network container:metrix_node -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf -v /data/metrix-data/:/root/.metrixcoin/ metrixcoin/metrix:dev metrix-cli getblockchaininfo
```

For more metrix-cli commands, use:

```
$ docker run -i --network container:metrix_node -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf -v /data/metrix-data/:/root/.metrixcoin/ metrixcoin/metrix:dev metrix-cli help
```

