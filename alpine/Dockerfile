ARG GO_VERSION=1.21
FROM golang:$GO_VERSION-alpine as builder

ARG REWINGED_VERSION=0.7.1
ARG REWINGED_DOWNLOAD_HASH=4eceecab1d857d1f0c6ce6cdf16890237f47bd1e646a2f6b6f460fe11c8211a7

WORKDIR $GOPATH/src/jantari/rewinged/

RUN apk add --update --no-cache curl \
    && curl --location --fail -o rewinged.tar.gz \
    "https://api.github.com/repos/jantari/rewinged/tarball/v${REWINGED_VERSION}" \
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
