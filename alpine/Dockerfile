ARG GO_VERSION=1.19
FROM golang:$GO_VERSION-alpine as builder

ARG REWINGED_VERSION=0.3.0
ARG REWINGED_DOWNLOAD_HASH=994ee46335c8de233cbaeba51d16593b3eb15b3d14b5b1c6c282dd78d3e43e9f

WORKDIR $GOPATH/src/jantari/rewinged/

RUN apk add --update --no-cache curl \
    && curl --location --fail -o rewinged.tar.gz \
    "https://github.com/jantari/rewinged/archive/refs/tags/v${REWINGED_VERSION}.tar.gz" \
    && echo "$REWINGED_DOWNLOAD_HASH  rewinged.tar.gz" | sha256sum -c - \
    && tar -xz --strip-components=1 -f rewinged.tar.gz \
    && go build -o rewinged \
        -ldflags "-X main.releaseMode=true -X main.version=${REWINGED_VERSION} -X main.commit=unknown -X 'main.compileTime=$(date)'" \
        .

FROM alpine

COPY --from=builder /go/src/jantari/rewinged/rewinged /usr/local/bin

RUN mkdir /srv/rewinged \
    && mkdir /srv/rewinged/packages \
    && addgroup -g 1000 rewinged \
    && adduser -D -G rewinged -u 1000 -h /srv/rewinged rewinged

ENTRYPOINT ["/usr/local/bin/rewinged"]
CMD ["-listen", ":8080", "-manifestPath", "/srv/rewinged/packages"]

USER rewinged
WORKDIR /srv/rewinged
