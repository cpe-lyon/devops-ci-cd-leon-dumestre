FROM postgres:14.1-alpine

ENV POSTGRES_DB=db

COPY CreateScheme.sql /docker-entrypoint-initdb.d/
COPY InsertData.sql /docker-entrypoint-initdb.d/
