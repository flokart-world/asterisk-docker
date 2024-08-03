FROM debian:bookworm-slim
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update

ARG ASTERISK_VERSION=20.9.1
ADD https://github.com/asterisk/asterisk/releases/download/$ASTERISK_VERSION/asterisk-$ASTERISK_VERSION.tar.gz /tmp
WORKDIR /tmp
RUN mkdir src
RUN tar zxf asterisk-$ASTERISK_VERSION.tar.gz -C src
WORKDIR /tmp/src/asterisk-$ASTERISK_VERSION
RUN contrib/scripts/install_prereq install
RUN ./configure
RUN make menuselect.makeopts

ARG ASTERISK_OPTIONS=""
RUN menuselect/menuselect $ASTERISK_OPTIONS
RUN make
RUN make install

ARG ASTERISK_UID=2000
ARG ASTERISK_GID=2000
RUN groupadd -g $ASTERISK_GID asterisk
RUN useradd -u $ASTERISK_UID -g asterisk -s /bin/bash -m asterisk
RUN chown -R asterisk:asterisk \
        /var/lib/asterisk \
        /var/log/asterisk \
        /var/spool/asterisk \
        /var/run/asterisk
WORKDIR /home/asterisk
USER asterisk
CMD ["/usr/sbin/asterisk", "-f"]
