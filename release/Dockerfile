FROM ubuntu
LABEL authors="nibbles83"

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates curl wget apt-utils jq

# install metrix binaries
RUN set -ex \
    && echo `curl -s https://api.github.com/repos/TheLindaProjectInc/metrix/releases/latest | jq -r ".assets[] | select(.name | test(\"metrix-linux-x64.tar.gz\")) | .browser_download_url"` > /tmp/metrix_url \
    && METRIX_URL=`cat /tmp/metrix_url` \
    && METRIX_DIST=$(basename $METRIX_URL) \
    && wget -O $METRIX_DIST $METRIX_URL \
	&& tar -xzvf $METRIX_DIST -C /usr/local/bin \
	&& rm /tmp/metrix*

# create data directory
ENV METRIX_DATA /data
RUN mkdir $METRIX_DATA \
	&& ln -sfn $METRIX_DATA /root/.metrixcoin \
VOLUME /data

EXPOSE 33830 33831 33840 33841
CMD ["metrixd"]
