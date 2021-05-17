FROM alpine:3.13.5

RUN apk add --no-cache \
  go \
  python3 \
  py3-pip \
  nodejs \
  npm \
  libc-dev readline readline-dev gcc g++ make wget unzip git cmake outils-md5

ENV GO111MODULE="on"

ARG VERSION

RUN go get github.com/mattn/efm-langserver@v${VERSION}

RUN python3 -m pip install flake8
RUN apk add --no-cache --virtual .build-deps gcc musl-dev python3-dev\
  && python3 -m pip install mypy \
  && apk del .build-deps gcc musl-dev

# install lua
ARG LUA_VER="5.1.5"
ARG LUA_ROCKS_VER="3.7.0"
RUN cd /tmp \
    && wget https://www.lua.org/ftp/lua-${LUA_VER}.tar.gz \
    && tar zxf lua-${LUA_VER}.tar.gz \
    && cd lua-${LUA_VER} \
    && make linux install \
    && cd /tmp \
    && rm -rf /tmp/*
# install luarocks
RUN cd /tmp \
    && wget https://luarocks.org/releases/luarocks-${LUA_ROCKS_VER}.tar.gz \
    && tar zxf luarocks-${LUA_ROCKS_VER}.tar.gz \
    && cd luarocks-${LUA_ROCKS_VER} \
    && ./configure \
    && make build \
    && make install \
    && cd /tmp \
    && rm -rf /tmp/*

RUN luarocks install --server=https://luarocks.org/dev luaformatter
RUN luarocks install luacheck

CMD [ "/root/go/bin/efm-langserver" ]
