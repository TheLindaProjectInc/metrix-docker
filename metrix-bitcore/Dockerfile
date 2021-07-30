FROM ubuntu
LABEL authors="nibbles83"

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates git wget apt-utils \
    && apt-get install -y build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils \
    && apt-get install -y libboost-all-dev \
	&& apt-get install -y software-properties-common \
	&& add-apt-repository -y ppa:bitcoin/bitcoin \
	&& apt-get update \
	&& apt-get install -y libdb4.8-dev libdb4.8++-dev \
	&& apt-get install -y libzmq3-dev \
 	&& rm -rf /var/lib/apt/lists/*

# install metrix from github
RUN cd /tmp && git clone --recursive https://github.com/TheLindaProjectInc/metrix.git \
	&& cd metrix \
	&& ./autogen.sh \
	&& ./configure --enable-bitcore-rpc --without-gui \
	&& make install  \
	&& rm -rf /tmp/*

# create data directory
ENV METRIX_DATA /data
RUN mkdir $METRIX_DATA \
	&& ln -sfn $METRIX_DATA /root/.metrixcoin \
VOLUME /data

EXPOSE 33830 33831 33840 33841
CMD ["metrixd"]
