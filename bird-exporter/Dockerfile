FROM golang:latest as builder

MAINTAINER Alexandre Corso <alexandre@acorso.fr>

RUN apt install -y git

RUN git clone https://github.com/czerwonk/bird_exporter.git

WORKDIR bird_exporter

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o /go/bin/bird_exporter

FROM alpine:latest

RUN apk --no-cache add ca-certificates bash

COPY --from=builder /go/bin/bird_exporter /

EXPOSE 9324

ENTRYPOINT ["/bird_exporter", "-format.new", "-bird.v2"]