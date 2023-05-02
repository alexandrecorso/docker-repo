FROM debian:bullseye-slim as builder

MAINTAINER Alexandre Corso <alexandre@acorso.fr>

RUN apt update && apt install -y \
  unzip \
  autoconf \
  bison \
  build-essential \
  curl \
  flex \
  libreadline-dev \
  libncurses5-dev \
  libssh-dev \
  m4 

RUN curl -O -L https://gitlab.nic.cz/labs/bird/-/archive/v2.13/bird-v2.13.zip
RUN unzip bird-v2.13.zip && mv bird-v2.13 bird-src

RUN cd bird-src && \
  autoconf && \
  autoheader && \
  ./configure --sysconfdir=/etc/bird/ --localstatedir=/var && \
  make


EXPOSE 179

FROM debian:bullseye-slim

RUN apt update && apt install -y \
  libssh-4 \
  libreadline8

RUN mkdir /etc/bird && mkdir -p /var/run

COPY --from=builder /bird-src/bird /usr/local/sbin/bird
COPY --from=builder /bird-src/birdc /usr/local/sbin/birdc
COPY --from=builder /bird-src/birdcl /usr/local/sbin/birdcl
COPY --from=builder /bird-src/doc/bird.conf.example /etc/bird/bird.conf

ENTRYPOINT [ "bird", "-d", "-s", "/var/run/bird.ctl"]
