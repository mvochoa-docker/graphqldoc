FROM golang:alpine3.12 AS builder

ENV VERSION_GRAPHQLDOC=2.2.0

WORKDIR /go/src/github.com/mvochoa/graphqldoc

RUN apk add --no-cache --update git \
    && git clone https://gitlab.com/mvochoa/graphqldoc.git /go/src/github.com/mvochoa/graphqldoc \
    && git checkout $VERSION_GRAPHQLDOC

RUN go get github.com/markbates/pkger/cmd/pkger \
    && pkger -include $(pwd)/template \
    && cd cmd/ \
    && go install -v

FROM alpine:3.12
LABEL maintainer="mario@mvochoa.com"

COPY --from=builder /go/bin/cmd /usr/local/bin/graphqldoc
RUN chmod +x /usr/local/bin/graphqldoc

CMD ["graphqldoc"]
