FROM alpine:3.13.5

RUN apk add --no-cache \
  go \
  python3 \
  py3-pip \
  nodejs \
  npm

ENV GO111MODULE="on"

ARG VERSION

RUN go get github.com/mattn/efm-langserver@v${VERSION}

RUN python3 -m pip install flake8
RUN apk add --no-cache --virtual .build-deps gcc musl-dev python3-dev\
  && python3 -m pip install mypy \
  && apk del .build-deps gcc musl-dev

CMD [ "/root/go/bin/efm-langserver" ]
