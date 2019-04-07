FROM mysql:5.6
ENV MYSQL_USER=root 
ENV MYSQL_ROOT_PASSWORD=root 
ENV MYSQL_DATABASE=mydb
ADD dump.sql /docker-entrypoint-initdb.d
