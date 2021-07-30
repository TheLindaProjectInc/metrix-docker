# Quickstart

This is a metrix-qt image, launch GUI wallet

## Get docker image

To get the latest image, you might take either way:

### Pull a image from Public Docker hub

```
$ docker pull metrixcoin/metrix-qt
```

### Or, build metrix image with provided Dockerfile

```
$docker build --rm -t metrix/metrix-qt .
```

For historical versions, please visit [docker hub](https://hub.docker.com/r/metrix/metrix-qt/)

## Prepare data path & metrix.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save metrix block chain data:

```
sudo rm -rf /data/metrix-data
sudo mkdir -p /data/metrix-data
sudo chmod a+w /data/metrix-data
```

Create your config file, refer to the example [metrix.conf]!(https://github.com/TheLindaProjectInc/metrix/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/metrix.conf). Then please create the file ${PWD}/metrix.conf with content:

```
rpcuser=metrix
rpcpassword=metrixtest
```

User can set their own config file on demands.

## Launch metrix-qt

For Linux:

```
$ docker run -it --rm \
             -v /tmp/.X11-unix:/tmp/.X11-unix \
             -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf \
             -v /data/metrix-data/:/root/.metrixcoin/ \
             -p 127.0.0.1:33831:33831 \
             -e DISPLAY  metrixcoin/metrix-qt
```

For Mac:

Please refer to
[https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc](https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc) about how to run gui with docker on mac.

```
## install & launch socat
$ brew install socat
$ socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"

## install & open Xquartz
$ brew install xquartz
$ open -a Xquartz

## then set Xquartz preferences "Security-'Allow connections from network clients'"

## launch metrix-qt 
$ docker run -e DISPLAY=<your_ip>:0 -v ${PWD}/metrix.conf:/root/.metrixcoin/metrix.conf -v /data/metrix-data/:/root/.metrixcoin/ metrix/metrix-qt

```


`${PWD}/metrix.conf` will be used, and blockchain data saved under /data/metrix-data/


## exit metrix-qt

Exit the gui wallet in normal way.


