FROM buildpack-deps:buster

RUN apt-get update && apt-get install -y --no-install-recommends \
                libgmp-dev \
                bison \
                flex \
    && rm -rf /var/lib/apt/lists/*

ENV PBC_VERSION 0.5.14

RUN set -ex \
        \
        && wget -O pbc.tar.gz "https://crypto.stanford.edu/pbc/files/pbc-$PBC_VERSION.tar.gz" \
        && mkdir -p /usr/src/pbc \
        && tar -xzC /usr/src/pbc --strip-components=1 -f pbc.tar.gz \
        && rm pbc.tar.gz \
	    \
	    && cd /usr/src/pbc \
        && ./configure \
        && make \
        && make install \
        && rm -rf /usr/src/pbc
