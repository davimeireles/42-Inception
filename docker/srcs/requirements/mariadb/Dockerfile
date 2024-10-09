FROM debian:bullseye

RUN apt update -y \
&& apt upgrade -y \
&& apt install -y mariadb-server

COPY conf/50-server.cnf /etc/mysql/mariadb.conf.d/.

RUN mkdir /run/mysqld

COPY db.sh /db.sh

RUN chmod +x ./db.sh

ENTRYPOINT ["/db.sh"]