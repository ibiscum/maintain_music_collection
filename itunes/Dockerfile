FROM perl:latest

USER root
WORKDIR /app

RUN groupadd -r itunesdb -g 901 && \
useradd -u 901 -r -g itunesdb -m -d /home/itunesdb -s /sbin/nologin -c "itunesdb user" itunesdb && \
chmod 755 /app

ADD itunes_db_cron /etc/cron.d/itunes_db_cron
RUN chmod 0644 /etc/cron.d/itunes_db_cron

RUN touch /var/log/itunes_db.log
RUN mkdir /mnt/music

COPY itunes_db.pl itunes_db.conf /app/
RUN chmod 0644 /app/itunes_db.pl

ARG DEBIAN_FRONTEND=noninteractive
RUN set -ex; \
    apt-get update && apt-get dist-upgrade -y && \
    apt-get -y install cron && \
    apt-get -y --no-install-recommends install && \
    apt-get -y autoremove && apt-get clean
RUN perl -MCPAN -e 'install Bundle::DBI'
RUN cpanm Data::Dumper Config::Tiny DateTime DateTime::Format::ISO8601
RUN rm -rf /var/lib/apt/lists/*; rm -rf /var/lib/apt/lists/*; \
    rm -rf /tmp/*; rm -rf /root/.cpanm/

USER itunesdb:itunesdb
CMD cron && tail -f /var/log/itunes_db.log
