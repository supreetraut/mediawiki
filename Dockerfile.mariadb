FROM mariadb

ADD ./config.sql /docker-entrypoint-initdb/config.sql

ENV MYSQL_ROOT_PASSWORD=Passw0rd
ENV MYSQL_DATABASE=wikidatabase

EXPOSE 3306
