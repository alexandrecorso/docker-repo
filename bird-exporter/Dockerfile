FROM golang:latest as builder

MAINTAINER Alexandre Corso <alexandre@acorso.fr>

RUN apt update && apt install -y unzip
RUN curl -L https://github.com/czerwonk/bird_exporter/archive/refs/tags/1.4.2.zip --output bird_exporter.zip
RUN unzip bird_exporter.zip -d extract_dir && mv extract_dir/* bird_exporter

WORKDIR bird_exporter

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o /go/bin/bird_exporter

FROM alpine:latest

RUN apk --no-cache add ca-certificates bash

COPY --from=builder /go/bin/bird_exporter /

EXPOSE 9324

ENTRYPOINT ["/bird_exporter", "-format.new", "-bird.v2"]
