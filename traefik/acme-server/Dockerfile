FROM golang as minica

RUN go get github.com/jsha/minica

COPY ca/ /ca

WORKDIR /ca

RUN minica -ca-cert pebble.minica.pem \
       -ca-key pebble.minica.key.pem \
       -domains localhost,pebble \
       -ip-addresses 127.0.0.1

FROM golang

ENV PEBBLE_WFE_NONCEREJECT=0
ENV PEBBLE_VA_NOSLEEP=1

RUN go get github.com/letsencrypt/pebble/... && \
    cd $GOPATH/src/github.com/letsencrypt/pebble && \
    go install ./...

COPY --from=minica /ca/localhost /config
COPY pebble-config.json /config

ENTRYPOINT ["pebble"]
CMD ["-config", "/config/pebble-config.json"]

EXPOSE 443
