FROM debian:buster-slim as builder

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

RUN curl -O -L https://gitlab.labs.nic.cz/labs/bird/-/archive/v2.0.8/bird-v2.0.8.zip
RUN unzip bird-v2.0.8.zip 

RUN cd bird-v2.0.8 && \
  autoconf && \
  autoheader && \
  ./configure --sysconfdir=/etc/bird/ --localstatedir=/var && \
  make


EXPOSE 179

FROM debian:buster-slim

RUN apt update && apt install -y \
  libssh-4 \
  libreadline7 

RUN mkdir /etc/bird && mkdir -p /var/run

COPY --from=builder /bird-v2.0.8/bird /usr/local/sbin/bird
COPY --from=builder /bird-v2.0.8/birdc /usr/local/sbin/birdc
COPY --from=builder /bird-v2.0.8/birdcl /usr/local/sbin/birdcl
COPY --from=builder /bird-v2.0.8/doc/bird.conf.example /etc/bird/bird.conf

ENTRYPOINT [ "bird", "-d" ]
