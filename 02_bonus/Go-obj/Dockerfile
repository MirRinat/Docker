FROM golang:1.10.2-alpine3.7

RUN apk update; apk upgrade

RUN apk add git vim

RUN go get -u github.com/golang/dep/cmd/dep

WORKDIR /go/src/app

VOLUME ["/go/src/app"]

#docker run -it 03
#docker run -it 03 /bin/sh