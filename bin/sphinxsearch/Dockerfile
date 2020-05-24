# Dockerfile for Sphinx SE
# https://hub.docker.com/_/alpine/
FROM alpine:3.8

# https://sphinxsearch.com/blog/
ENV SPHINX_VERSION 3.2.1-f152e0b

# install dependencies
RUN apk add --no-cache mysql-client \
	mariadb-connector-c-dev \
	postgresql-dev \
	wget

# set up and expose directories
RUN mkdir -pv /opt/sphinx/log /opt/sphinx/conf/ /opt/sphinx/index/

COPY sphinx.conf /opt/sphinx/conf/

# http://sphinxsearch.com/files/sphinx-3.2.1-f152e0b-linux-amd64-musl.tar.gz
RUN wget http://sphinxsearch.com/files/sphinx-${SPHINX_VERSION}-linux-amd64-musl.tar.gz -O /tmp/sphinxsearch.tar.gz
RUN cd /opt/sphinx && tar -xf /tmp/sphinxsearch.tar.gz
RUN rm /tmp/sphinxsearch.tar.gz

# point to sphinx binaries
ENV PATH "${PATH}:/opt/sphinx/sphinx-3.2.1/bin"

# redirect logs to stdout
RUN ln -sv /dev/stdout /opt/sphinx/log/query.log
RUN ln -sv /dev/stdout /opt/sphinx/log/searchd.log

# RUN indexer --rotate --all --config /opt/sphinx/conf/sphinx.conf && searchd --nodetach --config /opt/sphinx/conf/sphinx.conf
COPY sphinxindex/ /opt/sphinx/index/
CMD searchd --nodetach --config /opt/sphinx/conf/sphinx.conf
