# Postgres container

Create postgre instance

```
docker run --name some-postgres -e POSTGRES_PASSWORD=1234 -d postgres

docker run --name some-postgres -e POSTGRES_PASSWORD=1234 -d -p 5432:5432 postgres
```
