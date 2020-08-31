FROM golang:alpine3.9 AS builder

ENV VERSION_GRAPHQLDOC=1.0.0

WORKDIR /go/src/github.com/mvochoa/graphqldoc

RUN apk add --no-cache --update git \
    && git clone https://gitlab.com/mvochoa/graphqldoc.git /go/src/github.com/mvochoa/graphqldoc \
    && git checkout $VERSION_GRAPHQLDOC

RUN go get github.com/go-bindata/go-bindata/... \
    && go-bindata -o assets.go template/ \
    && sed -i 's/package\ main/package\ graphqldoc/g' assets.go \
    && cd cmd/ \
    && GO111MODULE=on go install -v

FROM alpine:3.8
LABEL maintainer="mario@mvochoa.com"

COPY --from=builder /go/bin/cmd /usr/local/bin/graphqldoc
RUN chmod +x /usr/local/bin/graphqldoc

CMD ["graphqldoc"]
