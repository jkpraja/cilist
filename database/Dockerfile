FROM mysql:5.7

ENV MYSQL_ROOT_PASSWORD=adminadmin1
ENV MYSQL_DATABASE=people
ENV MYSQL_USER=people
ENV MYSQL_PASSWORD=people

COPY ./crud_db.sql /docker-entrypoint-initdb.d/crud_db.sql