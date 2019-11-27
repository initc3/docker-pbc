FROM debian:buster-slim

# runtime dependency -- not sure
RUN apt-get update && apt-get install -y --no-install-recommends \
		ca-certificates \
	&& rm -rf /var/lib/apt/lists/*

ENV PBC_VERSION 0.5.14

RUN set -ex \
        \
        && savedAptMark="$(apt-mark showmanual)" \
        && apt-get update && apt-get install -y --no-install-recommends \
                bison \
                flex \
                gcc \
                make \
                libc6-dev \
                libgmp-dev \
                wget \
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
        \
        && apt-mark auto '.*' > /dev/null \
	    && apt-mark manual $savedAptMark \
	    && find /usr/local -type f -executable -execdir ldd '{}' ';' \
		        | awk '/=>/ { print $(NF-1) }' \
		        | sort -u \
		        | xargs -r dpkg-query --search \
		        | cut -d: -f1 \
		        | sort -u \
		        | xargs -r apt-mark manual \
	    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
	    && rm -rf /var/lib/apt/lists/* \
        && rm -rf /usr/src/pbc
